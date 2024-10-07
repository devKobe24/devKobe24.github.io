---
title: 🍃[Spring] 계층형 아키텍처에서 Repository의 역할.
tags:
    - Spring
    - Framework
date: "2024-10-04"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 계층형 아키텍처에서 Repository의 역할.

<img src = "https://github.com/devKobe24/images2/blob/main/springboot/layered-architecture.png?raw=true">

Java 백엔드 애플리케이션의 **계층형 아키텍처**에서 **Repository 계층**은 **데이터베이스와의 상호작용을 관리하는 역할**을 합니다.

Repository는 데이터의 CRUD(Create, Read, Update, Delete) 작업을 처리하며, 데이터를 저장소(일반적으로 데이터베이스)에서 가져오고, 수정하며, 삭제하는 기능을 **캡슐화**합니다.

이를 통해 애플리케이션의 비즈니스 로직에서 데이터 접근을 분리하고, 유지보수성과 테스트 가능성을 높입니다.

## 1️⃣ Repository 계층의 주요 역할.

### 1. 데이터베이스 접근 관리.
- Repository는 애플리케이션과 데이터베이스 간의 중개자 역할을 하며, 데이터베이스로부터 데이터를 조회하거나, 데이터를 저장, 수정, 삭제하는 기능을 제공합니다.
- 모든 데이터 접근 로직은 Repository 계층에서 처리됩니다.

### 2. CRUD 작업 처리.
- Repository는 엔티티의 생명주기 전반에 걸친 CRUD 작업을 처리합니다.
- Java의 JPA나 Hibernate 같은 ORM(Object-Relational Mapping) 프레임워크를 사용해 객체와 데이터베이스 간의 매핑을 자동으로 관리합니다.
- 예를 들어, `save`, `findById`, `deleteById` 등의 메서드를 통해 객체를 데이터베이스에 저장하거나 조회하는 작업을 수행합니다.

### 3. 데이터 쿼리 처리.
- Repository 계층은 데이터를 조회하기 위해 데이터베이스에서 쿼리를 생성하고 실행합니다.
- Spring Data JPA와 같은 프레임워크를 사용하면 쿼리 메서드를 간편하게 정의할 수 있으며, 복잡한 조건 검색을 위한 JPQL(Java Persistence Query Language) 또는 네이티브 SQL 쿼리를 사용할 수 있습니다.
- 기본적인 쿼리 메서드 외에도 복잡한 쿼리를 작성하여 특정 조건에 맞는 데이터를 필터링할 수 있습니다.

### 4. 데이터베이스와의 추상화.
- Repository는 데이터베이스 접근 로직을 캡슐화하여 상위 계층(Service 등)에서 데이터베이스의 구체적인 동작 방식을 알 필요가 없도록 합니다.
- 이렇게 하면 데이터베이스가 변경되더라도 상위 계층에는 영향을 미치지 않도록 구조를 유지할 수 있습니다.

### 5. 데이터베이스 독립성.
- Repository를 사용함으로써 데이터베이스와의 구체적인 종속성을 줄일 수 있습니다.
- 애플리케이션이 사용하는 데이터베이스가 변경되더라도, Repository 계층만 적절히 수정하면 상위 계층에는 영향을 주지 않기 때문에, 데이터베이스 독립성을 유지할 수 있습니다.

### 6. 객체와 데이블 매핑 관리
- ORM을 사용해 데이터베이스 테이블과 객체를 매핑하는 역할을 담당합니다.
- 객체와 테이블 간의 관계(1, N)를 정의하고, 이러한 관계를 기반으로 데이터베이스 연산을 수행합니다.

## 2️⃣ 예시 코드
Spring Data JPA를 사용한 Repository 계층의 예시를 살펴보겠습니다.

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    // 기본적으로 JpaRepository에서 제공하는 CRUD 메서드들
    // save, fundById, findAll, deleteById 등이 제공됩니다.
    
    // 커스텀 쿼리 메서드 정의
    Optional<User> findByEmail(String email);
    
    // 복잡한 JPQL 쿼리 메서드 정의 가능
    @Query("SELECT u FROM User u WHERE u.name = :name AND u.status = :status")
    List<User> findByNameAndStatus(@Param("name") String name, @Param("status") String status);
}
```

## 3️⃣ 설명
- `@Repository`
    - Spring에서 이 인터페이스가 데이터 엑세스 계층을 나타낸다는 것을 명시하는 어노테이션입니다.
    - `@Repository` 어노테이션은 데이터베이스 예외를 Spring의 데이터 엑세스 예외로 변환하는 역할도 합니다.
- `JpaRepository<User, Long>`
    - Spring Data JPA에서 제공하는 기본 인터페이스로, `User` 엔티티와 `Long` 타입의 ID를 사용해 CRUD 작업을 처리합니다.
- **기본 CRUD 메서드**
    - `save`, `findById`, `deleteById` 등의 메서드가 기본적으로 제공됩니다.
- **커스텀 쿼리 메서드**
    - 매서드 명명 규칙을 통해 자동으로 SQL 쿼리를 생성하는 방식으로 특정 필드로 검색하는 메서드를 정의할 수 있습니다.
    - 예를 들어, `findByEmail`은 이메일을 기준으로 사용자를 조회합니다.
- **JPQL 사용**
    - 복잡한 조건이 필요한 경우, `@Query` 어노테이션을 사용해 JPQL을 작성할 수 있습니다.
    - 이 예시에서는 이름과 상태를 기반으로 사용자를 조회하는 쿼리를 정의했습니다.

## 4️⃣ Repository 계층의 이점.

### 1. 비즈니스 로직과 데이터 접근 로직의 분리.
- Repository 계층은 비즈니스 로직과 데이터베이스 접근을 명확히 분리하여 각 계층이 자신의 역활에만 집중할 수 있게 합니다.
- 이렇게 하면 코드가 더 모듈화되고 유지보수하기 쉬워집니다.

### 2. 코드의 재사용성.
- Repository 계층은 데이터베이스 접근 로직을 재사용할 수 있도록 만들어집니다.
- 여러 서비스에서 동일한 데이터베이스 쿼리나 데이터를 필요로 할 때, 이를 중앙에서 관리함으로써 코드 중복을 줄일 수 있습니다.

### 3. 테스트 가능성 향상.
- Repository 계층을 인터페이스로 정의함으로써 테스트 시 Mock 객체로 쉽게 대체할 수 있어, 데이터베이스에 접근하지 않고도 단위 테스트를 작성할 수 있습니다.

### 4. 데이터베이스 변경의 유연성.
- 데이터베이스가 변경되더라도 Repository 계층만 수정하면 상위 계층(Service나 Controller)은 전혀 변경하지 않고도 애플리케이션을 유지할 수 있습니다.
- 데이터베이스 독립성을 높이는 데 중요한 역할을 합니다.

## 5️⃣ 결론
Repository 계층은 데이터베이스와 상호작용하는 모든 작업을 담당하며, 데이터를 저장, 조회, 업데이트, 삭제하는 기능을 캡슐화합니다.
이를 통해 데이터 접근 로직이 비즈니스 로직과 분리되므로 애플리케이션의 유지보수성, 확장성, 재사용성이 크게 향상됩니다.
