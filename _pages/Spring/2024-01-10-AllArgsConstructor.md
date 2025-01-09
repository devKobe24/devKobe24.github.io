---
title: 🍃[Spring] `@AllArgsConstructor`은 무엇일까요?
tags:
    - Spring
    - Framework
date: "2025-01-10"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] `@AllArgsConstructor`은 무엇일까요?
## 📌 Intro.
- ↘︎ `@AllArgConstructor`는 **Lombok**에서 제공하는 어노테이션으로 **모든 필드를 매개변수로 갖는 생성자를 자동으로 생성해 줌.**

## ✅1️⃣ 주요 특징.
### 1. 모든 필드를 포함한 생성자 추가.
- ↘︎ 클래스에 선언된 **모든 필드를 초기화**하는 생성자를 자동으로 생성함.
- ↘︎ 수동으로 생성자를 작성하지 않아도 되므로 코드가 간결해짐.

### 2. 주로 객체 생성에 사용.
- ↘︎ 의존성 주입, DTO 클래스 생성 등 **객체 생성 시 필드를 초기화하는 경우**에 유용함.

### 3. Builder 패턴과 호환.
- ↘︎ `@AllArgsConstructor`는 주로 Builder 패턴과 함께 사용되어 객체 초기화 방식을 유연하게 처리할 수 있다.

## ✅2️⃣ 사용 예제.
### 1. 기본 사용.
```java
import lombok.AllArgsConstructor;

@AllArgsConstructor
public class MyClass {
    private String name;
    private int age;
}
```
- ↘︎ 위 코드는 다음과 같은 생성자를 자동으로 생성함.
```java
public MyClass(String name, int age) {
    this.name = name;
    this.age = age;
}
```

### 2. 엔티티 클래스에서의 사용.
- ↘︎ JPA에서 데이터베이스와 매핑되는 **엔티티 클래스**에서 객체를 초기화할 때 유용함.
```java
import jakarta.persistence.Entity;
import lombok.AllArgsConstructor;

@Entity
@AllArgsConstructor
public class User {
    private Long id;
    private String username;
    private String email;
}
```
- ↘︎ 위 생성자를 통해 쉽게 엔티티 객체를 초기화할 수 있음.
```java
User user = new User(1L, "Alice", "alice@example.com");
```

### 3. DTO 객체 생성에 활용.
- ↘︎ 데이터를 전달하거나 응답할 DTO 객체를 초기화하는 데 사용됨.
```java
import lombok.AllArgsConstructor;

@AllArgsConstructor
public class UserDTO {
    private String username;
    private String email;
}
```

- ↘︎ `new UserDTO("Alice", "alice@example.com")`와 같은 형태로 객체를 생성할 수 있음.

## ✅3️⃣ `@AllArgsConstructor`와 다른 Lombok 어노테이션.
### 1. `@NoArgsConstructor`
- ↘︎ 매개변수가 없는 기본 생성자를 자동으로 생성.

### 2. `@RequiredArgsConstructor`
- ↘︎ `final` 또는 `@NonNull`로 선언된 필드만 포함하는 생성자를 자동으로 생성.

### 3. `@Builder`와 조합.
- ↘︎ Builder 패턴을 통해 가독성과 유연성을 높일 수 있음.
```java
import lombok.AllArgsConstructor;
import lombok.Builder;

@AllArgsConstructor
@Builder
public class MyClass {
    private String name;
    private int age;
}
```
- ↘︎ `MyClass.builder().name("Alice").age(25).builder();` 형태로 객채 생성 가능.

## ✅4️⃣ 장점.
### 1. 코드의 간결화.
- ↘︎ 생성자를 수동으로 작성할 필요 없이 **모든 필드를 포함하는 생성자를 자동으로 생성해줌.**

### 2. 유지보수성 향상.
- ↘︎ 클래스 필드가 변경될 경우 생성자 코드도 자동으로 변경되므로 **일관성을 유지할 수 있음.**

### 3. 객체 초기화 용이.
- ↘︎ 객체를 **한 번에 초기화**하는데 유용하며 DTO, 엔티티, VO 클래스 등 다양한 곳에서 활용 가능.

## ✅5️⃣ 주의점.
### 1. 필드 개수 증가 시 가독성 저하.
- ↘︎ 필드가 많은 경우 생성자 매개변수의 순서를 잘못 사용할 가능성이 있음.
    - ↘︎ 이런 경우 `@Builder` 사용을 권장.
### 2. JPA와 함께 사용할 때.
- ↘︎ JPA 엔티티 클래스에서 `@AllArgsConstructor`는 모든 필드를 초기화하므로 기본 생성자(`@NoArgsConstructor`)도 함께 추가해야 리플랙션과 호환됨.
```java
@AllArgsConstructor
@NoArgsConstructor
public class User
```

## 🚀 정리
- ↘︎ `@AllArgsConstructor`는 모든 필드를 매개변수로 받는 생성자를 자동으로 생성하여 **코드의 가독성과 유지보수성을 향상시킴.**
    - ↘︎ 주로 DTO, 엔티티 클래스의 객체 초기화에 사용되며, Builder 패턴과 함께 사용하면 더욱 효과적임.
