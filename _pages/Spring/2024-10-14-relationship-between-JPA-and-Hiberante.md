---
title: 🍃[Spring] JPA와 Hibernate는 어떤 관계인가요?
tags:
    - Spring
    - Framework
date: "2024-10-14"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] JPA와 Hibernate는 어떤 관계인가요?

- **JPA(Java Persistence API)와 Hibernate**는 밀접한 관계가 있지만 서로 다른 개념입니다.
    - 그들의 관계를 이해하기 위해서는 각각의 정의와 역할을 살펴볼 필요가 있습니다.

## 1️⃣ JPA(Java Persistence API).
- **JPA(Java Persistence API)는 자바 표준 명세(Java Specification)로,** 자바 애플리케이션에서 **관계형 데이터베이스(Relational Database, RDB)를** 쉽게 사용할 수 있도록 **ORM(Obeject-Relational Mapping)을 제공**하는 **인터페이스(Interface) 모음**입니다.
- **JPA(Java Persistence API)는** 자바 개발자에게 **ORM(Object-Relational Mapping) 기능을 제공**하기 위해 도입된 **표준 API**로, **특정 구현체가 아닙니다.**
    - 즉, **JPA(Java Persistence API) 자체는 기능을 제공하지 않고, ORM(Object-Relational Mapping)의 표준 규격만 정의합니다.**
- **JPA(Java Persistence API)를 사용**하면 애플리케이션 코드가 특정 구현체에 종속되지 않도록 **추상화(Abstraction)된 인터페이스(Interface)를 제공**합니다.

## 2️⃣ Hibernate
- **Hibernate**는 **JPA(Java Persistence API) 표준을 구현한 ORM(Object-Relational Mapping) 프레임워크 중 하나**입니다.
    - 즉, **JPA(Java Persistence API)가 정의한 규격을 여러 구현체 중 하나**로, 가장 널리 사용되는 **구현체**입니다.
- **Hibernate는 JPA(Java Persistence API)를 구현**하면서, **추가적인 기능도 제공하는 강력한 ORM(Object-Relational Mapping) 프레임워크**입니다.
    - 예를 들어, **캐싱, 스키마 자동 생성, HQL(Hibernate Query Language) 등의 고유한 기능을 포함**합니다.
- **Hibernate**는 **JPA(Java Persistence API)의 표준 인터페이스(Interface)를 지원**하면서도, **자체적인 API(Application Programming Interface)도 함께 제공**하여 더 많은 기능을 사용할 수 있게 합니다.

## 3️⃣ 관계 요약.
- **JPA(Java Persistence API)는 ORM(Object-Relational Mapping)의 표준 인터페이스(Interface)로서, 구현체가 필요합니다.**
- **Hibernate는 JPA(Java Persistence API) 표준을 구현한 구현체 중 하나**입니다.
    - 즉, **Hibernate는 JPA(Java Persistence API)의 규격을 따르면서도 자체적인 확장 기능을 제공하는 ORM(Object-Relational Mapping) 프레임워크(Framework)입니다.**

## 4️⃣ JPA와 Hibernate의 사용 방식.

### 1️⃣ JPA(Java Persistence API) 사용.
- JPA(Java Persistence API)는 인터페이스(Interface)이기 때문에 애플리케이션 코드가 특정 ORM(Object-Relational Mapping) 구현체에 의존하지 않도록 합니다.
    - 개발자는 JPA(Java Persistence API) 표준을 따르면서, Hibernate 같은 구현체를 선택해 사용할 수 있습니다.

### 👉 예시
```java
@PersistenceContext
private EntityManager entityManager;

public void saveUser(User user) {
    entityManager.persist(user); // JPA 표준 API 사용
}
```

### 2️⃣ Hibernate 사용
- Hibernate는 JPA를 구현하면서 자체적인 API(Application Programming Interface)도 제공합니다.
    - 개발자는 JPA(Java Persistence API) 표준 API(Application Programming Interface)를 사용하거나, Hibernate의 고유한 기능을 사용하기 위해 Hibernate의 API(Application Programming Interface)를 사용할 수 있습니다.

### 👉 예시
```java
Session session = sessionFactory.openSession();
session.save(user); // Hibernate 고유 API 사용
```

## 5️⃣ 결론.
- **JPA(Java Persistence API)는 ORM(Object-Relational Mapping)을 위한 표준 API이고, Hibernate는 그 표준을 구현한 구체적인 구현체입니다.**
- JPA(Java Persistence API)는 독립적이지만, Hibernate는 JPA를 따르며 추가적인 기능을 제공합니다.
    - Hibernate를 사용하면 JPA(Java Persistence API) 표준 인터페이스로 개발하면서도 필요한 경우 Hibernate의 고유 기능을 사용할 수 있습니다.
