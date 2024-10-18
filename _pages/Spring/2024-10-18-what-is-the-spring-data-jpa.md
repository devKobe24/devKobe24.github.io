---
title: 🍃[Spring] Spring Data JPA란 무엇일까요?
tags:
    - Spring
    - Framework
date: "2024-10-18"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Spring Data JPA란 무엇일까요?
- **Spring Data JPA**는 **Spring Framework**의 일부로, **JPA(Java Persistence API)를** 더욱 쉽게 사용할 수 있도록 도와주는 **모듈(Module)입니다.**
- JPA(Java Persistence API)는 자바 애플리케이션에서 **객체와 관계형 데이터베이스(Relational Database, RDB) 간의 매핑을 제공하는** 표준 인터페이스인데, Spring Data JPA는 이를 기반으로 개발자가 더 적은 코드로 데이터베이스와 상호작용할 수 있게 도와줍니다.

## 1️⃣ Spring Data JPA의 주요 특징.

### 1️⃣ 간편한 데이터 엑세스 계층.
- Spring Data JPA는 데이터베이스와의 CRUD(Create, Read, Update, Delete) 작업을 손쉽게 구현할 수 있는 방법을 제공합니다.
- 개발자는 **복잡한 SQL 쿼리를** 직접 작성할 필요 없이, **Repository 인터페이스를 정의하는** 것만으로 데이터베이스 작업을 처리할 수 있습니다.

### 2️⃣ 자동 구현 Repository.
- Spring Data JPA는 **Repository 인터페이스를 정의하면,** 런타임 시점에 해당 인터페이스의 구현체를 자동으로 생성합니다.
- 예를 들어, `findById`, `save`, `delete`와 같은 기본적인 데이터베이스 연산은 별도로 구현할 필요가 없습니다.

### 3️⃣ 메서드 이름 기반 쿼리 생성.
- Spring Data JPA는 **메서드 이름**을 분석하여 자동으로 쿼리를 생성합니다.
    - 예를 들어, `findByName`과 같은 메서드를 정의하면, `name` 필드를 기준으로 데이터를 조회하는 쿼리를 자동으로 생성합니다.
- 이를 통해 복잡한 쿼리도 쉽게 작성할 수 있으며, 추가적인 코드 작성이 줄어들어 개발 속도를 높여줍니다.

### 4️⃣ JPQL 및 네이티브 쿼리 지원.
- 필요할 경우 **JPQL(Java Persistence Query Language)** 또는 **네이티브 SQL(Structured Query Language) 쿼리를** 직접 작성할 수도 있습니다.
    - 복잡한 쿼리를 처리하거나 성능 최적화가 필요한 경우 유용하게 사용할 수 있습니다.

### 5️⃣ 페이징 및 정렬 지원.
- Spring Data JPA는 데이터 조회 시 **페이징(Paging)과 정렬(Sorting)** 기능을 기본적으로 지원합니다.
    - 이를 통해 대량의 데이터를 효율적으로 처리할 수 있습니다.

## 2️⃣ 주요 구성 요소.

### 1️⃣ Entity 클래스.
- 데이터베이스 테이블과 매핑되는 자바 클래스입니다.
    - JPA(Java Persistence API) 엔티티(Entity) 어노테이션(`@Entity`)을 사용하여 테이블 구조를 정의합니다.

### 2️⃣ Repotitory 인터페이스.
- Spring Data JPA에서 데이터베이스에 접근하기 위해 사용하는 인터페이스입니다.
- `CrudRepository`, `JpaRepository`, `PagingAndSortingRepository`와 같은 다양한 Repository 인터페이스가 제공됩니다.
    - 이를 확장하여 기본적인 CRUD(생성, 조회, 수정, 삭제) 기능뿐만 아니라 페이징, 정렬 등을 쉽게 구현할 수 있습니다.

### 3️⃣ Spring Data JPA의 어노테이션
- `@Entity`: 클래스를 JPA 엔티티(Entity)로 정의합니다.
- `@Id`: 기본 키(Primary Key)를 지정합니다.
- `@Repository`: 데이터 접근 객체(DAO)로 사용될 인터페이스 또는 클래스에 사용합니다. Spring에서 해당 클래스를 빈으로 관리할 수 있도록 합니다.
- `@Query`: JPQL 또는 네이티브 쿼리를 직접 작성할 때 사용합니다.

> 🙋‍♂️ [API에서의 인터페이스와 소프트웨어 공학에서의 인터페이스 개념.](https://www.devkobe24.com/CS/2024/2024-10-10-interface-in-api-and-interface-concepts-in-software-engineering.html)
> 🙋‍♂️ [JPA 어노테이션 - `@Entity`](https://www.devkobe24.com/Spring/2024-10-15-jpa-annotation-series-entity.html)
> 🙋‍♂️ [JPA 어노테이션 - `@Id`](https://www.devkobe24.com/Spring/2024-10-15-id-annotation.html)
> 🙋‍♂️ [언제 `@Service`,`@Repository`,`@Controller`와 같은 어노테이션을 사용할까?](https://www.devkobe24.com/Spring/2024-10-06-when-to-ues-these-annotations.html)
> 🙋‍♂️ [엔티티(Entity)는 무엇일까요?](https://www.devkobe24.com/CS/2024/2024-10-15-what-is-the-entity.html)

## 3️⃣ Spring Data JPA 사용 예시.

### 1️⃣ 엔티티 클래스 정의.
```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.GeneretedValue;
import javax.persistence.GenerationType;

@Entity
public class User {
    @Id
    @GeneretedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;
    
    // 기본 생성자, getter, setter 생략
}
```

### 2️⃣ Repository 인터페이스 정의.
```java
import org.springframework.data.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
    // 기본 CRUD 메서드 외에 추가 메서드 정의
    User findByName(String name);
}
```

- 위 코드에서 `UserRepository`는 `JpaRepository`를 확장하여, **User 엔티티(Entity)와 Long 타입의 기본 키(Primary Key)를 사용하는 Repository로 정의됩니다.**
- `findByName`: 메서드 이름을 기반으로 Spring Data JPA가 `name` 필드로 User를 찾는 쿼리를 자동으로 생성합니다.

### 3️⃣ Service 클래스에서 사용.
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
    
    public User getUserByName(String name) {
        return userRepository.findByName(name);
    }
    
    public User createUser(User user) {
        return userRepository.save(user);
    }
}
```

### 4️⃣ Spring Boot 설정(`application.properties`)
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mydatabase
spring.datasource.username=root
spring.datasource.password=secret
spring.jpa.hibernate.ddl-auto=update
```

## 4️⃣ Spring Data JPA의 장점.

### 1️⃣ 개발 생산성 향상.
- 데이터 접근 코드를 줄여주며, 메서드 이름 기반의 쿼리 생성으로 개발 속도를 높입니다.

### 2️⃣ 유연성.
- 복잡한 쿼리를 직접 작성할 수 있으며, 다양한 기능을 쉽게 확장할 수 있습니다.

### 3️⃣ Spring 생태계와 통합.
- Spring Framework와 쉽게 통합되며, Spring Boot를 통해 설정이 간편해집니다.

### 4️⃣ 페이징 및 정렬 지원.
- 대량의 데이터를 효율적으로 처리할 수 있도록 기본 페이징과 정렬을 제공합니다.

## 5️⃣ 요약.
- **Spring Data JPA**는 JPA 기반의 애플리케이션 개발을 더욱 쉽게 할 수 있도록 지원하는 Spring 모듈입니다.
- Repository 인터페이스를 통해 기본적인 CRUD(생성, 조회, 수정, 삭제) 기능과 메서드 이름 기반의 쿼리를 제공하며, 개발자가 데이터베이스와의 상호작용 코드를 줄일 수 있도록 도와줍니다.
- 이를 통해 빠르고 간편하게 데이터베이스 접근 계층을 구현할 수 있으며, 더 복잡한 쿼리가 필요한 경우 직접 쿼리를 작성할 수도 있습니다.
