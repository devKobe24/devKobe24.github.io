---
title: 🍃[Spring] Spring MVC에서 `Controller` 객체.
tags:
    - Spring
    - Framework
date: "2024-09-11"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Spring MVC에서 `Controller` 객체.

## 1️⃣ `Controller` 객체.
- Spring MVC에서 `Controller` 객체는 애플리케이션에서 HTTP 요청을 처리하고, 해당 요청에 대해 적절한 응답을 생성하는 역할을 하는 구성 요소입니다.
- `Controller`는 사용자 입력을 처리하고, 비즈니스 로직을 수행한 후, 뷰를 선택하여 결과를 클라이언트에게 반환합니다.
- Spring MVC에서 `Controller`는 주로 `@Controller` 또는 `@RestController` 애노테이션으로 정의됩니다.

### 1️⃣ 주요 역할.
- **1. HTTP 요청 처리**
    - `Controller` 객체는 특정 URL 경로와 매핑된 HTTP 요청(GET, POST, PUT, DELETE 등)을 처리합니다.
        - 각 요청은 컨트롤러의 특정 메서드와 연결되며, 메서드는 요청 매개변수를 처리하고, 필요한 작업을 수행합니다.
- **2. 비즈니스 로직 수행**
    - 요청을 처리하는 동안, `Controller`는 서비스 계층이나 비즈니스 로직을 호출하여 필요한 처리를 수행합니다.
        - 이는 데이터베이스에서 데이터를 가져오거나, 계산을 수행하거나, 다른 복잡한 작업을 포함할 수 있습니다.
- **3. 모델 데이터 준비**
    - `Controller`는 뷰에 전달할 데이터를 준비하고, 이를 `Model` 객체에 담아 뷰로 전달합니다.
        - 이 데이터는 뷰 템플릿에서 사용되어 클라이언트에게 보여질 콘텐츠를 동적으로 생성합니다.
- **4. 뷰 선택 및 응답 생성**
    - `Controller`는 처리 결과에 따라 어떤 뷰를 사용할지 결정합니다.
        - 일반적으로 뷰의 이름을 반환하거나, `ModelAndView` 객체를 통해 뷰와 모델 데이터를 함께 반환합니다.
        - `@RestController`를 사용하면 JSON, XML 등의 형식으로 직접 데이터를 반환할 수도 있습니다.

### 2️⃣ `Controller` 객체의 정의.
- Spring MVC에서 `Controller`는 `@Controller` 애노테이션으로 정의됩니다.
    - 이 애노테이션은 클래스가 컨트롤러 역할을 한다는 것을 Spring에게 알리며, HTTP 요청을 처리할 수 있게 합니다.

#### 예제: 기본 컨트롤러
```java
@Controller
public class GreetingController {
    
    @GetMapping("/greeting")
    public String greeting(Model model) {
        model.addAttribute("message", "Hello, welcome to our website!");
        return "greeting";
    }
}
```

- 위 코드에서 `GreetingController` 클래스는 `@Controller` 애노테이션을 통해 컨트롤러로 정의됩니다.
- `/greeting` 경로로 GET 요청이 들어오면 `greeting()` 메서드가 호출되며, `"message"` 라는 데이터를 모델에 추가하고, `"greeting"` 이라는 뷰 이름을 반환합니다.
    - Spring MVC는 이 뷰 이름을 기반으로 실제 뷰를 랜더링합니다.

### 3️⃣ `@RestController` 사용.
- `@RestController`는 `@Controller`와 `@ResponseBody`를 결합한 애노테이션입니다.
    - 이 애노테이션이 적용된 클래스는 JSON이나 XML과 같은 데이터 형식으로 직접 응답을 반환하는 RESTful 웹 서비스의 컨트롤러로 동작합니다.

#### 예제: REST 컨트롤러
```java
@RestController
public class ApiController {
    
    @GetMapping("/api/greeting")
    public Map<String, String> greeting() {
        Map<String, String> response = new HashMap<>();
        response.put("message", "Hello, welcome to our API!");
        return response;
    }
}
```
- 위 코드에서 `ApiController` 클래스는 `@RestController` 애노테이션을 사용하여 정의됩니다.
    - `/api/greeting` 경로로 GET 요청이 들어오면 `greeting()` 메서드는 JSON 형식의 응답을 반환합니다.

### 4️⃣ 요청 매핑.
- `Controller`는 URL 경로 및 HTTP 메서드와 매핑된 메서드를 통해 요청을 처리합니다.
    - Spring MVC는 이를 위해 `@RequestMapping`, `@GetMapping`, `@PostMapping` 등 다양한 매핑 애노테이션을 제공합니다.

#### 예제: 요청 매핑.
```java
@Controller
@RequestMapping("/users")
public class UserController {
    
    @GetMapping("/{id}")
    public String getUser(@PathVariable("id") Long id, Model model) {
        User user = userService.findUserById(id);
        model.addAttribute("user", user);
        return "userProfile";
    }
    
    @PostMapping("/create")
    public String createUser(@ModelAttribute User user) {
        userService.saveUser(user);
        return "redirect:/users/" + user.getId();
    }
}
```

- 위 예제에서 `UserController`는 `/users` 경로와 관련된 요청을 처리합니다.
    - `/users/{id}` 경로로 GET 요청이 들어오면, 해당 사용자의 프로필 정보를 조회하여 `"userProfile"` 뷰에 전달합니다.
    - `/users/create` 경로로 POST 요청이 들어오면, 새로운 사용자를 생성하고, 해당 사용자의 프로필 페이지로 리다이렉트합니다.

### 5️⃣ 요약.
- Spring MVC에서 `Controller` 객체는 클라이언트의 HTTP 요청을 처리하고, 비즈니스 로직을 실행한 후, 적절한 응답을 생성하는 핵심 구성 요소입니다.
- `Controller`는 요청과 비즈니스 로직, 그리고 뷰 렌더링을 연결하는 역할을 하며, Spring MVC 애플리케이션에서 중요한 역할을 수행합니다.
