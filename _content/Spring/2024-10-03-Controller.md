---
title: 🍃[Spring] 계층형 아키텍처에서 Controller의 역할.
tags:
    - Spring
    - Framework
date: "2024-10-03"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 계층형 아키텍처에서 Controller의 역할.

Java 백앤드 애플리케이션의 **계층형 아키텍처(Layerd Architecture)** 에서 **Controller**는 사용자(클라이언트)로부터 요청을 받아 처리하고, 그에 대한 응답을 반환하는 역할을 담당하는 **중요한 계층**입니다.

Controller는 애플리케이션의 외부 인터페이스로 작동하며, 주로 웹 요청과 응답의 흐름을 제어하는 역할을 수행합니다.

## 1️⃣ Controller의 주요 역할.

### 1. 요청 수신 및 응답 반환.
- Controller는 클라이언트(브라우저, 모바일 앱 등)로 부터 HTTP 요청을 수신합니다.
- 각 요청은 URL 경로 및 HTTP 메서드(GET, POST, PUT, DELETE 등)에 따라 Controller의 특정 메서드에 매핑됩니다.
- 이 메서드는 비즈니스 로직을 호출하고, 처리 결과를 다시 클라이언트에게 응답으로 반환합니다.

### 2. 요청 검증.
- Controller는 클라이언트로부터 전달된 요청 데이터(쿼리 파라미터, 폼 데이터, JSON 등)를 검증하는 역할을 합니다.
- 이 검증 작업은 일반적으로 요청이 비즈니스 로직으로 전달되기 전에 수행됩니다.
- 검증 실패 시, 에러 메시지나 상태 코드를 클라이언트에 반환합니다.

### 3. 비즈니스 로직 호출.
- Controller는 직접적으로 비즈니스 로직을 수행하지 않고, **Service 계층**의 메서드를 호출합니다.
- 이 방식은 역할을 분리하여 유지보수성을 높이고, 코드가 더욱 이해하기 쉽게 만듭니다.
- Service 계층이 비즈니스 로직을 처리하고 결과를 반환하면, Controller는 이를 클라이언트에게 전달합니다.

### 4. 뷰와의 통신(View와 연동)
- Controller는 **View**와 통신하여 적절한 응답을 생성합니다.
- 웹 애플리케이션에서는 HTML이나 JSON 데이터를 반환하는 것이 일반적입니다.
- 예를 들어, 템플릿 엔진(Thymeleaf 등)을 사용해 동적인 HTML 페이지를 렌더링하거나, API 서버의 경우 JSON 포맷으로 데이터를 반환합니다.

### 5. HTTP 상태 코드 관리.
- Controller는 요청 처리 결과에 따라 적절한 **HTTP 상태 코드(예: 200 OK, 400 Bad Request, 404 Not Found, 500 Internal Server Error 등)** 를 설정하여 클라이언트에 전달합니다.
- 이로써 클라이언트가 요청 처리 상태를 알 수 있도록 합니다.

### 6. 예외 처리
- Controller는 애플리케이션에서 발생하는 예외를 처리하거나, 전역적인 예외 처리 메커니즘을 사용해 예외를 다룹니다.
- 이를 통해 적절한 에러 응답을 클라이언트에게 반환하고, 에러가 발생했을 때 애플리케이션이 비정상적으로 종료되지 않도록 합니다.

## 2️⃣ 예시 코드

Spring Boot 애플리케이션에서의 Controller 예시를 통해 그 역할을 더 구체적으로 볼 수 있습니다.

```java
@RestController
@RequestMapping("/users")
public class UserController {
    
    private final UserService userService;
    
    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<UserDTO> getUser(@PathVariable Long id) {
        UserDTO user = userService.getUserById(id);
        return ResponseEntity.ok(user);
    }
    
    @PostMapping
    public ResponseEntity<UserDTO> createUser(@RequestBody @Valid UserDTO userDTO) {
        UserDTO createdUser = userService.createUser(userDTO);
        return ResponseEntity.status(HttpStatus.CREATED).body(createdUser);
    }
}
```

## 3️⃣ 설명.
- `@RestController`
    - Spring에서 RESTful 웹 서비스를 만들기 위해 사용되는 어노테이션입니다.
    - JSON 또는 XML 데이터를 반환하는 컨트롤러를 정의합니다.
- `@RequestMapping`
    - 이 컨트롤러가 `\users` 경로에 대한 요청을 처리하도록 설정합니다.
- `@GetMapping`
    - HTTP GET 요청을 처리하는 메서드로, `id` 경로 변수에 해당하는 사용자를 가져옵니다.
- `@PostMapping`
    - HTTP POST 요청을 처리하는 메서드로, 새로운 사용자를 생성하고 결과를 반환합니다.
- `@RequestBody`, `@Valid`
    - 요청 본문 데이터를 객체로 매핑하고, 입력 데이터를 검증합니다.
- `ResponseEntity`
    - 응답 본문과 상태 코드를 포함해 클라이언트에게 응답을 반환합니다.

## 4️⃣ 결론
Contoller는 사용자 요청을 받아 Service 계층과 통신하며, 요청을 처리하고 그 결과를 클라이언트에게 반환하는 역할을 합니다.
이러한 역할 분리를 통해 애플리케이션은 더욱 유연하고 관리하기 쉬운 구조를 갖추게 됩니다.
