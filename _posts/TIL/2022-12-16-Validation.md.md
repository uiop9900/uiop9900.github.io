---
title: Annotation을 이용한 Validation
date: 2022-12-16 17:18:00
categories: [TIL]
tags: [jpa, gradle]  
---

환경: spring boot + gradle + java

회사에서 validation 처리를 하게 되었다.
spring의 경우 손쉽게 annotation을 통해 validation 처리가 가능한데 그 방법을 알아보자.

## 기존 코드

```java
public Response validationTest(final ValidationInput input) {
	// service 로직
}

@Data  
@AllArgsConstructor  
@NoArgsConstructor  
@Builder(toBuilder = true)  
public class ValidationInput {
	private String name;
	private Integer age;
	private Long grade;
	private Boolean passOrFail;
	private LocalDate createdAt;
	private LocalDate updatedAt;
	private TeacherInfo teacherInfo
}

@Data  
@AllArgsConstructor  
@NoArgsConstructor  
@Builder(toBuilder = true)  
public class TeacherInfo {
	private String teacherName;
	private String subject;
}

```


## gradle 설정 추가
```java
// validation을 위한 의존성 추가
implementation group: 'org.springframework.boot', name: 'spring-boot-starter-validation'
```

```java
public Response validationTest(@Valid final ValidationInput input) {
	// service 로직
}

@Data  
@AllArgsConstructor  
@NoArgsConstructor  
@Builder(toBuilder = true)  
public class ValidationInput {
	@Size(max = 20, message = "이름 글자수(20)가 초과되었습니다.")
	private String name;
	private Integer age;
	@NotNull
	private Long grade;
	private Boolean passOrFail;
	private LocalDate createdAt;
	private LocalDate updatedAt;
	@Valid
	private TeacherInfo teacherInfo
}

@Data  
@AllArgsConstructor  
@NoArgsConstructor  
@Builder(toBuilder = true)  
public class TeacherInfo {
	private String teacherName;
	private String subject;
}

```


## Validated 와는 뭐가 다른거죠?
Validated 는 A상황일때는 Validation을 체크하고 B상황에서는 Validation을 체크하고 싶지 않을 때 상황마다의 그룹을 만들어서 Validation Check를 할때 사용하는 annotation이다. 궁금하다면 검색해보자.

validated :  org.springframework.validation.annotation
valid: javax.validation
Validated 가 Valid의 기능을 포함하고 있다. 