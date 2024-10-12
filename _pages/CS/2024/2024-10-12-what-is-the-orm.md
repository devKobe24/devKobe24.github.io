---
title: "💾 [CS] ORM이란 무엇일까요?"
tags:
    - CS
date: "2024-10-12"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] ORM이란 무엇일까요?
- **ORM(Object-Relational Mapping)은 객체-관계 매핑**을 의미하며, **객체 지향 프로그래밍 언어(예: Java, Python 등)에서** 사용하는 객체와 **관계형 데이터베이스**의 **테이블 간의 데이터를 매핑하는 기술을 말합니다.**
- ORM(Object-Relational Mapping)은 **객체 지향 방식과 관계형 데이터베이스**의 데이터 구조가 서로 다르다는 문제를 해결하기 위한 솔루션으로, 프로그래머가 데이터베이스의 세부적인 SQL 쿼리 없이 **객체 모델을 통해 데이터베이스와 상호작용할 수 있도록 도와줍니다.**

## 1️⃣ ORM(Object-Relational Mapping)의 주요 기능.

### 1️⃣ 객체와 테이블 간의 매핑.
- **객체 지향 언어**에서는 데이터가 **객체**로 표현되고, 관계형 데이터베이스에서는 데이터가 **테이블** 형태로 저장됩니다.
    - ORM(Object-Relational Mapping)은 프로그래밍 언어의 **객체와 데이터베이스의 테이블을 자동으로 매핑하여,** 객체를 이용해 데이터베이스와 상호작용할 수 있도록 합니다.

### 2️⃣ SQL 추상화.
- ORM(Object-Relational Mapping)은 **SQL 쿼리를 자동으로 생성하고,** 프로그래머가 **객체를 사용하여** 데이터를 조회, 삽입, 삭제, 수정하는 코드를 작성할 수 있게합니다.
    - 이를 통해 SQL 없이도 데이터베이스 작업을 쉽게 수행할 수 있습니다.

> 🙋‍♂️ [SQL이란?](https://www.devkobe24.com/CS/2024/2024-09-30-SQL.html)
> 🙋‍♂️ [데이터베이스란?](https://www.devkobe24.com/CS/2024/2024-09-29-Database.html)

### 3️⃣ 데이터베이스 독립성.
- ORM(Object-Relational Mapping)은 특정 **데이터베이스에 종속되지 않고,** 다양한 데이터베이스에서 동일한 코드로 동작할 수 있습니다.
    - 데이터베이스를 변경하더라도 ORM을 사용하면 코드를 거의 수정하지 않아도 됩니다.

### 4️⃣ 객체 모델 중심의 개발.
- ORM을 사용하면 **객체 모델**을 통해 데이터베이스와 상호작용하기 때문에, 데이터베이스와의 상호작용이 객체 지향 프로그래밍의 방식과 일관성을 유지합니다.
    - 이는 **비즈니스 로직**과 데이터베이스 상호작용을 자연스럽게 통합하는 데 도움을 줍니다.

> 🙋‍♂️ [비즈니스 로직(Business Logic)이란?](https://www.devkobe24.com/CS/2024/2024-09-02-Business-Logic.html)
> 🙋‍♂️ [API 설계, 계층형 아키텍처, 트랜잭션, 엔티티(Entity), 비즈니스 로직과 비즈니스 규칙의 차이점.](https://www.devkobe24.com/CS/2024/2024-09-26-APIdesign-LayeredArchitecture-Transaction-Entity-BusinessLogicAndRules.html)

## 2️⃣ ORM의 동작 원리.

### 1️⃣ 객체와 데이터베이스 테이블 매핑.
- **객체의 속성**은 **데이터베이스 테이블의 컬럼(Column, 열)에** 대응하고, **객체의 인스턴스**는 **테이블의 로우(Row, 행)에** 대응됩니다.
- 예를 들어, 객체 모델에 `User` 클래스가 있다면, 데이터베이스에는 `User` 테이블이 있고, 그 속성 `id`, `name`, `email` 등은 **테이블의 컬럼(Column, 열)과** 매핑됩니다.

### 2️⃣ SQL 자동 생성.
- ORM(Object-Relational Mapping) 라이브러리는 **객체를 기반으로 SELECT, INSERT, UPDATE, DELETE**와 같은 SQL 쿼리를 자동으로 생성합니다.
- 예를 들어, `User` 객체를 데이터베이스에 저장하는 코드를 작성하면, ORM(Object-Relational Mapping)이 자동으로 **INSERT** SQL 쿼리를 생성하여 테이블에 해당 데이터를 삽입합니다.

### 3️⃣ 데이터베이스 연동.
- ORM(Object-Relational Mapping)은 **객체 상태를** 추적하고, 변경 사항이 있을 경우 이를 데이터베이스와 동기화합니다.
    - 객체의 속성값이 변경되면, ORM(Object-Relationl Mapping)은 자동으로 **UPDATE** 쿼리를 생성하고 실행하여 데이터베이스를 업데이트합니다.

## 3️⃣ ORM의 장점.

### 1️⃣ 생산성 향상.
- ORM(Object-Relational Mapping)을 사용하면 **SQL 작성을 줄이고,** 객체 지향 프로그래밍 방식으로 데이터를 처리할 수 있기 때문에, 개발자는 더 적은 코드로 데이터베이스와 상호작용할 수 있습니다.
    - 이는 개발 속도를 높이고 **유지보수를** 쉽게합니다.

### 2️⃣ 데이터베이스 독립성.
- ORM(Object-Relational Mapping)은 특정 데이터베이스에 의존하지 않으며, 데이터베이스를 변경하더라도 ORM(Object-Relational Mapping) 라이브러리만 맞추면 프로그램 코드를 거의 수정하지 않고도 다양한 데이터베이스에서 동작할 수 있습니다.

### 3️⃣ 보안성
- ORM(Object-Relational Mapping)은 자동으로 SQL 쿼리를 생성하기 때문에 **SQL 인젝션 공격**과 같은 보안 취약점을 방지하는 데 도움이 됩니다.
    - 직접 SQL을 작성할 필요가 줄어들기 때문에, 보안성이 향상됩니다.

### 4️⃣ 유지보수성.
- 객체 지향 설계를 유지하면서 데이터베이스와 상호작용할 수 있어, 코드의 가독성과 유지보수가 쉬워집니다.
    - 데이터베이스 관련 변경이 필요할 때도 객체 모델을 통해 쉽게 변경할 수 있습니다.

## 4️⃣ ORM의 단점.

### 1️⃣ 복잡한 쿼리 작성의 한계.
- ORM은 복잡한 **쿼리 최적화**나 **특정한 SQL** 기능을 충분히 지원하지 않을 수 있습니다.
    - 매우 복잡한 쿼리가 필요한 경우 ORM 대신 **직접 SQL**을 작성해야 할 때도 있습니다.

### 2️⃣ 성능 이슈.
- 자동으로 SQL 쿼리를 생성하는 ORM은 **직접 생성한 SQL**에 비해 성능이 다소 떨어질 수 있습니다.
    - 대규모 트래픽이나 데이터 처리가 많은 환경에서는 ORM(Object-Relational Mapping) 사용이 **비효율적일 수 있습니다.**

### 3️⃣ 추상화로 인한 제어력 감소.
- ORM(Object-Relational Mapping)은 SQL을 **추상화**하기 때문에, 데이터베이스의 세부적인 제어가 어렵습니다.
    - SQL의 세부 동작을 직접 관리하고 싶을 때는 ORM(Object-Relational Mapping)보다 **직접 SQL 작성이 더 나을 수 있습니다.**

> 🙋‍♂️ [추상화(abstraction)](https://www.devkobe24.com/CS/2024/2024-08-31-Abstraction.html)
> 🙋‍♂️ [DIP의 정의에서 말하는 '추상화된 것'이란 무엇일까?](https://www.devkobe24.com/CS/2024/2024-10-07-what-is-the-abstracted-thing-mentioned-in-the-definition-of-DIP.html)
> 🙋‍♂️ [DIP의 정의에서 말하는 '추상화된 것'과 '추상화'의 개념의 차이점.](https://www.devkobe24.com/CS/2024/2024-10-07-diff-btw-abstracted-thing-and-abstraction.html)

## 5️⃣ 대표적인 ORM 라이브러리.

### Hibernate
- **Java와 JPA(Java Persistence API)를** 지원하는 가장 널리 사용되는 ORM 프레임워크입니다.
    - Hibernate는 객체와 관계형 데이터베이스 간의 매핑을 자동으로 처리하며, 다양한 데이터베이스를 지원합니다.

> 🙋‍♂️ [JPA란 무엇인가요?](https://www.devkobe24.com/Spring/2024-10-08-what-is-the-jpa.html)
> 🙋‍♂️ [JPA를 사용하는 이유.](https://www.devkobe24.com/Spring/2024-10-08-why-use-jpa.html)

## 6️⃣ 결론.
- **ORM(Object-Relational Mapping)은** 객체 지향 프로그래밍에서 **객체와 관계형 데이터베이스 간의 매핑**을 관리하는 기술로, 개발자가 **객체를 사용하여 데이터베이스 작업을 수행할 수 있게 해줍니다.**
    - 이를 통해 개발자는 SQL 작성의 번거로움을 줄이고, 생성성, 유지보수성, 보안성을 높일 수 있습니다.
        - 그러나 복잡한 쿼리 처리나 성능 문제에 있어서는 SQL을 장성하는 것이 더 나을 수 있습니다.
- ORM(Object-Relation Mapping)을 적절히 활용하면 **코드의 가독성과 효율성을 크게 향상시킬 수 있습니다.**
