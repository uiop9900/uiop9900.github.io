---
title: OpenFeign으로 인한 순환참조 에러
date: 2023-3-2 17:18:00
categories: [Error]
tags: [til, openFeign]  
---

직장동료가 테스트 코드를 작성하려 하니 `OpenFeign`(내가 작성한 코드)에서 순환참조가 일어나 테스트 코드가 제대로 돌아가지 않는다고 한다. 어서 해결해보자.

---
환경: spring boot + GraphQL <br>
## OpenFeign 코드

```java
@Service  
@RequiredArgsConstructor  
public class OpenFeignServiceImpl implements OpenFeignService {  
	
	private final OpenFeign sendOpenFeign;  // 아래의 OpenFeign interface를 참조
  
	@Override  
	public Response getResponse(final Input input) {  
		return sendOpenFeign.getResponse(input);  
	}  
}
```

```java
@FeignClient(name = "Client", url = "${client.url}", configuration = OpenFeignConfig.class)  
public interface OpenFeign {  
  
	@GetMapping(value = "/api/v1/test/{input}", consumes = "application/json")  
	Response getResponse(@PathVariable("input") String input);  

}
```

코드를  확인해보면, <br>
`OpenFeignServiceImpl` 에서 `OpenFeign` 를 주입받아서 `OpenFeign` 의 getResponse 메소드를 사용하는 형태이다.

## 에러 화면

![circular reference error](/assets/img/openFeign_error.png)

나는 `mvcResourceUrlProvider`를 참조하지 않았는데, 참조되어서 저 `mvcResourceUrlProvider`가 `Graphql Servlet`을 참조하고 ... 꼬리를 물어서 순환참조가 되었다.

## 순환참조가 일어나는 이유

`OpenFeign`은 RESTFul Service를 쉽게 부르기 위한 라이브러리이고, `mvcResourceUrlProvider`은 정적리소스에 url을 제공하는 스프링 빈이다.  
`OpenFeign`의 value인  “/api/v1/test/{input}” 가 정적인 url이라서 mvcResourceUrlProvider을 부르는 것으로 판단했다.

### 순환참조를 끊어내는 방법
순환참조를 끊어내기 위해 빈을 늦게 생성해주면 되는데, 2가지의 옵션이 있다.

- @Lazy 
- ObjectProvider

하지만 실질적으로 `@Lazy`는 순환참조를 제대로 끊어내지 못하는데, 그 이유의 둘의 차이에 있다.
`@Lazy`는 단순히 만드는 걸 늦게 만들게 하는 어노테이션이고, `ObjectProvider`는 실제 사용될때 생성하게 만들어주는 빈이기 때문에 `ObjectProvider` 를 사용해야 순환참조가 끊긴다.

```java
@Service  
@RequiredArgsConstructor  
public class OpenFeignServiceImpl implements OpenFeignService {  
	
	private final ObjectProvider<OpenFeign> sendOpenFeign;  // ObjectProvider로 한번 감싼서 참조한다.
  
	@Override  
	public Response getResponse(final Input input) {  
		return sendOpenFeign.getObject.getResponse(input);  
	}  
}
```

결국 `ObjectProvider` 를 사용해서 문제는 해결되었다...
욕 먹어서 배부르다.. 하하

---

## 번외

#### 근데 왜 `mvcResourceUrlProvider` 는 `graphqlServletRegistrationBean`를 왜 참조하지?

`mvcResourceUrlProvider`는 Spring Boot에서 제공하는 웹 관련 기능의 필수적인 부분인 GraphQL 요청을 처리하는 데 필요하기 때문에 간접적으로 `graphqlServletRegistrationBean`을 참조합니다.

#### 그럼 `gql` 쓰면 무조건 `ObjectProvider` 써야 하나?
`graphql`과 spring boot 에서 제공하는 웹 관련 기능을 같이 사용하게 되면 `ObjectProvider` 혹은 `다른 lazy` 를 권장합니다.

-   참조 : [https://github.com/spring-cloud/spring-cloud-openfeign/issues/321](https://github.com/spring-cloud/spring-cloud-openfeign/issues/321)
-   ChatGPT..