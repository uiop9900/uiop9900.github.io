---
title: Message Queue
date: 2023-2-2 17:18:00
categories: [TIL]
tags: []  
---

사이드 프로젝트에 MQ를 사용해보고 싶은데, 정작 MQ가 무엇인지 제대로 이해하지 못하고 있는 거 같아서 정리해보았다.
메시지 큐를 이해하기 위해서는 먼저 IPC에 대해 알아야 한다. <br>

## IPC (Inter Process Communication)
- 개념

```
프로세스 간 의사소통 하는 것을 의미하며, 서로 다른 프로세스가 데이터를 주고 받는다는 건 동시에 접근이 가능한 메모리(= 프로세스들이 공유하는 메모리)가 필요하다는 뜻이다.
```

- 종류
	- 공유 메모리
	- 파이프
	- 소켓
	- 메시지 큐
	- 메모리 맵
	- RPC(Remote Procedure Call)

일단 메세지 큐에 대해서 집중적으로 알아보자!

---


#  Message Queue

### 개념
- 프로세스 혹은 프로그램 간에 데이터를 교환할 때 사용하는 통신 방법 중 하나이다.
- 메시지 지향 미들웨어 (Message Oriented Middleware)를 구현한 시스템이다.
	- 메세지 지향 미들웨어: 비동기 메시지를 사용하는 응용 프로그램들 사이에서 데이터를 송수신하는 것을 말한다. 
		- 메세지: 요청, 응답, 오류 메세지 혹은 단순한 정보 등의 작은 데이터
- 큐는 메세지를 임시로 저장하는 간단한 버퍼의 형태이다.
- 거대한 양의 데이터를 다룰일이 생길 경우 메시지 큐가 버퍼 역할을 수행한다.


### 동작 순서

![Foo](/assets/img/mq_image.png)


1) 송신자(생산자)가 메세지를 송신하면 메세지가 메세지 큐에 추가된다.
2) 큐에 저장된 메세지는 수신자(소비자)가 메세지를 검색해서 사용하고 수행할때까지 메세지 큐에 저장되어 있다.
3) 각 메세지는 하나의 수신자(소비자)에 의해 처리가 가능하다. 
큐를 이용하는 방식 = 일대일 통신


### 특징과 사용고려
- 메세지 큐는 송신자가 언제 메세지를 가져가서 처리할지는 보장하지는 않지만
- 언젠가는 처리될 것을 보장한다.
- 그렇기때문에 실패하면 안되는 중요한 핵심작업 보다는 부가적인 기능일때 많이 사용한다.
	- 예시) 이메일 전송


### 메시지 큐의 장점
1. 비동기
	- 큐에 넣어둔 후 나중에 처리하기 때문에 대용량으로 요청이 들어올 시 더 효율적이다.
2. 낮은 결합도
	- 생산자 서비스와 소비자 서비스가 독립적이다.
3. 확장성
4. 탄력성
	- 만약 소비자 서비스가 다운되더라도 메시지 큐에 메시지들이 담겨있기 때문에 소비자 서비스가 다시 시작되면 다시 메시지 처리가 가능하다.
5. 보장성
	- 메시지 큐에 있는 메시지들은 결국 소비자 서비스에게 전달된다는 걸 보장한다.


### 메시지 큐의 오픈소스
RabbitMQ, Kafka, ActiveMQ