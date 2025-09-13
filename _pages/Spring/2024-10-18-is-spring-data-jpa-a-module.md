---
title: 🍃[Spring] Spring Data JPA는 라이브러리가 아닌 모듈인가요?
tags:
    - Spring
    - Framework
date: "2024-10-18"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Spring Data JPA는 라이브러리(Library)가 아닌 모듈(Module)인가요?
- **Spring Data JPA**는 **Spring Data 프로젝트의 하위 모듈(Module)로,** 이를 **모듈(Module)이라고** 부르는 것이 더 정확합니다.
    - 다만, **라이브러리(Library)라는** 용어와도 종종 혼용되어 사용되곤 합니다.
- 두 용어 간에 약간의 차이가 있지만, 개발자들이 **Spring Data JPA**를 이야기할 때는 주로 모듈(Module)로서의 의미를 강조합니다.

## 1️⃣ 모듈(Module)과 라이브러리(Library)의 차이.

### 1️⃣ 모듈(Module)
- 모듈(Module)은 **특정 기능이나 목적을 중심으로 구성된 컴포넌트**를 의미하며, 더 큰 시스템의 일부분으로 동작합니다.
- Spring Data JPA는 **Spring Data**라는 더 큰 프로젝트의 일부분이며, 데이터 접근 계층에서 JPA(Java Persistence API)와 Hibernate를 쉽게 사용할 수 있도록 하는 기능을 제공합니다.
    - 이 점에서 Spring Data JPA는 모듈(Module)로 볼 수 있습니다.

> 🙋‍♂️ [모듈과 컴포넌트를 레고 블록에 비유해보면?!](https://www.devkobe24.com/CS/2024/2024-10-07-compare-modules-and-components-to-lego-blocks.html)
> 🙋‍♂️ [소프트웨어 공학에서의 컴포넌트.](https://www.devkobe24.com/CS/2024/2024-10-07-components-in-software-engineering.html)
> 🙋‍♂️ [소프트웨어 공학에서의 모듈.](https://www.devkobe24.com/CS/2024/2024-10-07-modules-in-software-engineering.html)

### 2️⃣ 라이브러리(Library)
- **라이브러리(Library)는 재사용 가능한 코드 집합으로,** 개발자가 특정 기능을 쉽게 구현할 수 있도록 도와줍니다.
- Spring Data JPA도 **의존성**으로 추가하면 프로젝트에서 사용 가능한 코드 집합을 제공하기 때문에 **라이브러리(Library)로** 볼 수도 있습니다.
- 하지만, 더 큰 맥락에서 **모듈(Module)은** 더 큰 시스템의 일부로 동작하는 반면, **라이브러리(Library)는** 독립적으로 재사용 가능한 코드 집합이라는 점에서 차이가 있습니다.

> 🙋‍♂️ [라이브러리(Library)와 프레임워크(Framework)의 차이점.](https://www.devkobe24.com/CS/2024/2024-09-26-Library-and-Framework.html)

## 2️⃣ Spring Data 프로젝트의 구조.
- **Spring Data**는 데이터 접근을 단순화하기 위해 만들어진 프로젝트로, 다양한 데이터 저장소에 쉽게 접근할 수 있도록 여러 모듈(Module)로 구성되어 있습니다.
    - 이 프로젝트는 다양한 **모듈(Module)을** 포함하고 있으며, 각 모듈(Module)은 특정 데이터 저장소에 대한 기능을 제공합니다.
        - 예를 들어
            - **Spring Data JPA :** JPA를 사용한 관계형 데이터베이스 접근을 위한 모듈
            - **Spring Data MongoDB :** MongoDB를 사용한 NoSQL 데이터베이스 접근을 위한 모듈
            - **Spring Data Redis :** Redis 데이터 저장소 접근을 위한 모듈
            - **Spring Data Elasticsearch :** Elasticsearch 접근을 위한 모듈
- 이처럼 **Spring Data JPA**는 **Spring Data 프로젝트**의 여러 모듈 중 하나로, **JPA(Java Persistence API)를 기반으로 하는 관계형 데이터베이스(Relational Database)와의 상호작용을 단순화**하는 기능을 제공하는 역할을 합니다.

## 3️⃣ Spring Data JPA를 모듈(Module)로 보는 이유.
- **Spring 생태계의 일부분**
    - Spring Data JPA는 Spring Data 프로젝트의 하위 모듈로, Spring의 일관된 구조와 스타일을 따르며, 다른 Spring 모듈과 자연스럽게 통합됩니다.
- **특정 기능에 집중**
    - Spring Data JPA는 JPA를 통해 데이터베이스와 상호작용하는 기능에 특화되어 있으며, 이러한 역할에 초점을 맞춘 독립적인 모듈로 설계되었습니다.
- **의존성 관리**
    - Maven 또는 Gradle의 의존성으로 추가할 때, Spring Data JPA 모듈을 추가함으로써 JPA 기반 데이터 접근 계층을 쉽게 구현할 수 있습니다.

## 4️⃣ 요약.
- **Spring Data JPA**는 **Spring Data 프로젝트의 하위 모듈로서,** JPA 기반의 데이터 접근을 쉽게 할 수 있도록 도와주는 기능을 제공합니다.
- 이 모듈은 더 큰 Spring Data 생태계의 일부로 동작하며, JPA를 사용하는 애플리케이션에서 데이터베이스와의 상호작용을 단순화하는 역할을 합니다.
- 개발자들이 라이브러리와 모듈이라는 용어를 혼용해 사용하기도 하지만, **Spring Data JPA는 엄밀히 말하면 모듈**로 보는 것이 맞습니다.
