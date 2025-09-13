---
title: "☕️[Java] attribute의 의미와 역할"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-07-31"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] attribute의 의미와 역할.

- Java 백엔드 개발에서 "attribute"라는 용어는 몇 가지 다른 맥락에서 사용될 수 있습니다.
- 주로 두 가지 의미로 사용되는 경우가 많은데, **클래스의 속성** 을 의미하는 경우와 **웹 개발**에서 HTTP 요청이나 세션과 관련된 데이터를 지칭하는 경우입니다.

## 1️⃣ 클래스의 속성(Field or Property)
- Java에서 클래스의 **"attribute"** 는 해당 클래스의 상태를 정의하는 변수를 말합니다.
    - 이러한 변수들은 객체의 데이터이터를 저장하고, 클래스의 인스턴스들이 갖는 특징과 상태 정보를 나타냅니다.
        - 예를 들어, **'Person'** 클래스가 있다면, **'name', 'age'** 같은 필드들이 이 클래스의 **"attribute"** 가 됩니다.

```java
public class Person {
    private String name; // Attribute
    private int age; // Attribute
    
    // Constructors, getters, setters 등
}
```

## 2️⃣ 웹 개발에서의 Attribute
- 웹 개발에서 **"attribute"** 는 주로 **세션(Session)이나 요청(Request) 객체에 저장된 데이터를 지칭** 합니다.
    - 이 데이터는 사용자가 웹 사이트를 이용하는 동안 지속되거나 요청 동안에만 존재할 수 있습니다.
        - 예를 들어, 사용자가 로그인을 하면 그 사용자의 정보를 **세션 attribute로 저장**하여 다른 페이지에서도 사용자 정보를 유지할 수 있게 합니다.

```java
// 세션에 사용자 정보 저장
request.getSession().setAttribute("user", userObject);

// 세션에서 사용자 정보 가져오기
User user = (User) request.getSession().getAttribute("user");
```

- 이 두 가지 사용 사례는 Java 백엔드 개발에서 매우 흔하게 접할 수 있으며, 각각의 맥락에서 attribute가 가지는 의미와 역할을 이해하는 것은 중요합니다.
    - 첫 번째 경우는 객체 지향 프로그래밍의 핵심 요소로 클래스의 속성을 정의합니다.
    - 두 번째 경우에는 웹 애플리케이션의 상태 관리를 돕는 수단으로서 활용됩니다.
