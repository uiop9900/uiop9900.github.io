---
title: Jooq와 QueryDsl
date: 2022-12-25 14:33:13
categories: [DB]
tags: [db, jooq, querydsl]  
---

# 데이터 베이스란?
데이터들의 집합을 의미한다.
- ### DBMS (Database Management system)
데이터베이스를 관리하고 운영하는 소프트웨어로 MySQL, 오라클, SQL 서버, Maria DB 등이 존재한다.
이 안에 계층형, 망형, 관계형, 객체지향형, 객체관계형으로 분류가 되고 가장 많이 쓰이는 것은 관계형 DBMS 이다.

- ## ORM (Object Relational Mapping)
객체와 관계형 데이터베이스의 데이터를 자동으로 매핑(연결)해주는 것을 의미한다. 
ORM은 독립적이기 때문에 
->  객체지향적인 코드 가능, 직관적인 코드, 가독성 좋음, 재사용 및 유지보수에 좋다.

---

- ## Jooq (Java Object Oriented Querying)
자바로 코드를 작성할 수 있는 데이터베이스 인터페이스, dsl 클래스를 사용해서 쿼리를 작성, 상단에서 DSLContext 선언을 해서 Autowired 해야한다. 
-> orm은 아니고 컴파일 시점에서 오류 잡을 수 있다.

```java
public class MemeberRepositoryJooqImpl implements MemeberRepositoryJooq {
	private final DSLContext dslContext;

	public MemeberRepositoryJooqImpl(DSLContext dslContext) {
		this.dslContext =  dslContext;
	}


}

```

jooq가 더 활용성이 좋다.
조회는 ㅁjooq, jooq는 바로 나가고 quetdsl은 변환해서 나간다.

- ### QueryDSL (Java Object Oriented Querying)
queryDsl은 jpa 의 확장개념으로 java코드로 JPQL을 작성할 수 있는 라이브러리이다. jpa가 관리하기때문에 @TranX안에서 알아서 flush가 되지만 


- ### Jooq QueryDSL 비교

jooq는 다른 영역이기때문에 flush를 꼭 해줘야 한다

jooq는 쿼리문을 더 손쉽게 짤수있는 것으로 컴파일 시점에 에러를 잡아주기때문에 queryDsl도 해주지 못하는 복잡한 쿼리를 작성할때 사용한다.

커밋시점에 저장이 된다.
