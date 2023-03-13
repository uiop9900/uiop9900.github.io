---
title: Transactional isolation(격리수준)
date: 2023-3-10 17:18:00
categories: [TIL]
tags: []  
---


코드를 짜게 되면 `@Transactional` 는 필수적으로 사용하게 되는데, 여러 쓰레드가 한꺼번에 Transaction을 물게 될 경우, 데이터가 서로 안 맞는 상황이 생길 수 있다.
이런 경우, *데이터의 정합성* 이 깨지기 때문에 `@Transactional` 은 isolation(격리수준) 설정을 통해서 데이터들의 정합성을 맞춘다.

*데이터의 정합성: 데이터끼리 서로 모슨 없이 일치해야하는 것*

- Transaction과 `@Transactional` 은 동일하지 않다.
---

# `@Transactional`  격리수준 단계
-   READ_UNCOMMITED (level 0)
	- 커밋되지 않은 것까지 읽기
-   READ_COMMITED (level 1)
	- 커밋된거 이상만 읽기
-   REPEATABLE_READ (level 2)
	- 동일 트랜잭션 내에서는 동일 데이터를 보여준다.
-   SERIALIZABLE (level 3)
	- 다른 트랙잭션이 사용하고 있으면 다른 트랜잭션은 접근 불가.

레벨이 높을수록 데이터의  정합성은 높지만, 그만큼 속도는 느려진다.
(레벨이 높을수록 함부로 db에 접근하지 못하며 그렇기에 속도가 느리다.)
<br>
이 단계들 중에서 비교해보고 싶은건   `READ_COMMITED` (level 1)과   `REPEATABLE_READ` (level 2)이다.

# 기본(default) 격리수준

기본적으로 `@Transactional` 을 선언하면 아래와 같은 격리수준을 사용한다.

![Transactional Default isolation](/assets/img/Trans_default.png)

이 말은 DB에 따라 다른 격리수준을 디폴트로 사용한다는 뜻인데 MYSQL 의 경우, 아래와 같이 `REPEATABLE_READ` 가 기본격리수준이다.

![Mysql Default isolation](/assets/img/mysql_default.png)

---

# 격리수준 테스트

아래와 같이 `value = 8000` 인 point db 가 있다. 이 db로 트랜잭션 격리수준에 따라 세션들의 값이 어떻게 다른게 보이는지 확인해보자.

![test db](/assets/img/test.png)



## REPEATABLE_READ 

그럼 먼저 default 설정인 `REPEATABLE_READ` 로 트랜잭션을 실험해보자.

![REPEATABLE-READ](/assets/img/REPEATABLE-READ.jpg)

	세션1 과 세션2에서 Tx를 시작한다.
	세션1에서 값을 update 한다.
	세션2에서 update 된 값을 불러온다.

<b>세션2는 세션1이 값을 변경해서 commit 하여도 변경된 값을 가지고 오지 못하고, Tx 시작시에 가지고 왔던 기존 값을 물고 있다. </b>


## READ_COMMITED


![READ-COMMITTED](/assets/img/READ-COMMITTED.jpg)

	세션1 과 세션2에서 Tx를 시작한다.
	세션1에서 값을 update 한다.
	세션2에서 update 된 값을 불러온다.

<b>세션1에서 변경된 값을 commit 하면 세션2는 변경된 값을 가지고 온다. </b> <br>

위의 결과와 같이, `REPEATABLE_READ` 는 다른 트랜잭션에서 commit 이 되어도 기존의 값을 물고 있고 `READ-UNCOMMITTED` 는 commit이 되면 변경된 값을 가지고 와서 확인할 수 있다.

<br>

#### TMI 새로운 상식
- 쿼리를 통해서 트랜잭션을 db에 바로 걸 수 있다.
``` Mysql
SELECT @@`Transaction_isolation`;  # 현재 격리수준 확인
SET @@session.transaction_isolation = 'READ-COMMITTED'; # 격리수준 변경하기
```

- 한 세션은 하나의 DB와의 커넥션으로 이해할 수 있다. 
	- 인텔리제이에서는 db console을 키면 된다. 
	- 하지만 하나의 콘솔이 하나의 세션을 의미하는 것은 아니다. = 한 세션에 여러개의 콘솔을 사용할 수 있다.