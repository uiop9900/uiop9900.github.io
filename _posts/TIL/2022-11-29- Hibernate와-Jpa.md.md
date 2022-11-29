---
title: Hibernate와 Jpa
date: 2022-11-29 17:18:00
categories: [TIL]
tags: [jpa, spring]  
---

# JDBC
: Java DataBase Connectivity
Java 프로그램이 데이터베이스와 연동할 수 있게 해주는 표준 API 이며
JDK 에 포함되어 있으며 *DBMS(DataBase Management System)*와 상관없이 사용할 수 있는 API이다.

즉, 어떤 DB 프로그램을 쓰더라도 다 접근이 가능하다.

*DBMS :데이터베이스에 접근하여 데이터베이스를 관리하는 프로그램으로 Oracle, MySQL, MARIA DB 등등 이 있다.*


# JPA
Java Application에서 관계형 데이터베이스를 사용하는 방식으로 정의한 인터페이스이다.
인터페이스라는 건, hibernate 가 아니라 다른 구현체를 사용해도 JPA는 사용할 수 있다는 것을 의미한다. 
(하지만 hibernate가 워낙 구현이 잘 되어있기에 주로 hibernate를 사용한다.)

# Hibernate
JPA 구현체로 ORM FrameWork로 칭한다.
HQL(Hibernate Query Language) 라고 하는 강력한 쿼리언어가 있다.


jpa를 사용하려고 hubernate를 사용함 -> 그리고 DB를 연결한다 -> db에 연결하려면 jdbc와 연결해야한다.
