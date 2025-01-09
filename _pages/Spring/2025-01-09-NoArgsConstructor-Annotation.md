---
title: 🍃[Spring] `@NoArgsConstructor`은 무엇일까요?
tags:
    - Spring
    - Framework
date: "2025-01-09"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] `@NoArgsConstructor`은 무엇일까요?
## 📌 Intro.
- ↘︎ `@NoArgsConstructor`는 **Lombok**에서 제공하는 어노테이션으로, **파라미터가 없는 기본 생성자를 자동으로 생성해줍니다.**

## ✅1️⃣ 주요 특징.
### 1. 기본 생성자 추가.
- ↘︎ `@NoArgsConstructor`를 사용하면 클래스에 기본 생성자를 명시적으로 작성하지 않아도 됨.
- ↘︎ 자동 생성된 기본 생성자는 **필드 초기화 로직이 없는 기본적인 생성자임.**

### 2. 주로 객체 생성에 사용.
- ↘︎ JPA, Jackson 같은 라이브러리에서 객체를 인스턴스화할 때 기본 생성자가 필요함.

### 3. final 필드와 호환 가능.
- ↘︎ 기본적으로 `final` 필드가 있으면 컴파일 에러가 발생함.
- ↘︎ 이를 해결하기 위해 `@NoArgsConstructor(force = true)`를 사용할 수 있음.

## ✅2️⃣ 사용 예제.
### 1. 기본 사용.
```java
import lombok.NoArgsConstructor;

@NoArgsConstructor
public class MyClass {
    private String name;
    private int age;
}
```
- ↘︎ 위 코드는 다음과 같은 기본 생성자를 생성함.
```java
public MyClass() {
}
```

### 2. JPA에서의 사용.
- ↘︎ JPA에서는 **엔티티 클래스**에 기본생성자가 필요함.
    - ↘︎ `@NoArgsConstructor`를 사용하여 간편하게 기본 생성자를 추가할 수 있음.
```java
import jakarta.persistence.Entity;
import lombok.NoArgsConstructor;

@Entity
@NoArgsConstructor
public class User {
    private Long id;
    private String username;
}
```
- ↘︎ JPA는 엔티티 클래스를 리플렉션을 통해 인스턴스화하기 때문에 기본 생성자가 필수임.

### 3. force 속성 사용 (final 필드와 함께)
- ↘︎ 기본 생성자는 `final` 필드 초기화를 강제하지 않음.
    - ↘︎ 하지만 `@NoArgsConstructor(force = true)`를 사용하면 `final` 필드도 기본값으로 초기화되도록 처리할 수 있음.
```java
import lombok.NoArgsConstructor;

@NoArgsConstructor(force = true)
public class MyClass {
    private final String name; // 기본값: null
    private final int age; // 기본값: 0
}
```
- ↘︎ 위 코드는 다음과 같은 생성자를 생성함.
```java
public MyClass() {
    this.name = null;
    this.age = 0;
}
```
- ↘︎ `force = true`를 사용하면 기본 생성자를 통해 `final` 필드도 초기화가 가능해지므로 특정 상황에서 유용함.

## ✅3️⃣ Spring과 함께 사용.
### Spring에서 객체 생성 시 활용.
📌 Spring에서 JSON 요청 본문을 처리하거나 JPA 엔티티를 관리할 때 기본 생성자가 필요함.
```java
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

@Getter
@Setter
@NoArgsConstructor
public class UserDTO {
    private String username;
    private String email;
}
```
- ↘︎ JSON 요청을 처리하는 라이브러리(Jackson 등)가 기본 생성자를 사용하여 DTO 객체를 인스턴스화 함.
- ↘︎ `@NoArgsConstructor`룰 사용하여 간편하게 이를 지원할 수 있음.

## ✅4️⃣ 장점.
### 1. 코드 간결화.
- ↘︎ 기본 생성자를 자동으로 생성하므로 수동으로 작성할 필요가 없음.
### 2. 라이브러리 호환성.
- ↘︎ JPA, Jackson 등 기본 생성자가 필요한 라이브러리와 쉽게 호환됨.
### 3. 특수 상황 지원.
- ↘︎ `force - true`를 통해 `final` 필드도 기본값으로 초기화할 수 있음.

## ✅5️⃣ 주의점.
### 1. `force = true` 사용 시 초기화 주의.
- ↘︎ `final` 필드가 기본값(null 또는 0)으로 초기화되기 때문에 의도치 않은 동작을 유발할 수 있음.
### 2. JPA와 함께 사용할 때 접근 제한자.
- ↘︎ JPA에서는 기본 생성자가 `protected` 이상이어야 하므로 다음과 같이 설정하는 것이 좋음
```java
@NoArgsConstructor(access = AccessLevel.PROTECTED)
```

## 🚀 정리.
- ↘︎ `@NoArgsConstructor`는 기본 생성자를 자동으로 생성해주는 Lombok 어노테이션으로, JPA와 같은 라이브러리와 호환성을 높이고 코드의 가독성을 개선하는 데 유용함.
    - ↘︎ 추가로 `force` 옵션을 통해 `final` 필드도 초기화할 수 있음.
