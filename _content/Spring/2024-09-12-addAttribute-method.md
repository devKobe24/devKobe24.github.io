---
title: 🍃[Spring] `.addAttribute()` 메서드.
tags:
    - Spring
    - Framework
date: "2024-09-12"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] `.addAttribute()` 메서드.

## 1️⃣ `.addAttribute()` 메서드.
- `.addAttribute()`는 Spring MVC에서 [`Model` 또는 `ModelMap`](https://www.devkobe24.com/Backend/Spring/2024-09-12-Model-Object-at-the-Spring-MVC.html)객체의 메서드로, 컨트롤러에서 뷰로 전달할 데이터를 추가하는 데 사용됩니다.
- 이 메서드를 사용하여 컨트롤러가 처리한 결과를 뷰에 전달하고, 뷰는 이 데이터를 사용해 동적으로 콘텐츠를 렌더링합니다.

## 2️⃣ 주요 기능.
- **모델에 데이터 추가**
    - `addAttribute()`를 사용하여 키-값 쌍 형태로 데이터를 모델에 추가합니다.
        - 이 데이터는 이후 뷰에서 사용될 수 있습니다.
- **뷰 랜더링에 데이터 전달**
    - 모델에 추가된 데이터는 뷰 템플릿(예: Thymeleaf, JSP)에서 접근 가능하며, 이를 통해 클라이언트에게 동적으로 생성된 HTML을 반환할 수 있습니다.

## 3️⃣ 사용 예시.
```java
@Controller
public class HomeController {
    
    @GetMapping("/home")
    public String home(Model model) {
        // 모델에 데이터를 추가
        model.addAttribute("message", "Welcome to the Home Page!");
        model.addAttribute("date", LocalDate.now());
        
        // "home"이라는 뷰 이름을 반환
        return "home";
    }
}
```

- 위 코드에서 `home()` 메서드는 `/home` URL로 GET 요청이 들어오면 실행됩니다.
- `Model` 객체를 사용하여 `"message"`와 `"date"`라는 두 개의 속성을 추가합니다.
- 이 속성들은 "home"이라는 뷰(예: `home.html` 또는 `home.jsp`)에서 사용됩니다.

## 4️⃣ 뷰에서의 사용 예시(Thymeleaf)
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Home Page</title>
</head>
<body>
    <h1 th:text="${message}">Default Message</h1>
    <p>Today's date is: <span th:text="${date}">01/01/2024</span></p>
</body>
</html>
```

- 위의 Thymeleaf 템플릿에서 `${message}`와 `${date}`는 컨트롤러에서 `addAttribute()`를 통해 추가된 데이터를 나타냅니다.
- 컨트롤러에서 제공한 `"Welcome to the Home Page!"` 와 현재 날짜가 뷰에 렌더링됩니다.

## 5️⃣ `.addAttribute()`의 다양한 사용 방법.
- **1. 키와 값 쌍으로 사용**
    - 가장 기본적인 사용 방법으로, 첫 번째 인수로 속성의 이름을, 두 번째 인수로 속성의 값을 지정합니다.
```java
model.addAttribute("username", "JohnDoe");
```

- **2. 키 없이 값만 전달**
    - 키를 생략하고 값만 전달할 수도 있습니다.
        - 이 경우 Spring은 전달된 객체의 클래스 이름을 기본 키로 사용합니다.(첫 글자는 소문자로).
```java
User user = new User("John", "Doe");
model.addAttribute(user);
// "user"라는 키로 모델에 추가됩니다.
```

- **3. 체이닝(Chaining)**
    - `addAttribute()`는 메서드 체이닝이 가능하여, 여러 개의 속성을 한 번에 추가할 수 있습니다.
```java
model.addAttribute("name", "Jane")
     .addAttribute("age", 30)
     .addAttribute("city", "New York");
```

- `.addAttribute()`는 Spring MVC에서 컨트롤러와 뷰 사이의 데이터를 전달하는 중요한 역할을 하며, 동적인 웹 애플리케이션 개발에 필수적인 요소입니다.
