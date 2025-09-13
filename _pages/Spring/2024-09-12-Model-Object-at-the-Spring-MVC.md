---
title: 🍃[Spring] Spring MVC에서 `Model` 객체.
tags:
    - Spring
    - Framework
date: "2024-09-12"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Spring MVC에서 `Model` 객체.

## 1️⃣ `Model` 객체.
- Spring MVC에서 `Model` 객체는 컨트롤러와 뷰 사이에서 데이터를 전달하는 역할을 합니다.
- 컨트롤러에서 생성된 데이터를 `Model` 객체에 담아두면, 이 데이터는 뷰 템플릿(예: Thymeleaf, JSP)에서 사용될 수 있습니다.
- `Model` 객체는 요청에 대한 응답으로 어떤 데이터를 뷰로 보내야 하는지 결정하는 데 사용됩니다.

## 2️⃣ `Model` 객체의 주요 기능.
- **1. 데이터 저장 및 전달.**
    - `Model` 객체에 데이터를 저장하면, 해당 데이터는 뷰에 전달되어 클라이언트에게 표시됩니다.
        - 예를 들어, 사용자의 이름이나 리스트와 같은 데이터를 뷰에 전달할 수 있습니다.
- **2. 키-값 쌍 형태로 데이터 관리.**
    - `Model` 객체는 데이터를 키-값 쌍 형태로 관리합니다.
        - 뷰에서 이 데이터를 사용할 때는 키를 통해 접근합니다.
- **3. 뷰 템플릿에서 데이터 사용.**
    - `Model`에 추가된 데이터는 뷰 템플릿에서 변수로 사용됩니다.
        - 뷰 템플릿 엔진(예: Thymeleaf, JSP)은 이 데이터를 이용해 동적인 HTML을 생성하고, 이를 클라이언트에게 반환합니다.

## 3️⃣ `Model` 객체의 사용 예.
```java
@Controller
public class GreetingController {
    
    @GetMapping("/greeting")
    public String greeting(Model model) {
        model.addAttribute("name", "John");
        model.addAttribute("message", "Hello welcome to our website!");
        
        // "greeting"이라는 뷰 이름을 반환
        return "greeting";
    }
}
```

- 위 예제에서 `greeting()` 메서드는 `Model` 객체에 `"name"` 과 `"message"` 라는 두 가지 데이터를 추가합니다.
    - 이 데이터는 뷰 이름 `"greeting"`에 전달되며, 해당 뷰 템플릿에서 사용될 수 있습니다.

## 4️⃣ 뷰 템플릿에서의 사용(Thymeleaf)
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Greeting Page</title>
</head>
<body>
    <h1 th:text="${message}">Default Message</h1>
    <p>Hello, <span th:text="${name}">User</span>!</p>
</body>
```

- 이 Thymeleaf 템플릿에서 `${message}` 와 `${name}` 은 `Model` 객체에 담긴 데이터를 사용해 동적으로 콘텐츠를 생성합니다.
    - 컨트롤러에서 `Model`에 추가된 `"Hello, welcome to our website!"`와 `"John"`이 뷰에 렌더링됩니다.

## 5️⃣ `Model`, `ModelMap`, `ModelAndView`
- Spring MVC에서 `Model` 외에도 `ModelMap`과 `ModelAndView`라는 유사한 객체들이 있습니다.
    - **Model :** 간단한 인터페이스로, 데이터 저장 및 전달을 위한 가장 기본적인 방법을 제공합니다.
    - **ModelMap :** `Model`의 구현체로, 데이터를 맵 형식으로 관리합니다.
    - **ModelAndView :** 모델 데이터와 뷰 이름을 함께 반환할 때 사용됩니다. 한 번에 모델과 뷰 정보를 모두 설정할 수 있습니다.

## 6️⃣ 요약.
- `Model` 객체는 Spring MVC에서 컨트롤러가 뷰에 데이터를 전달하는 데 사용되는 중요한 구성 요소입니다.
    - 이를 통해 동적인 웹 페이지를 쉽게 생성할 수 있으며, 애플리케이션의 응답 결과를 클라이언트에게 효과적으로 전달할 수 있습니다.
