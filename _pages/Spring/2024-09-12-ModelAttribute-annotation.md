---
title: 🍃[Spring] `@ModelAttribute` 애노테이션
tags:
    - Spring
    - Framework
date: "2024-09-12"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] `@ModelAttribute` 애노테이션.

## 1️⃣ `@ModelAttribute`
- `@ModelAttribute`는 Spring Framework에서 주로 사용되는 애노테이션으로, Spring MVC에서 모델 데이터와 요청 데이터를 바인딩하거나, 컨트롤러에서 반환된 데이터를 뷰에 전당하는 데 사용됩니다.
- `@ModelAttribute`는 두 가지 주요 용도로 사용됩니다.

## 1️⃣ 메서드 파라미터에서 사용(`@ModelAttribute` 파라미터 바인딩)
- `@ModelAttribute`를 메서드 파라미터에 적용하면, Spring은 요청 파라미터 또는 폼 데이터로부터 객체를 자동으로 생성하고, 그 객체의 필드를 요청 파라미터 값으로 바인딩합니다.
    - 이렇게 생성된 객체는 컨트롤러 메서드 내부에서 사용할 수 있습니다.

### 예시.
```java
@Controller
public class UserController {
    
    @PostMapping("/register")
    public String registerUser(@ModelAttribute User user) {
        // User 객체는 폼 데이터로부터 자동으로 생성되고 바인딩됩니다.
        // 이제 user 객체를 사용할 수 있습니다.
        System.out.println("User Name: " + user.getName());
        return "registrationSuccess";
    }
}
```

- 위 예시에서 `@ModelAttribute`는 `User` 객체를 폼 데이터로부터 생성하고, `name`, `email` 등의 필드 요청 파라미터 값으로 바인딩합니다.

## 2️⃣ 메서드에 사용(`@ModelAttribute` 메서드)
- `@ModelAttribute`를 메서드 레벨에 적용하면, 해당 메서드는 컨트롤러의 모든 요청 전에 실행되어, 반환된 객체를 모델에 추가합니다.
    - 이 객체는 이후 뷰에서 사용될 수 있습니다.
```java
@Controller
public class UserController {
    
    @ModelAttribute("userTypes")
    public List<String> populateUserTypes() {
        // 이 메서드는 컨트롤러의 모든 요청 전에 실행됩니다.
        // 이 리스트는 모델에 추가되어 뷰에서 사용될 수 있습니다.
        return Arrays.asList("Admin", "User", "Guest");
    }
    
    @GetMapping("/register")
    public String showRegistrationForm(Model model) {
        model.addAttribute("user", new User());
        return "register";
    }
}
```

- 위 예시에서 `populateUserTypes()` 메서드는 `@ModelAttribute("userTypes")`로 선언되어, 이 메서드가 반환하는 리스트는 `userTypes`라는 이름으로 모델에 추가됩니다.
    - 이 값은 뷰(예: JSP, Thymeleaf 템플릿 등)에서 접근할 수 있습니다.

## 3️⃣ `@ModelAttribute` 의 주요 기능 요약.
- **1. 파라미터 바인딩 :** 클라이언트의 요청 데이터를 객체로 변환하고, 이 객체를 컨트롤러 메서드에서 사용하도록 해줍니다.
- **2. 모델 추가 :** 메서드의 반환 값을 모델에 추가하여, 해당 데이터가 모든 뷰에서 사용될 수 있도록 합니다.
    - 이 두가지 기능을 통해 `@ModelAttribute`는 Spring MVC에서 데이터 바인딩과 뷰에 전당되는 데이터의 관리를 간소화하는 데 중요한 역할을 합니다.
