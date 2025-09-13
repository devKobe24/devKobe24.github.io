---
title: 🍃[Spring] `@RequireArgsConstructor`은 무엇일까요?
tags:
    - Spring
    - Framework
date: "2025-01-09"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] `@RequireArgsConstructor`은 무엇일까요?
## 📌 Intro.
- ↘︎ `@RequireArgsConstructor`는 **Lombok**에서 제공하는 어노테이션으로, **클래스의 `final`로 선언된 필드나 `@NonNull`로 지정된 필드에 대해 필수 생성자를 자동으로 생성함.**

## ✅1️⃣ 주요 특징.
### 1. `final` 필드만 포함.
- ↘︎ `final`로 선언된 필드가 대상이 됨.
- ↘︎ `final`이 아닌 필드는 생성자에 포함되지 않음.

### 2. `@NonNull` 필드 포함.
- ↘︎ `@NonNull` 어노테이션이 붙은 필드도 포함됨.
- ↘︎ `@NonNull`이 붙은 필드는 생성자에서 null 체크를 자동으로 추가함.

### 3. 주로 의존성 주입에 사용.
- ↘︎ Spring에서 **생성자 기반 의존성 주입(Constructor-based Dependency Injection)에** 자주 사용됨.

## ✅2️⃣ 생성자 자동 생성 예제.
### 1. 기본 사용.
```java
import lombok.RequiredArgsConstructor;

@RequiredArgsConstructor
public class MyService {
    private final MyRepository myRepository;
    private final String name; // 생성자 포함.
    private int age; // 생성자 포함되지 않음.
}
```
- ↘︎ 위 코드는 다음과 같은 생성자를 자동으로 생성함:
```java
public MyService(MyRepository myRepository, String name) {
    this.myRepository = myRepository;
    this.name = name;
}
```

### 2. `@NonNull` 필드 사용.
```java
import lombok.NonNull;
import lombok.RequiredArgsConstructor;

@RequiredArgsConstructor
public class MyService {
    @NonNull
    private final MyRepository myRepository; // @NonNull null 체크 추가.
    private final String name; // 생성자에 포함.
    private int age; // 생성자에 포함되지 않음.
}
```
- ↘︎ 생성된 생성자는 `@NonNull` 필드에 대해 null 체크를 포함함.
```java
public MyService(MyRepository myRepository, String name) {
    if (myRepository == null) {
        throw new NullPointerException("myRepository is marked @NonNull but is null");
    }
    this.myRepository = myRepository;
    this.name = name;
}
```

### 3. Spring과 함께 사용.
📌 Spring에서는 `@RequiredArgsConstructor`를 사용하면 **생성자 주입** 코드를 간결하게 작성할 수 있음.
- ↘︎ **일반적인 생성자 주입**
```java
@Service
public class MyService {
    private final MyRepository myRepository;
    
    public MyService(MyRepository myRepository) {
        this.myRepository = myRepository;
    }
}
```
- ↘︎ **`@RequiredArgsConstructor` 사용**
```java
@Service
@RequiredArgsConstructor
public class MyService {
    private final MyRepository myRepository;
}
```
- ↘︎ Spring이 자동으로 MyRepository를 주입함.
- ↘︎ 코드가 간결해지고, 의존성 주입 시 더 가독성이 좋아짐.

## ✅3️⃣ 장점.
### 1. 코드 간결화.
- ↘︎ 필수 생성자를 직접 작성할 필요가 없어짐.

### 2. 불변 객체 생성에 적합.
- ↘︎ `final` 필드를 사용하면 해당 필드를 불변으로 유지할 수 있음.

### 3. Spring과의 호환성.
- ↘︎ 생성자 기반 의존성 주입에서 활용도가 높음.

## ✅4️⃣ 주의점.
### 1. `final` 필드만 포함.
- ↘︎ 생성자에 포함되지 않는 필드(`final`이 아닌 필드)에 값을 설정하려면 `Setter` 또는 다른 방법을 사용해야 함.

### 2. `@NonNull`로 null 체크 필요.
- ↘︎ `@NonNull` 필드를 사용하지 않으면 null 값이 들어가도 예외가 발생하지 않음.

## 🚀 정리.
- ↘︎ `@RequiredArgsConstructor`는 생성자 주입을 간결하게 하고, 불변성을 유지하면서 코드를 깔끔하게 작성할 수 있도록 도와줌.
- ↘︎ Spring과 Lombok을 함께 사용할 때 가장 많이 쓰이는 Lombok 어노테이션 중 하나임.
