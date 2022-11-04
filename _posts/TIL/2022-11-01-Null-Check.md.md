---
title: Null Check
date: 2022-11-01 17:18:00
categories: [TIL]
tags: [til]  
---

# 11.01
## Null 체크를 하는 방법 (StringUtils, ObjectUtils)
null은 값이 없는 것을 의미하며 ""(공백) 과는 다르다.

### 1) StringUtils
- StringUtils.isEmpty : null 이거나 공백("") 인 경우, true를 반환한다.

```java

	public static boolean isEmpty(@Nullable Object str) {
		return (str == null || "".equals(str));
	}

```

- StringUtils.isBlank : 문자가 없으면 true.

```java
  StringUtils.isBlank(null)      = true
  StringUtils.isBlank("")        = true
  StringUtils.isBlank(" ")       = true
  StringUtils.isBlank("bob")     = false
  StringUtils.isBlank("  bob  ") = false
```

### isEmpty보다 isBlank가 더 타이트하게 null을 체크한다.


### 2) ObjectUtils
- == null : 객체가 null인지 판단한다.

```java
    if (Object == null) {
        ...
    }


```

- ObjectUtils.isNull : 객체가 null인지 판단한다.
```java
public static boolean isNull(Object obj) {
        return obj == null;
    }
```

== 연산자와 동일한 역할을 하기 때문에 둘 중 아무거나 써도 무관하다.

----
## HandlerExceptionResolver 
HanlderExceptionResolver : 예외를 다른 곳에서 터뜨리려고 할때 사용 (500에러 -> 400에러로)
실제 에러가 난 경우 postHanlder 호출되지 않는다.
에러가 난 경우, Dispather Servlet에서 ExceptionResolver가 호출되고 예외 처리를 시도한다. 
예외가 해결이 되어도 postHanlder 호출되지 않는다.