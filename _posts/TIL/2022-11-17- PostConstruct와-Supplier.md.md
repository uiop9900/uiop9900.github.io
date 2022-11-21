---
title: @PostConstruct와 @Supplier
date: 2022-11-17 17:18:00
categories: [TIL]
tags: [til, spring]  
---

# Annotation
Spring을 사용하다보면 다양한 Annotaion을 사용하게 되는데 회사코드에서 처음보는 Annotation을 만나서 어떤 기능을 하는지, 왜 사용하는 건지 정리해보았다.

## @PostConstruct
: DI(의존성을 주입) 한 이후에 실행되며 class 가 service로 올라가기 전에 실현된다.
한 클래스에서 하나의 메소드에만 사용이 가능하다.

### 사용이유와 예시
@Value를 통해 설정값을 받고, 그 값을 주입받은 이후에 변수에 값을 넣어야하기 때문에 @PostConstruct 를 사용해서 DI가 일어난 다음에 메소드가 실행되게끔 한다.

- 예시

```java
@Getter
@Configuration
public class Configuration { // 환경변수 받음

	@Value("${apiGateway.username}")
	private String username;

	@Value("${apiGateway.password}")
	private String password;
	
}
```

```java
	private final Configuration configuration;

	@PostConstruct
	private void postConstruct() { // 환경변수 주입후 변수에 값을 담는다.
		username = configuration.getUsername();
		password = configuration.getPassword();
	}
```

## @Supplier<>
: 매개변수를 받지않고 단순히 무언가를 반환한다. 말그대로 supply(공급)한다.

```java
@FunctionalInterface  
public interface Supplier<T> {  
  
    /**  
     * Gets a result.     *     * @return a result  
     */    T get();  
}
```

Supplier 의 내부코드를 보면 값을 리턴하는 로직이 존재한다.

- 예시
```java
public class SupplierExample {

	public static void main(String[] args) { 
		
		Supplier<String> helloSupplier = () -> "Hello "; 
		System.out.println(helloSupplier.get() + "World"); 
		
	}
}
```

결과값: Hello World