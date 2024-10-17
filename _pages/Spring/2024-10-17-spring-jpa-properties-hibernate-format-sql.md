---
title: 🍃[Spring] spring.jpa.properties.hibernate.format_sql이란 무엇일까요?
tags:
    - Spring
    - Framework
date: "2024-10-17"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] spring.jpa.properties.hibernate.format_sql이란 무엇일까요?

- **`spring.jpa.properties.hibernate.format_sql`은** Spring Boot 애플리케이션에서 Hibernate가 생성하는 **SQL(Structured Query Language) 쿼리를 읽기 쉽게 포맷하는 설정입니다.**
- 이 설정을 활성화하면 Hibernate가 데이터베이스에 실행하는 **SQL(Structured Query Language) 쿼리가 포맷된 형식으로 출력되며,** 이를 통해 개발자는 쿼리의 구조를 더 쉽게 이해할 수 있습니다.

## 1️⃣ 역할.
- **SQL(Structured Query Language) 쿼리 포맷팅**
    - 기본적으로 Hibernate가 출력하는 SQL(Structured Query Language) 쿼리는 모두 한 줄로 출력되기 때문에 복잡한 쿼리를 읽기가 어렵습니다.
        - `spring.jpa.properties.hibernate.format_sql`을 **true**로 설정하면 SQL(Structured Query Language) 쿼리를 보기 좋게 들여쓰기가 된 형태로 출력해줍니다.
- **디버깅 및 최적화**
    - SQL(Structured Query Language) 쿼리가 가독성이 좋아지면, 개발자는 애플리케이션이 데이터베이스에 어떤 쿼리를 실행하는지 쉽게 파악할 수 있으며, 쿼리를 디버깅하거나 성능을 최적화하는 데 도움이 됩니다.

## 2️⃣ 설정 방법.
- Spring Boot에서는 `application.properties` 또는 `application.yml` 파일에서 이 속성을 설정할 수 있습니다.

### 👉 `application.properties` 파일에서 설정.
```properties
spring.jpa.properties.hibernate.format_sql=true
```

### 👉 `application,yml` 파일에서 설정.
```yaml
spring:
  jpa:
    properties:
      hibernate:
        format_sql: true
```

## 3️⃣ 동작 예시.
- `spring.jpa.properties.hibernate,format_sql=true`로 설정한 후, 복잡한 SQL 쿼리를 실행하면 콘솔에 출력되는 SQL이 자동으로 들여쓰기가 적용된 상태로 출력됩니다.

### 👉 설정 전(포맷팅되지 않은 SQL)
```sql
select user0_.id as id1_0_, user0_.email as email2_0_, user0_.name as name3_0_ from user user0_
```

### 👉 설정 후(포맷팅된 SQL)
```sql
select
    user0_.id as id1_0_, 
    user0_.email as email2_0_, 
    user0_.name as name3_0_ 
from
    user user0_
```

- 이렇게 포맷팅된 SQL은 가독성이 높아져, 복잡한 SQL 쿼리도 쉽게 이해할 수 있습니다.

## 4️⃣ 관련 설정.
- `spring.jpa.show-sql`
    - 이 설정은 Hibernate가 실행하는 SQL 쿼리를 콘솔에 출력하도록 활성화합니다.
        - `show-sql=true`로 설정하면 SQL 쿼리를 콘솔에서 확인할 수 있습니다.
```properties
spring.jpa.show-sql=true
```

- `spring.jpa.properties.hibernate.use_sql_comments`
    - 이 설정은 쿼리 로그에 SQL 주석을 추가하여, 쿼리가 실행된 위치나 목적에 대한 추가 정보를 제공합니다.
```properties
spring.jpa.properties.hibernate.use_sql_comments=true
```

## 5️⃣ 요약.
- **`spring.jpa.properties.hibernate.format_sql`은** Hibernate가 출력하는 SQL 쿼리를 읽기 쉽게 포맷팅하는 설정입니다.
- 이 설정을 `true`로 활성화하면 SQL 쿼리의 가독성이 향상되며, 개발자는 디버깅과 쿼리 최적화 작업을 더 쉽게 할 수 있습니다.
