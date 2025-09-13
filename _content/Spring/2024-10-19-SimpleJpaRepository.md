---
title: 🍃[Spring] SimpleJpaRepository란 무엇일까요?
tags:
    - Spring
    - Framework
date: "2024-10-19"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] SimpleJpaRepository란 무엇일까요?
- **`SimpleJpaRepository`는 Spring Data JPA(Java Persistence API)에서** 제공하는 **기본 Repository 구현 클래스**입니다.
- 이 클래스는 Spring Data JPA의 **Repository 인터페이스(`CrudRepository`, `JpaRepository` 등)를** 구현하며, 일반적으로 사용되는 **CRUD(생성(Create), 조회(Read), 수정(Update), 삭제(Delete)) 기능을** 포함하고 있습니다.

## 1️⃣ 역할.
- `SimpleJpaRepository`는 개발자가 정의한 **Repository 인터페이스의 구현체로,** 데이터베이스와의 상호작용을 간편하게 처리할 수 있도록 다양한 메서드를 제공합니다.
- 개발자는 직접 CRUD 로직을 구현하지 않고도, `SimpleJpaRepository`를 통해 **자동으로 제공되는 기본 CRUD 메서드를** 활용할 수 있습니다.

## 2️⃣ `SimpleJpaRepository`의 동작 방식.

### 1️⃣ 기본 CRUD 메서드 제공.
- `SimpleJpaRepository`는 `save`, `findById`, `findAll`, `delete` 등과 같은 기본적인 CRUD(생성(Create), 조회(Read), 수정(Update), 삭제(Delete)) 메서드를 구현합니다.
- 개발자가 `JpaRepository` 인터페이스를 확장하여 커스텀 Repository 인터페이스를 정의하면, Spring Data JPA가 런타임 시점에 `SimpleJpaRepository`를 사용해 해당 인터페이스의 구현체를 자동으로 생성합니다.

### 2️⃣ 데이터베이스와의 자동 상호작용.
- `SimpleJpaRepository`는 **Hibernate**와 같은 JPA(Java Persistence API) 구현체와 상호작용하여, 데이터를 데이터베이스에 저장하거나 조회할 때 필요한 SQL(Structured Query Language) 쿼리를 자동으로 생성하고 실행합니다.
- 개발자는 데이터베이스와의 직접적인 상호작용을 구현할 필요가 없므며, Spring Data JPA가 이 모든 것을 자동으로 처리합니다.

### 3️⃣ 쿼리 메서드 지원
- `SimpleJpaRepository`는 메서드 이름을 분석하여 **쿼리 메서드**를 자동으로 생성하는 기능을 지원합니다.
    - 예를 들어, `findByName`과 같은 메서드를 정의하면, Spring Data JPA는 자동으로 `name` 필드를 기준으로 데이터를 조회하는 쿼리를 생성합니다.

## 3️⃣ `SimpleJpaRepository`의 기본적인 CRUD 메서드.
- `SimpleJpaRepository`는 다음과 같은 기본 메서드를 제공합니다.
    - `save(S entity)`
        - 엔티티를 데이터베이스에 저장합니다. 엔티티가 이미 존재하면 업데이트하고, 존재하지 않으면 새로 삽입합니다.
    - `findById(ID id)`
        - 지정된 ID로 데이터베이스에서 엔티티를 조회합니다.
    - `findAll()`
        - 데이터베이스에 저장된 모든 엔티티를 조회합니다.
    - `delete(T entity)`
        - 데이터베이스에서 엔티티를 삭제합니다.
    - `deleteById(ID id)`
        - 지정된 ID로 데이터베이스에서 엔티티를 삭제합니다.
    - `count()`
        - 데이터베이스에 저장된 엔티티의 총 개수를 반환합니다.

## 4️⃣ 사용 예시.

### 👉 `UserRepository` 인터페이스.

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface UseRepository extends JpaRepository<User, Long> {
    User findByName(String name);
}
```
- 위 예시에서 `UserRepository`는 `JpaRepository`를 상속하고 있으며, 이를 통해 `SimpleJpaRepository`가 `UserRepository`의 구현체로 동작하게 됩니다.
    - `findByName`, `save`, `findById` 등의 메서드를 별도로 구현하지 않아도, Spring Data JPA가 **`SimpleJpaRepository`를** 사용하여 이들을 자동으로 제공해줍니다.

### 👉 서비스 클래스에서 사용.
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.Optional;

@Sevice
public class UserService {
    
    private final UserRepository userRepository;
    
    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    public User createUser(String name, Integer age) {
        User user = new User(name, age);
        return userRepository.save(user); // SimpleJpaRepository의 save 메서드 사용
    }
    
    public Optional<User> getUserById(Long id) {
        return userRepository.findById(id); // SimpleJpaRepository의 findById 메서드 사용
    }
    
    public void deleteUser(Long id) {
        userRepository.deleteById(id); // SimpleJpaRepository의 deleteById 메서드 사용
    }
}
```

## 5️⃣ `SimpleJpaRepository`의 확장 및 커스터마이징.

### 1️⃣ 기본 기능 확장.
- 개발자는 `SimpleJpaRepository`가 제공하는 기본 기능 외에 추가적인 커스텀 기능을 Repository에 구현할 수 있습니다.
    - 예를 들어, 복잡한 쿼리를 직접 정의하거나, 추가적인 비즈니스 로직을 구현할 수 있습니다.

### 2️⃣ `@Query` 어노테이션 사용.
- `@Query` 어노테이션을 사용하면 복잡한 JPQL(Java Persistence Query Language) 또는 네이티브 SQL 쿼리를 직접 작성하여 메서드에 연결할 수 있습니다.

```java
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;

public interface UserRepository extends JpaRepository<User, Long> {
    @Query("SELECT u FROM User u WHERE u.name = :name AND u.age > :age")
    List<User> findByNameAndAgeGreaterThan(@Param("name") String name, @Param("age") Integer age);
}
```

## 6️⃣ 요약.
- **`SimpleJpaRepository`는** **Spring Data JPA**에서 기본적으로 제공하는 **CRUD 기능의 구현체로,** `JpaRepository` 인터페이스를 통해 자동으로 생성됩니다.
- 개발자는 `JpaRepository`를 상속받는 Repository 인터페이스를 정의함으로써 `SimpleJpaRepository`가 제공하는 **자동 CRUD 기능**을 활용할 수 있으며, 메서드 이름 기반 쿼리, 커스텀 쿼리 등을 통해 데이터베이스와의 상호작용을 간편하게 처리할 수 있습니다.
- Spring Data JPA는 **데이터베이스와의 상호작용을 단순화**하고, **코드의 가독성과 유지보수성**을 높이는 데 큰 역할을 합니다.
- `SimpleJpaRepository`는 이러한 Spring Data JPA의 핵심을 담당하는 기본 구현체입니다.
