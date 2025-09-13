---
title: "📚[Backend Development] Java Spring 프레임워크의 계층 - Presentation Layer(표현 계층)"
tags:
    - Backend Ddevelopment
    - Component
    - Layer
    - Architecture
date: "2025-08-03"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 📚[Backend Development] Java Spring 프레임워크의 계층 1️⃣
## ✅ Presentation Layer(표현 계층)
- **주요 컴포넌트 :** `@Controller`, `@RestController`
- **역할 :** 사용자의 HTTP 요청(Request)을 받고, 그 결과를 응답(Response)으로 보내주는 **애플리케이션의 진입점**입니다.
- **설명 :** 사용자의 입력을 받아 유효성을 검사하고, 비즈니스 계층으로 데이터 처리를 위임합니다. 비즈니스 계층에서 처리된 결과는 다시 이 계층에서 사용자에게 보여줄 형식(HTML, JSON 등)으로 변환되어 전달됩니다.

## ✅ 1. Presentation Layer는 언제 사용하나요?

외부 클라이언트(웹 브라우저, 모바일 앱 등)가 애플리케이션의 **기능에 접근해야 할 때 항상 사용**합니다.
사용자가 웹사이트의 버튼을 클릭하거나, 앱이 서버로부터 데이터를 가져오는 모든 순간에 이 계층이 가장 먼저 동작합니다.

## ✅ 2. Presentation Layer는 어디서 사용하나요?

애플리케이션 아키텍처의 **가장 바깥쪽 계층에 위치**합니다.
외부 클라이언트와 내부 비즈니스 계층(Service Layer) 사이의 **중간 다리** 역할을 하며, 시스템의 '현관' 또는 '안내 데스크'라고 생각할 수 있습니다.

## ✅ 3. Presentation Layer는 어떻게 사용하나요?

주로 **어노테이션 기반**으로 사용하며, 요청을 처리하고 응답을 반환하는 방식으로 동작합니다.
- `@RestController`, `@Controller` 어노테이션으로 클래스를 지정해 요청을 받을 수 있는 상태로 만듭니다.
- `@GetMapping`, `@PostMapping` 등으로 특정 URL과 HTTP Method에 맞는 처리 메서드를 연결합니다.
- 메서드 내에서는 클라이언트가 보낸 데이터를 받아 유효성을 검사한 후, 처리를 **서비스 계층(Service Layer)에 위임**합니다.
- 서비스로부터 받은 결과를 클라이언트에게 **JSON 데이터나 HTML 화면의 형태로 반환**합니다.

## ✅ 4. Presentation Layer는 왜 사용하나요?

가장 중요한 이유는 **관심사의 분리(Separation of Concerns, SoC)** 를 통해 시스템을 더 체계적이고 유연하게 만들기 위함입니다.
- **역할과 책임 명확화 :** 이 계층은 '웹 요청과 처리'라는 책임만 가집니다. 덕분에 비즈니스 계층은 '핵심 로직 처리'에만 집중할 수 있어 코드 이해가 쉬워집니다.
- **유지보수 용이성 :** 프론트엔트에 보여주는 데이터 형식을 변경해야 할 때, 다른 비즈니스 로직은 건드리지 않고 Presentation Layer만 수정하면 되므로 관리가 편합니다.
- **유연성 및 확장성 :** 웹(HTML)으로 서비스를 제공하다가 모바일 앱을 위한 API가 추가로 필요해져도, 기존 비즈니스 로직은 그대로 두고 새로운 Controller만 추가하면 되므로 확장이 용이합니다.

### ✅ Controller의 역할은 무엇안가요?

Controller의 핵심 역할은 **HTTP 요청을 받아, 그에 맞는 서비스(기능)를 호출하고, 처리된 결과를 다시 사용자에게 응답(Response)하는 것**입니다.

조금 더 구체적으로는 다음과 같은 역할을 담당합니다.

- **요청 접수 (Endpoint Mapping) :** 사용자가 보낸 URL과 HTTP Method(GET, POST 등)를 보고, 어떤 메서드가 이 요청을 처리해야 할지 결정합니다.
- **데이터 수신 및 검증 (Data Binding & Validation) :** 사용자가 보낸 데이터(예: 회원가입 정보)를 자바 객체로 변환하고, 데이터가 유효한지(예: 이메일 형식이 맞는지) 검사합니다.
- **서비스에 작업 위임 (Delegate to Service) :** Controller는 비즈니스 로직을 직접 처리하지 않습니다. 데이터 처리가 필요한 실제 작업은 **서비스(Service) 계층에 위임**합니다.
- **결과 반환 (Return Response) :** 서비스로부터 받은 처리 결과를 사용자에게 적절한 형태로(주로 JSON 또는 HTML) 변환하여 반환합니다.

**📝 비유 :** 레스토랑의 '웨이터'와 같습니다. 손님(Client)의 주문(Request)을 받아 주방(Service)에 전달하고, 완성된 요리(Data)를 손님에게 다시 서빙(Response)하는 역할을 합니다.
웨이터가 직접 요리하지 않는 것처럼, Controller도 직접 비즈니스 로직을 처리하지 않습니다.

### ✅ Controller는 언제 사용하나요?

**웹을 통해 외부에서 애플리케이션의 특정 기능에 접근해야 할 때** 항상 사용합니다.

- 사용자가 웹사이트에서 **버튼을 클릭**하거나 **페이지를 요청**할 때
- 모바일 앱에서 서버의 **데이터를 불러오거나 저장**할 때
- 다른 서버(시스템)가 우리 애플리케이션의 **API를 호출**할 때

즉, 클라이언트(브라우저, 앱 등)와 서버의 **비즈니스 로직을 연결하는 모든 접점**에서 Controller가 사용됩니다.

### ✅ Controller는 어떻게 사용하나요?

주로 어노테이션(`@`)을 사용하여 클래스와 메서드의 역할을 지정하는 방식으로 사용합니다.

아래는 간단한 사용자 정보 조회 API의 Controller 예시 코드입니다.

```java
// 이 클래스가 API 요청을 처리하는 Controller임을 선언합니다.
// @RestController는 각 메서드의 반환 값을 자동으로 JSON 형태로 변환해줍니다.
@RestController
public class UserController {
    
    // 비즈니스 로직을 처리할 UserService를 연결합니다. (의존성 주입)
    private final UserService userService;
    
    public UserController(UserService userService) {
        this.userService = userService;
    }
    
    /**
     * 모든 사용자 목록을 조회하는 API
     * HTTPGET 요청 & URL "/api/users"에 매핑됩니다.
     */
    @GetMapping("/api/users")
    public List<User> getAllUsers() {
        // 실제 데이터 조회는 Service에게 위임합니다.
        return userService.findAllUsers();
    }
    
    /**
     * 특정 ID의 사용자 한 명을 조회하는 API
     * URL 경로의 {id} 부분을 파라미터로 받습니다.
     * 예: /api/users/1 -> id 변수에 1이 담깁니다.
     */
    @GetMapping("/api/users/{id}")
    public User getUserById(@PathVariable Long id) {
        // Service에게 id를 전달하여 데이터 조회를 위임합니다.
        return userService.findUserById(id);
    }
}
```

**코드 사용법 요약:**

1. 클래스 위에 **`@RestController`** 를 붙여 Controller임을 명시합니다.
2. 요청을 처리할 메서드 위에 `@GetMapping`, `@PostMapping` 등으로 어떤 HTTP 요청과 URL에 응답할지 지정합니다.
3. URL에 포함된 값은 **`@PathVariable`** 로, 요청 본문에 담긴 데이터는 **`@RequestBody`** 로 받습니다.
4. 처리할 로직은 `Service` **객체의 메서드를 호출**하여 위임합니다.
5. 메서드의 **반환 값**은 Spring이 자동으로 클라이언트에게 보낼 응답 데이터(주로 JSON)로 만들어 줍니다.

### ✅ Controller와 RestController의 차이점은?

`@RestController`는 `@Controller`에 `@ResponseBody`가 추가된 것으로, API를 만들 때 사용합니다.

`@Controller`와 `@RestController`의 가장 큰 차이점은 **반환하는 값의 종류**에 있습니다.

- **@Controller :** 주로 **View(HTML 페이지)** 를 반환하기 위해 사용합니다.
    - 메서드가 문자열을 반환하면, Spring은 그 문자열을 View의 이름으로 해석하여 해당 화면을 렌더링합니다.
    - 만약 `@Controller`에서 JSON 같은 데이터를 반환하려면, 메서드에 `@ResponseBody` 어노테이션을 별도로 붙여줘야 합니다.
- **@RestController :** **데이터(주로 JSON, XML)** 를 반환하기 위해 사용합니다.
    - 이 어노테이션은 **`@Controller` + `@ResponseBody`** 를 합쳐 놓은 것 입니다.
    - 클래스에 `@RestController`를 붙이면, 그 안의 모든 메서드는 View가 아닌 데이터 자체를 반환하는 것이 기본 동작이 됩니다. RESTful API를 만들 때 사용됩니다.

---

| 구분 | `@Controller` | `@RestController` |
| -------- | -------- | -------- |
| 주요 목적 | MVC 패턴의 **View(화면)** 반환 | REST API의 **데이터(JSON 등)** 반환 |
| 메서드 반환 값 | View 이름 (String) | 객체, 데이터 (자동으로 JSON으로 변환) |
| `@ResponseBody` | 데이터 반환 시, 메서드에 **별도 추가 필요** | 클래스에 **자동으로 포함**되어 있어 불필요 |
| 사용 사례 | 서버 사이드 렌더링 (JSP, Thymeleaf) 웹 페이지 개발 | 모바일 앱, 프론트엔드(React, Vue)와 연동하는 API 개발 |

### ✅ 그 외 Presentation Layer의 컴포넌트

Controller 외에도 Presentation Layer에서는 요청을 처리하고 예외를 관리하기 위한 여러 컴포넌트를 사용합니다.

- **@ControllerAdvice / @RestControllerAdvice :** **전역 예외 처리**를 담당합니다. 여러 Controller에서 발생하는 특정 예외(Exception)들을 한 곳에서 공통으로 처리할 수 있게 해줍니다. 코드 중복을 줄이고 예외 관리를 중앙화하는 데 필수적입니다.
- **@ExceptionHandler :** `@Controller`나 `@ControllerAdvice` 클래스 내에서 특정 예외가 발생했을 때 실행될 메서드를 지정하는 어노테이션입니다. 예를 들어, `NullPointException`이 발생하면 특정 에러 페이지나 JSON 응답을 보내도록 처리할 수 있습니다.
- **Filter / Interceptor :** Controller에 요청이 도달하기 **전후**에 공통된 작업을 처리하기 위해 사용됩니다.
    - **Filter:** 사용자의 요청/응답 내용을 변경하거나, 인코딩 변환, 인코딩 변환, 보안 검사 등을 수행합니다. Spring 프레임워크 바깥인 Servlet 단계에서 동작합니다.
    - **Interceptor:** Filter와 유사하지만 Spring MVC의 일부입니다. 사용자 **인증(로그인) 여부를 확인**하거나, Controller로 넘어가는 데이터에 추가 정보를 더하는 등 비즈니스 로직에 더 가까운 작업을 처리할 때 사용됩니다.
