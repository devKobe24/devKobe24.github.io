---
title: 🍃[Spring] Controller에서 getter가 있는 객체를 반환하면 JSON이 된다?!
tags:
    - Spring
    - Framework
date: "2024-09-26"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Controller에서 getter가 있는 객체를 반환하면 JSON이 된다?!

- **Controller**에서 getter가 있는 객체를 반환하면 기본적으로 JSON으로 변환되어 클라이언트에 전달됩니다.
    - 이 동작은 **Spring Framework**에서 자동으로 처리해줍니다.

## 1️⃣ Spring에서 객체가 JSON으로 변환되는 과정.

### 1. 객체 반환 및 `@RestController`
- Spring에서는 `@RestController` 또는 `@ResponseBody` 애너테이션이 붙은 메서드가 객체를 반환하면, 해당 객체는 자동으로 **JSON** 형식으로 변환되어 클라이언트에 전달됩니다.
- Spring은 내부적으로 **Jackson**이라는 라이브러리를 사용하여 Java 객체를 **JSON**으로 직렬화합니다.
    - 이 과정에서 객체의 getter 메서드를 통해 데이터를 추출하려 JSON을 생성합니다.

### 2. Jackson에 의한 직렬화.
- Spring Boot는 **Jackson**을 기본으로 포함하고 있어, 추가적인 설정 없이 Java 객체를 JSON으로 변환할 수 있습니다.
- 객체의 getter 메서드를 통해 객체 내부의 데이터를 추출하고, 이를 JSON으로 변환합니다.
- 예를 들어, `getName()` 이라는 getter가 있으면, 이는 `name`이라는 필드로 JSON에 포함됩니다.

## 2️⃣ 예시.

### 1. 객체 클래스.

```java
public class User {
    private Long id;
    private String name;
    private String email;
    
    // 기본 생성자
    public User() {}
    
    // Getter와 Setter
    public Long getId() {
        return id;
    }
    
    public void setId(Long id) {
        this.id = id;
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public String getEmail() {
        return email;
    }
    
    public void setEmail(String email) {
        this.email = email;
    }
}
```

### 2. Controller 클래스.
```java
@RestController
@RequestMapping("/users")
public class UserController {
    
    @GetMapping("/{id}")
    public User getUserById(@PathVariable Long id) {
        // User 객체를 생성하고 반환 (일반적으로는 데이터베이스에서 가져옴)
        User user = new User();
        user.setId(id);
        user.setName("Kobe");
        user.setEmail("kobe@example.com");
        
        // 이 객체는 자동으로 JSON 형식으로 변환되어 클라이언트에 반환됨.
        return user;
    }
}
```

## 3️⃣ 동작 원리.

### 1. 클라이언트 요청.
- 클라이언트 `/users/1` 과 같이 요청을 보내면, Spring은 해당 경로에 매핑된 `getUserById` 메서드를 호출합니다.

### 2. Java 객체 반환.
- 이 메서드는 `User` 객체를 반환합니다.

### 3. 객체를 JSON으로 변환.
- Spring은 `@RestController` 또는 `@ResponseBody`를 보고, `User` 객체를 JSON 형식으로 변환합니다.
- 이때 Jackson 라이브러리가 getter 메서드들을 사용하여 객체 내부의 값을 추출하고, 이를 JSON으로 직렬화합니다.

> 🙋‍♂️ [라이브러리와 프레임워크의 차이점](https://www.devkobe24.com/CS/2024/2024-09-26-Library-and-Framework.html)

### 4. 클라이언트에 JSON 응답.
- 직렬화된 JSON 데이터가 클라이언트에 응답으로 전송됩니다.

#### 결과로 클라이언트가 받는 JSON
```json
{
    "id": 1,
    "name": "Kobe",
    "email": "kobe@example.com"
}
```

## 4️⃣ 주의 사항.
- **객체에 getter가 있어야 함.**
    - Jackson은 객체의 필드를 직접 접근하는 것이 아니라, 기본적으로 getter 메서드를 사용하여 데이터를 추출합니다.
    - 따라서 **getter 메서드가 존재해야 합니다.**

- **객체가 null인 경우.**
    - 객체가 null이거나, 특정 필드가 null인 경우 그 필드는 JSON에 포함되지 않거나 null로 표현됩니다.

- **추가 설정.**
    - 필요에 따라 Jackson의 직렬화/역직렬화 방식을 커스터마이징할 수 있습니다.
    - 예를 들어, 필드 이름을 변경하거나 특정 필드를 제외하고 싶을 때는 `@JsonProperty`, `@JsonIgnore` 같은 Jackson 애너테이션을 사용할 수 있습니다.

## 5️⃣ 요약.
- Java 백엔드 애플리케이션에서 계층형 아키텍처로 구현한 후 **Controller**에서 **getter**가 있는 **객체를 반환하면**, Spring은 해당 객체를 자동으로 **JSON 형식**으로 변환하여 클라이언트에 응답합니다.
- 이는 기본적으로 Spring의 **Jackson** 라이브러리를 통해 이루어지며, 추가적인 설정 없이도 객체의 getter를 통해 JSON으로 직렬화됩니다.
