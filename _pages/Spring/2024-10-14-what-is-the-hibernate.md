---
title: 🍃[Spring] Hibernate란 무엇일까요?
tags:
    - Spring
    - Framework
date: "2024-10-14"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Hibernate란 무엇일까요?

- **Hibernate는** 자바에서 사용되는 **ORM(Object-Relational Mapping)** 프레임워크로, 관계형 데이터베이스(Relational Database, RDB)와 객체 지향 프로그래밍(Object-Oriented Programming, OOP) 간의 불일치를 해결해 주는 도구입니다.
- 객체 지향적인 자바 애플리케이션의 데이터를 관계형 데이터베이스(Relational Database, RDB)의 테이블에 자동으로 매핑해주는 역할을 합니다.

## 1️⃣ 주요 특징.

### 1️⃣ ORM(Object-Relational Mapping)
- 데이터베이스 테이블과 자바 클래스 간의 매핑을 통해, SQL(Structured Query Language) 문을 직접 작성하지 않고도 자바 객체로 데이터베이스 작업을 처리할 수 있습니다.
- 예를 들어, 데이터베이스의 테이블 행(Row)을 자바 객체로 변환하고, 자바 객체를 테이블의 행(Row)으로 변환하는 과정이 자동으로 이루어집니다.

### 2️⃣ HQL(Hibernate Query Language)
- SQL(Structed Query Language)과 유사하지만, 데이터베이스 테이블이 아닌 자바 객체를 대상으로 질의를 할 수 있도록 설계된 쿼리 언어입니다.
- 데이터베이스에 종속되지 ㅇ낳아 다른 DBMS(Database Management System, 데이터베이스 관리 시스템)로의 전환이 용이합니다.

### 3️⃣ 캐싱(Caching)
- Hibernate는 1차, 2차 캐싱을 제공하여 성능을 최적화합니다.
    - 이를 통해 동일한 데이터를 여러 번 요청 할 때 데이터베이스에 불필요한 접근을 줄일 수 있습니다.

### 4️⃣ 트랜잭션 관리.
- 데이터베이스의 트랜잭션을 효과적으로 관리해주며, 여러 데이터베이스 작업을 하나의 단위로 처리할 수 있게 도와줍니다.

### 5️⃣ 자동 스키마 생성.
- Hibernate는 자바 클래스를 기반으로 데이터베이스 스키마를 자동으로 생성하고 관리할 수 있습니다.

## 2️⃣ 장점.

### 👉 DB 독립성.
- Hibernate는 특정 데이터베이스에 종속되지 않으며, 여러 데이터베이스에 쉽게 적용할 수 있습니다.

### 👉 생산성 향상.
- SQL을 직접 작성할 필요가 없으므로 개발자의 생산성을 높일 수 있습니다.

### 👉 유지보수 용이.
- 객체지향적인 코드를 유지하며 데이터베이스 작업을 처리할 수 있어, 코드의 가독성과 유지 보수성이 높습니다.

## 3️⃣ 간단한 예.
```java
@Entity
@Table(name = "User")
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    private String email;
    
    // Getters and Setters
}
```

- 위의 예시에서 `User` 클래스는 데이터베이스 테이블에 매핑됩니다.
    - `@Entity`와 `@Table` 애노테이션을 통해 클래스가 테이블과 매핑되고, `@Id` 애노테이션은 기본 키(Primary Key)를 나타냅니다.
- Hibernate는 자바 애플리케이션이 데이터베이스와 상호작용할 때 객체 지향적으로 데이터를 처리할 수 있도록 도와주며, 데이터베이스의 복잡한 작업을 쉽게 관리할 수 있게 해줍니다.
