---
title: 🍃[Spring] 계층형 아키텍처(Layered Architecture), 3계층 아키텍처(Three-Tier Architecture)
tags:
    - Spring
    - Framework
date: "2024-09-02"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 계층형 아키텍처(Layered Architecture), 3계층 아키텍처(Three-Tier Architecture)
- Controller를 통해 외부의 요청을 받고, Service에서 비즈니스 로직을 처리하며, Repository에서 데이터를 저장하고 관리하는 패턴은 **"계층형 아키텍처(Layered Architecture)"** 또는 **"3계층 아키텍처(Three-Tier Architecture)"** 라고 부릅니다.

## 1️⃣ 계층형 아키텍처(Layered Architecture)
- 이 아키텍처 패턴은 애플리케이션을 여러 계층으로 나누어 각 계층이 특정한 역할을 담당하도록 구조와합니다.
- 이 방식은 소프트웨어의 복잡성을 줄이고, 코드의 유지보수성을 높이며, 테스트하기 쉽게 만들어줍니다.
- 스프링 프레임워크에서 이 패턴은 자주 사용됩니다.

## 2️⃣ 각 계층별 역할.
- **1. Presentation Layer(프레젠테이션 계층) - Controller**
    - 사용자 인터페이스와 상호작용하는 계층입니다.
    - 외부의 요청을 받아서 처리하고, 응답을 반환합니다.
    - 스프링에서는 주로 **`@Controller`** 또는 **`@RestController`** 를 사용하여 이 계층을 구현합니다.
- **2. Business Logic Layer(비즈니스 로직 계층) - Service**
    - [비즈니스 로직](https://www.devkobe24.com/Backend/CS/2024-09-02-Business-Logic.html)을 처리하는 계층입니다.
    - 데이터의 처리, 계산, 검증 등 핵심적인 애플리케이션 로직이 구현됩니다.
    - 스프링에서는 주로 **`@Service`** 애노테이션을 사용하여 이 계층을 구현합니다.
- **3. Data Access Layer(데이터 접근 계층) - Repository**
    - 데이터베이스나 외부 데이터 소스와 상호작용하는 계층입니다.
    - 데이터의 CRUD(Create, Read, Update, Delete) 작업을 처리합니다.
    - 스프링에서는 주로 **`@Repository`** 애노테이션을 사용하여 이 계층을 구현하며 JPA, MyBatis, Hibernate 등의 ORM(Object-Relational Mapping) 도구와 함께 사용됩니다.

## 3️⃣ 이 패턴의 주요 장점.
- **모듈화**
    - 각 계층이 독립적으로 관리되므로, 각 계층의 코드가 명확히 분리됩니다.
- **유지보수성**
    - 비즈니스 로직, 데이터 접근, 그리고 프레젠테이션 로직이 분리되어 있어, 각 부분을 독립적으로 수정하거나 확장하기 쉽습니다.
- **테스트 용이성**
    - 각 계층을 독립적으로 테스트할 수 있어, 단위 테스트와 통합 테스트가 용이합니다.
- **유연성**
    - 특정 계층을 변경하거나 대체할 때, 다른 계층에 미치는 영향을 최소화할 수 있습니다.

이 계층형 아키텍처는 스프링 프레임워크를 사용하는 대부분의 애플리케이션에서 채택하는 일반적인 구조이며, 소프트웨어 설계의 베스트 프랙티스 중 하나로 널리 인정받고 있습니다.
