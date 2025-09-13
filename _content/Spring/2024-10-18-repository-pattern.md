---
title: 🍃[Spring] Repository 패턴이란 무엇일까요?
tags:
    - Spring
    - Framework
date: "2024-10-18"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Repository 패턴이란 무엇일까요?
- **Repository 패턴**은 **데이터 접근 로직을 비즈니스 로직(Business Logic)과 분리하여, 데이터베이스나 다른 저장소와의 상호작용을 캡슐화하는 디자인 패턴입니다.**
- 이 패턴을 사용하면, 애플리케이션의 나머지 부분이 데이터베이스와 직접 상호작용하는 대신 **Repository**를 통해 데이터를 저장, 조회, 수정, 삭제 등의 작업을 수행할 수 있게 되어, 코드의 **유지보수성과 재사용성을 높일 수 있습니다.**

## 1️⃣ Repository 패턴의 주요 개념.

### 1️⃣ 데이터 접근 로직의 캡슐화.
- 데이터베이스와의 상호작용을 **Repository** 클래스로 감싸서 구현합니다.
    - 이렇게 하면 애플리케이션의 다른 부분에서 데이터베이스와 직접 상호작용하지 않고, Repository 클래스를 통해서만 데이터를 다루게 됩니다.
- 이로 인해 데이터 접근 로직과 비즈니스 로직이 분리되어, 코드의 유지보수성이 높아집니다.
    - 예를 들어, 데이터베이스를 변경해야 하는 경우에도 비즈니스 로직에는 영향을 미치지 않고 Repository만 수정하면 됩니다.

### 2️⃣ 데이터 소스의 추상화.
- Repository 패턴은 데이터가 어디에서 오는지에 대해 추상화를 제공합니다.
    - 데이터가 **관계형 데이터베이스(Relational Database, RDB), NoSQL 데이터베이스, 파일 시스템, 웹 서비스 등** 어떤 곳에 저장되어 있는지와 무관하게 동일한 인터페이스를 통해 데이터를 다룰 수 있습니다.
- 이 추상화 덕분에, 애플리케이션은 특정 데이터 소스에 종속되지 않으며, 데이터 소스를 교체하거나 확장하기 쉬워집니다.

### 3️⃣ CRUD 작업의 표준화.
- Repository는 보통 **CRUD 작업(Create, Read, Update, Delete)을** 표준화된 방식으로 제공합니다.
    - 이를 통해 데이터 접근 로직이 일관성을 유지할 수 있으며, 개발자가 CRUD(생성, 조회, 수정, 삭제) 작업을 수행할 때 혼란 없이 사용할 수 있습니다.

## 2️⃣ Repository 패턴의 장점.

### 1️⃣ 비즈니스 로직(Business Logic)과 데이터 접근 로직의 분리.
- 데이터 접근 코드와 비즈니스 로직(Business Logic) 코드가 분리되므로, 유지보수가 쉬워집니다.
    - 데이터베이스 관련 로직이 변경되더라도 비즈니스 로직(Business Logic)에 영향을 주지 않습니다.

> 🙋‍♂️ [비즈니스 로직(Business Logic)이란?](https://www.devkobe24.com/CS/2024/2024-09-02-Business-Logic.html)

### 2️⃣ 데이터 소스의 변경에 대한 유연성.
- 애플리케이션은 특정 데이터베이스 기술이나 저장소에 의존하지 않습니다.
    - 예를 들어, 애플리케이션이 MySQL에서 MongoDB로 데이터베이스를 전환해야 할 경우, Repository 내부의 구현만 변경하면 되므로 변경 작업이 용이해집니다.

### 3️⃣ 테스트 용이성.
- 데이터 접근 로직이 Repository로 추상화되어 있기 때문에, 테스트 코드에서 **Mock(모의 객체)를** 사용해 Repository의 동작을 대체할 수 있습니다.
    - 이를 통해 데이터베이스 없이도 비즈니스 로직을 테스트할 수 있게 됩니다.

## 3️⃣ Spring Data JPA에서의 Repository 패턴 구현.
- Spring Data JPA는 **Repository 패턴을 편리하게 구현할 수 있도록 해줍니다.**
    - 개발자는 데이터 접근 로직을 일일이 작성할 필요 없이, Spring Data JPA가 제공하는 `JpaRepository` 인터페이스를 확장함으로써 **자동으로 CRUD(생성, 조회, 수정, 삭제) 작업을 처리할 수 있게 됩니다.**

### 👉 예시: User 엔티티 클래스.

```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;

@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String Integer age;
    
    // 기본 생성자, getter, setter 생략
}
```

### 👉 예시: UserRepository 인터페이스.

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
    User findByName(String name);
}
```

- 위의 코드에서 `UserRepository`는 `JpaRepository`를 상속받아 자동으로 **기본적인 CRUD 메서드(`save`, `findAll`, `findById`, `delete` 등)를** 사용할 수 있습니다.
    - 또한, `findByName` 이라는 메서드를 선언하면, Spring Data JPA는 이 메서드 이름을 분석하여 **name** 필드를 기준으로 조회하는 SQL 쿼리를 자동으로 생성해줍니다.

### 👉 서비스 레이어에서 사용.
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.streotype.Service;

@Service
public class UserSevice {
    
    private final UserRepository userRepository;
    
    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    public User getUserByName(String name) {
        return userRepository.findByName(name);
    }
    
    public void saveUser(User user) {
        userRepository.save(user);
    }
}
```

## 4️⃣ Repository 패턴의 사용 예시.
- 만약 현재 MySQL을 사용하고 있지만, 미래에 **MongoDB**로 전환하고 싶다고 가정해 보겠습니다.
    - Repository 패턴을 사용하고 있다면, 다음과 같은 작업이 가능합니다.
        - 1. **MySQL 기반의 UserRepository 구현을 MongoDB 기반의 새로운 UserRepository로 교체.**
        - 2. 서비스 레이어나 비즈니스 로직을 수정할 필요 없이, 데이터베이스 기술을 전환할 수 있음.

## 5️⃣ 요약.
- **Repository 패턴**은 데이터베이스와의 상호작용을 캡슐화하여, **데이터 접근 로직을 비즈니스 로직과 분리하는 디자인 패턴입니다.**
- 이 패턴은 **데이터 소스의 변경에 대한 유연성을 제공하고, 애플리케이션 코드의 유지보수성과 재사용성을 높여줍니다.**
- Spring Data JPA는 이러한 Repository 패턴을 매우 쉽게 구현할 수 있도록 다양한 기능을 제공하여, 개발자들이 데이터 접근 계층을 간단하고 효율적으로 관리할 수 있게 해줍니다.
