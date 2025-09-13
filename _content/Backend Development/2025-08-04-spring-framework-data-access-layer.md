---
title: "📚[Backend Development] Java Spring 프레임워크의 계층 - Data Access Layer (데이터 접근 계층)"
tags:
    - Backend Ddevelopment
    - Component
    - Layer
    - Architecture
date: "2025-08-04"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 📚[Backend Development] Java Spring 프레임워크의 계층 - Data Access Layer (데이터 접근 계층)

## ✅ Data Access Layer (데이터 접근 계층)

Data Access Layer는 비즈니스 로직과 데이터베이스를 분리하여 시스템을 유연하고 확장 가능하게 만들기 위해 사용합니다.

- **주요 컴포넌트 :** `@Repository`
- **역할 :** 데이터베이스(DB)에 **직접 접근하여 데이터를 CRUD(Create, Read, Update, Delete)하는 역할**을 담당합니다.
- **설명 :** 비즈니스 로직과 데이터베이스 사이의 다리 역할을 합니다. JPA(Java Persistence API)와 같은 기술을 사용하여 SQL 쿼리를 직접 다루고, 데이터의 영속성(Persistence)을 관리합니다.

### ✅ 1. JPA (Java Persistence API)

**JPA**는 자바 애플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 **'명세' 또는 '규칙'** 입니다.

- **개념 :** 개발자가 직접 SQL 쿼리를 작성하는 대신, 일반 자바 객체(Object)를 다루듯이 코드를 짜면 JPA가 알아서 적절한 SQL을 생성하여 데이터베이스와 통신합니다. 이처럼 객체와 관계형 데이터베이스를 자동으로 연결해주는 기술을 **ORM(Object-Relational Mapping)** 이라고 하며, JPA는 이 ORM 기술의 표준 규격입니다.

- **역할 :** SQL 중심의 개발에서 객체 중심의 개발로 전환하여, 개발자가 비즈니스 로직에 더 집중할 수 있게 도와줍니다. 가장 유명한 JPA 구현체로는 **하이버네이트(Hibernate)** 가 있습니다.

**📝 비유 :** JPA는 '자동 번역기'와 같습니다. 개발자는 '자바 객체'라는 언어로 말하면, JPA가 'SQL'이라는 언어로 번역하여 데이터베이스와 소통하고 그 결과를 다시 '자바 객체'로 번역해서 돌려줍니다.

### ✅ 2. 데이터의 영속성 (Persistence)

**영속성**은 데이터가 프로그램이 종료되어도 사라지지 않고 **영구적으로 저장소에 남아있는 특성**을 의미합니다.

- **일반적 개념 :** 애플리케이션을 껐다 켜도 데이터가 그대로 남아있는 상태를 말합니다. 이러한 영속성을 보장하기 위해 데이터베이스, 파일 시스템 등을 사용합니다.
- **JPA에서의 개념 (영속성 컨텍스트) :** JPA에서는 조금 더 구체적인 의미로 사용됩니다. **'영속성 컨텍스트(Persistence Context)'** 는 JPA가 엔티티(Entity) 객체들을 관리하는 가상의 공간(캐시)입니다.
    - 데이터베이스에서 조회하거나 저장할 엔티티는 이 영속성 컨텍스트에 들어갑니다.
    - 일단 영속성 컨텍스트에 들어온 객체는 JPA가 계속 추적하며 변화를 감지합니다.
    - 트랜잭션이 끝나는 시점에, JPA는 영속성 컨텍스트 안에서 변경된 객체들을 감지하여 자동으로 **UPDATE** 쿼리를 날려 데이터베이스에 그 변경사항을 반영합니다. 이 과정을 통해 데이터의 영속성이 유지됩니다.

### ✅ 3. Data Access Layer는 언제 사용하나요?

**애플리케이션의 데이터를 영구 저장소(주로 데이터베이스)에 저장하거나 조회하는 모든 작업**에 사용됩니다.
비즈니스 계층(`Service`)이 데이터 처리를 요청하면, 이 계층이 실질적인 데이터베이스 통신을 담당합니다.

### ✅ 4. Data Access Layer는 어디서 사용하나요?

애플리케이션 아키텍처의 **가장 안쪽, 데이터베이스와 가장 가까운 곳에 위치**합니다.
비즈니스 계층(`Service`)의 요청을 받아 데이터베이스와 직접 상호작용하는 최전선 역할을 수행합니다.

### ✅ 5. Data Access Layer는 어떻게 사용하나요?

주로 `@Repository` **어노테이션**을 붙인 인터페이스나 클래스로 구현합니다.
특히 Spring Data JPA를 사용하면, `JpaRepository`와 같은 인터페이스를 상속받는 것만으로도 기본적인 CRUD 기능이 구현되어 매우 편리하게 사용할 수 있습니다.

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // 이메일로 사용자를 찾는 커스텀 메서드
    Optional<User> findByEmail(String email);
}
```

### ✅ 6. Data Access Layer는 왜 사용하나요?

가장 중요한 이유는 **데이터 접근 기술의 캡슐화와 관심사의 분리**입니다.

- **유연선 및 확장성 :** 나중에 데이터베이스를 MySQL에서 Oracle이나 다른 종류로 변경하더라도, 비즈니스 로직 코드는 전혀 수정할 필요 없이 이 Data Access Layer의 구현만 바꾸면 됩니다.

- **유지보수 용이성 :** 데이터베이스와 관련된 모든 코드가 한곳에 모여있어, SQL 쿼리 최적화나 테이블 구조 변경 시 수정할 부분을 찾기 쉽고 관리가 용이합니다.

- **역할 분리 :** 비즈니스 계층은 '무엇을 할지'에만 집중하고, 데이터 접근 계층은 '어떻게 데이터를 가져올지'에만 집중하게 하여 코드의 가독성과 명확성을 높입니다.

## ✅ `@Repository`

`@Repository`는 데이터베이스에 직접 접근하는 클래스(DAO)에 사용되며, 예외를 스프링의 일관된 방식으로 변환해주는 역할을 합니다.

### ✅ 1. `@Repository`는 언제 사용되나요?

**데이터베이스와 직접 통신하여 데이터를 CRUD(생성, 조회, 수정, 삭제)하는 객체를 만들 때** 사용합니다.
주로 데이터베이스의 테이블에 접근하는 DAO(Data Access Object) 클래스나 인터페이스에 적용됩니다.

### ✅ 2. `@Repository`는 어디서 사용되나요?

애플리케이션 아키텍처의 **Data Access Layer(데이터 접근 계층)에서 사용**됩니다.
이 계층은 비즈니스 로직을 처리하는 `Service` 계층과 실제 데이터베이스 사이에서 데이터를 주고 받는 역할을 담당합니다.

### ✅ 3. `@Repository`는 어떻게 사용되나요?

클래스나 인터페이스 선언부 위에 `@Repository` **어노테이션을 붙여주기만 하면 됩니다.**
특히 Spring Data JPA를 사용하면, `JpaRepository` 인터페이스를 상속받는 것만으로도 자동으로 Repository로 등록되어 기본적인 CRUD 메서드를 바로 사용할 수 있습니다.

```java
// 이 인터페이스가 데이터베이스에 접근하는 Repository임을 선언합니다.
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // JpaRepository를 상속받으면 기본적인 CRUD(save, findById, findAll, delete 등)
    // 메서드가 자동으로 제공됩니다.
    
    // 메서드 이름을 규칙에 맞게 작성하면, Spring Data JPA가 알아서
    // 쿼리를 생성해줍니다. (예: 이메일로 사용자 찾기)
    Optional<User> findByEmail(String email);
}
```

### ✅ 4. `@Repository`는 왜 사용되나요?

`@Repository`를 사용하는 주된 이유는 두 가지입니다.

- **역할 명시 (코드 가독성) :** 이 클래스가 **'데이터 저장소에 접근하는 객체'** 임을 명확하게 나타냅니다. 이를 통해 개발자는 클래스의 역할을 쉽게 파악할 수 있습니다.
- **예외 변환 (Exception Translation) :** `@Repository`의 가장 중요한 기능입니다. 데이터베이스마다 발생하는 예외(e.g, MySQL의 `MySQLIntegrityConstraintViolationException`)가 다릅니다. `@Repository`가 붙어 있으면, 스프링이 이러한 특정 기술에 종속적인 예외들을 **스프링의 일관된 예외 계층인 `DataAccessException`으로 변환**해줍니다. 덕분에 개발자는 데이터베이스 종류와 상관없이 일관된 방식으로 예외를 처리할 수 있습니다.
