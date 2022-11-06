---
title: Exception
date: 2022-11-06 17:18:00
categories: [강의]
tags: [til, exception]  
---

# Exception

##### API에 따라 세분화된 예외를 내려줘야한다. 단순히 4XX, 5XX 에러가 아니라 어느 Controller에서 어떤 예외가 발생했는지 세밀하게 예외를 내려줘야하는데 기존의 방식들(servlet 방식)은 그렇게 사용하기 힘들다.

## @ExceptionHandler 
: Spring에서 제공하는 예외처리 어노테이션 , 예외발생시 우선순위가 제일 높게 실행된다.

예외처리 시, @ExceptionHandler 를 사용하게 되면 아래의 흐름으로 실행된다.

에러발생 -> ExceptionHandler -> ExceptionHandlerExceptionResolver -> @ExceptionHandler가 있는지 찾음 -> 존재하면 호출

호출을 통해 예외처리가 되면 정상흐름인 http 200으로 반환한다.
그렇기때문에 HttpHeader에 에러를 넣으려면 @ResponseStatus(HttpStatus.에러)를 넣어줘야 한다.

```java
// 여기서 흐름이 끝난다 -> Servlet Container까지 갔다가 다시 돌아오는 흐름이 아님

@ResponseStatus(HttpStatus.BAD_REQUEST) // 정상적 return 이 아니라 에러를 만들고 싶으면 붙인다.
@ExceptionHandler(IllegalArgumentException.class)

public ErrorResult illegalExHandler(IllegalArgumentException e) {
	log.error("[exceptionHanlder] ex", e);
	// 정상으로 return 한다. -> http 상태코드 200
	return new ErrorResult("BAD", e.getMessage()); 

}
```

1) Controller안에서 @ExceptionHandler 를 통해 예외처리를 하는 경우, 그 Controller안에서 발생한 에러만 처리한다.
2) 해당 에러뿐만 아니라 자식 에러까지 잡아낸다.
3) 일반 로직과 같이 섞여있기 때문에 가독성이 떨어진다.

위와 같은 사항(1번, 3번)들로 인해 @RestControllerAdvice 을 이용해 예외처리를 위한 로직을 모아둔다.

## @RestControllerAdvice
@RestController와 @Controller 처럼 상단에서 선언을 해서 예외처리를 위한 클래스를 만든다.

```java
@Slf4j
@RestControllerAdvice(basePackages = "hello.exception.api") // package로 대상 지정
public class ExControllerAdvice {
	// 기존에 @RestContoller에 같이 있던 예외처리 로직을 따로 가져온다.
}
```

따로 설정을 하지 않을 경우 글로벌로 설정되며 예시와 같이 package로 선언할 수 도, 혹은 Controller를 지정해 선언할 수 도 있다.


출처: 김영한, "스프링 MVC 2편 - 백엔드 웹 개발 활용 기술", 인프런