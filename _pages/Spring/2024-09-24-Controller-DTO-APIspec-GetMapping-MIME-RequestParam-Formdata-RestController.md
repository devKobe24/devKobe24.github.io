---
title: 🍃[Spring] Controller, DTO, API 명세, `@GetMapping`, MIME, `@RequestParam`, 폼 데이터, `@RestController`
tags:
    - Spring
    - Framework
date: "2024-09-24"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Controller, DTO, API 명세, `@GetMapping`, MIME, `@RequestParam`, 폼 데이터, `@RestController`

## 1️⃣ Controller.
- **Controller** 는 웹 애플리케이션의 요청을 처리하는 핵심 구성 요소로, 사용자 요청(URL 요청)을 받아 이를 처리한 후 적절한 응답(뷰 또는 데이터를 반환)을 제공하는 역할을 합니다.
- 이는 보통 **MVC(Model-View-Controller)** 패턴에서 **Controller** 에 해당하며, 클라이언트의 입력을 받아 비즈니스 로직과 상호작용하고 결과를 반환하는 흐름을 담당합니다.

---

- Controller는 주로 **`@Controller`** 또는 **`@RestController`** 애너테이션을 사용하여 정의 됩니다.

### 1. `@Controller` 와 `@RestController` 애너테이션의 차이점.
- **1. `@Controller`**
    - 일반적으로 **뷰(View)** 를 반환합니다.
    - Thymeleaf와 같은 템플릿 엔진을 사용할 때 HTML 파일을 반환할 때 사용됩니다.
    - 예: 사용자 요청을 처리하고 HTML 페이지를 렌더링하는 경우.

- **2. `@RestController`**
    - 주로 JSON, XML 등 데이터 포맷으로 응답을 반환합니다.
    - **`@Controller`** 와 **`@ResponseBody`** 를 함께 사용하는 것과 동일한 역할을 합니다.
    - RESTful API를 구현할 때 주로 사용됩니다.

### 2. 기본 예시.

- **1. `@Controller` 를 이용한 HTML 페이지 반환**

```java
@Controller
public class HomeController {
    
    @GetMapping("/home")
    public String homePage(Model model) {
        model.attribute("message", "Welcome to the home page!");
        return "home"; // resource/templates/home.html로 연결됨(Thymeleaf와 같은 템플릿 엔진 사용)
    }
}
```

- **2. `@RestController` 를 이용한 JSON 응답 반환**

```java
@RestController
public class ApiController {
    
    @GetMapping("/api/data")
    public ResponseEntity<String> getData() {
        return new ResponseEntity<>("Hello, this is JSON data!", HttpStatus.OK);
    }
}
```

### 3. Controller의 주요 역할.
- **요청 매핑**
    - URL을 특정 메서드에 매핑하여 적절한 처리를 하도록 합니다.(`@GetMapping`, `@PostMapping`, `@RequestMapping` 등 사용)
- **비즈니스 로직 호출**
    - 서비스 계층과 상호작용하여 필요한 작업을 수행합니다.
- **모델과 뷰 처리**
    - 데이터를 처리한 후 뷰로 데이터를 전달하거나 JSON 등의 형식으로 응답합니다.

Spring Boot의 Controller는 이러한 방식으로 애플리케이션의 핵심 흐름을 제어하며, 웹 요청을 처리하는 중요한 역할을 담당합니다.

## 2️⃣ DTO(Data Transfer Object)
- **DTO(Data Transfer Object)** 는 계층 간의 데이터를 전송할 때 사용하는 객체입니다.
- 일반적으로, 백엔드 애플리케이션에서는 여러 계층(Controller, Service, Repository 등) 간에 데이터를 주고받게 되는데, 이때 사용자가 전달한 데이터를 담거나 가공된 데이터를 전달하기 위해 DTO를 사용합니다.

### 1. DTO의 주요 목적.
- **1. 데이터 캡슐화**
    - DTO는 특정 데이터 구조를 캡슐화하여 외부에 노출됩니다.
    - 이로 인해 불필요한 데이터 노출을 방지할 수 있습니다.
- **2. 성능 최적화**
    - 대규모 데이터를 한 번에 전송하는 것보다는, 필요한 데이터만 DTO를 통해 전송하는 것이 네트워크 트래픽 및 성능을 최적화하는 데 도움이 됩니다.
- **3. 계층 분리**
    - DTO는 주로 서비스 계층에서 데이터를 조작하고, 이를 컨트롤러 계층으로 전달하는 데 사용됩니다.
    - 이를 통해 애플리케이션의 각 계층이 명확하게 분리되며, 유지보수성과 확장성이 형성됩니다.

### 2. DTO와 엔티티의 차이점.
- **엔티티(Entity)**
    - 엔티티는 데이터베이스 테이블과 1:1로 매핑되며, 주로 영속성(Persistence)을 담당합니다.
    - 데이터베이스와의 상호작용을 관리하는 역할을 하며, 비즈니스 로직을 포함하지 않는 것이 일반적입니다.
- **DTO**
    - DTO는 데이터를 전달하는 데 중점을 두며, 보통 엔티티를 변환하거나 특정 로직을 처리하기 위해 사용됩니다.
    - 엔티티와는 다르게 데이터베이스와 직접적으로 매핑되지 않으며, 전송하고자 하는 데이터만을 포함합니다.

### 3. 사용 예시.
- 다음은 간단한 User 엔티티와 UserDTO의 예시입니다.

- **User 엔티티**

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    private String email;
    
    // getters and setters
}
```

- **UserDTO**

```java
public class UserDTO {
    private String name;
    private String email;
    
    // 생성자, getters, setters
}
```

이처럼 엔티티는 데이터베이스와 연결된 객체이고, DTO는 사용자에게 데이터를 전달하거나 요청을 받아올 때 사용되는 객체입니다.

## 3️⃣ API 명세(API Specification)
- **API 명세(API Specification)** 는 API(Application Programming Interface)의 동작 방식, 요청 및 응답 형식, 사용 가능한 메서드, 파라미터 등을 명확히 설명한 문서입니다.
- API 명세는 API 사용자(주로 개발자)들이 API를 정확히 이해하고 사용할 수 있도록 도와주는 중요한 문서입니다.

### 1. API 명세의 주요 내용.
- **1. API 개요.**
    - API가 어떤 목적을 가지고 있는지, 주로 어떤 기능을 수행하는지 간단한 설명을 포함합니다.
- **2. 엔드포인트(Endpoint)**
    - API의 접근 경로를 의미하며, 각 엔드포인트는 특정 기능을 수행합니다.
    - 예를 들어 `GET /users`는 사용자의 목록을 가져오는 API 엔드포인트일 수 있습니다.
- **3. HTTP 메서드**
    - 각 엔드포인트에 사용할 수 있는 HTTP 메서드(GET, POST, PUT, DELETE 등)가 명시됩니다.
    - 예를 들어, `GET /users`는 사용자 목록을 조회하고, `POST /users`는 새 사용자를 생성하는 API를 의미할 수 있습니다.
- **4. 요청(Request) 파라미터**
    - API 요청 시 포함할 수 있는 파라미터나 데이터 형식을 명시합니다.
    - 파라미터는 경로 변수(Path Variables), 쿼리 파라미터(Query Parameters), 헤더(Headers), 또는 바디(Body)에 포함될 수 있습니다.
        - **경로 변수 :** `GET /users/{id}`에서 `{id}`가 경로 변수 입니다.
        - **쿼리 파라미터 :** `GET /users?status=active`에서 `status`가 쿼리 파라미터입니다.
- **5. 요청 본문(Request Body)**
    - 주로 POST나 PUT 같은 요청에서 사용되며, JSON, XML 또는 폼 데이터 등으로 전송할 데이터를 정의합니다.
    - 예를 들어, 사용자를 생성하는 API에서는 다음과 같은 JSON 요청이 본문이 있을 수 있습니다.

```json
{
    "name": "Jhon Doe",
    "email": "jhon@example.com"
}
```

- **6. 응답(Response)**
    - API 요청에 대한 응답 형식을 설명합니다.
    - 성공적인 요청이나 실패했을 때의 응답 코드(예: 200 OK, 404 Not Found)와 함께 응답 본문(Response Body) 형식도 명시됩니다.

```json
{
    "id": 1,
    "name": "Jhon Doe",
    "email" "jhon@example.com"
}
```

- **7. 상태 코드(Status Code)**
    - 각 응답의 결과를 나타내는 HTTP 상태 코드를 포함합니다.
    - 일반적인 상태 코드는 다음과 같습니다.
        - **200 OK :** 성공적인 요청.
        - **201 Created :** 리소스 생성 성공.
        - **400 Bad Request :** 잘못된 요청.
        - **401 Unauthorized :** 인증 실패.
        - **404 Not Found :** 리소스를 찾을 수 없음
        - **500 Internet Server Error :** 서버에서 발생한 오류

- **8. 예시(Examples)**
    - 실제로 요청을 보내는 방법과 그에 대한 응답을 보여주는 예시가 포함될 수 있습니다.
    - 예를 들어, `curl` 명령어를 사용한 HTTP 요청 방법 등을 명시합니다.

```bash
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name": "John Doe", "email": "john.doe@example.com"}'
```

### 2. API 명세의 중요성.
- **개발자 간의 의사소통**
    - API 명세는 API를 제공하는 측과 사용하는 측 간의 원활한 의사소통을 가능하게 합니다.
    - 명세가 정확할수록 API를 사용할 때 발생할 수 있는 혼란이나 오류가 줄어듭니다.

- **일관성**
    - 모든 엔드포인트와 요청/응답 형식이 일관되게 설계되고 구현될 수 있도록 도와줍니다.

- **유지보수**
    - 명세를 기준으로 API를 변경하거나 확장할 때, 이를 참고하여 기존 사용자들에게 영향을 최소화하면서 시스템을 개선할 수 있습니다.

### 3. API 명세 도구.

API 명세는 다양한 형식으로 작성될 수 있으며, 대표적인 도구와 표준은 다음과 같습니다.

- **OpenAPI(Swagger)**
    - 가장 널리 사용되는 API 명세 표준으로, Swagger UI를 통해 시각화와 테스트 기능을 제공합니다.
- **Postman**
    - API 테스트 및 명세 작성을 위한 도구로, 다양한 HTTP 메서드를 테스트하고 명세를 자동으로 생성할 수 있습니다.
- **RAML**
    - RESTful API Modeling LAnguage의 약자로, REST API 명세 작성을 위해 사용됩니다.

API 명세는 API를 사용하려는 모든 개발자에게 필수적인 가이드 역할을 하며, 정확하고 명확하게 작성하는 것이 중요합니다.

## 4️⃣ `@GetMapping` 애너테이션.
- **`@GetMapping` 애너테이션** 은 Spring Framework에서 HTTP GET 요청을 처리하기 위해 사용하는 애너테이션입니다.
- 주로 Spring MVC에서 컨트롤러의 메서드에 붙여서 특정 URL로 들어오는 GET 요청을 처리할 수 있도록 매핑합니다.

### 1. `@GetMapping`의 기본 동작.
- Spring Boot 애플리케이션에서는 클라이언트가 서버로 GET 요청을 보낼 때 해당 요청을 처리할 컨트롤러 메서드를 지정하기 위해 `@GetMapping`을 사용합니다.
- 이 애너테이션은 내부적으로 `@RequestMapping(method = RequestMethod.GET)`과 동일한 역할을 합니다.

### 2. 사용 예시.

다음은 `@GetMapping`을 사용하여 특정 URL에서 GET 요청을 처리하는 간단한 예시입니다.

```java
@RestController
public class UserController {
    
    // '/users' 경로에 대한 GET 요청 처리
    @GetMapping("/users")
    public List<User> getAllUsers() {
        // 서비스 계층에서 사용자 목록을 가져와 반환하는 메서드
        return userService.getAllUsers();
    }
}
```

위의 코드에서
- `@GetMapping("/users")`는 `/users` 경로로 들어오는 GET 요청을 처리합니다.
- 메서드 `getAllUsers()`는 GET 요청을 처리하며, 보통 클라이언트에게 데이터를 반환합니다.
    - 이때 반환하는 데이터는 기본적으로 JSON 형식으로 반환됩니다(Spring Boot에서는 `@RestController` 사용 시).

### 3. 주요 특징.
- **1. HTTP GET 요청 처리.**
    - `@GetMapping`은 GET 요청을 처리하는 데만 사용되며, 다른 HTTP 메서드(POST, PUT, DELETE 등) 요청은 처리하지 않습니다.

- **2. 경로 지정.**
    - `@GetMapping("/path")` 형식으로 매핑 경로를 지정할 수 있습니다.
    - 경로에 고정된 URL뿐만 아니라 경로 변수, 쿼리 파라미터 등도 포함될 수 있습니다.

- **3. 경로 변수 사용.**
    - 경로 변수(Path Variable)를 사용하여 동적으로 URL을 처리할 수도 있습니다.

```java
@GetMapping("/users/{id}")
public User getUserById(@PathVariable Long id) {
    return userService.getUserById(id);
}
```

- 위 코드에서 `@PathVariable`을 사용하여 URL 경로에 포함된 `id` 값을 메서드 파라미터로 전달받습니다.
    - 예를 들어, `/users/1`이 요청되면 `id`가 1로 설정됩니다.

- **4. 쿼리 파라미터 사용.**
    - 쿼리 파라미터(Query Parameter)를 처리할 때도 사용할 수 있습니다.

```java
@GetMapping("/users")
public List<User> getUsersByStatus(@RequestParam String status) {
    return userService.getUsersByStatus(status);
}
```

- 위 예시에서 `@RequestParam`을 사용하여 쿼리 파라미터 `status`를 메서드 파라미터로 전달받습니다.
    - 예를 들어, `/users?status=active`로 요청하면 `status` 값은 "active"가 됩니다.

### 4. `@GetMapping` 과 관련된 주요 옵션.
- **`produces`**
    - 응답으로 제공하는 데이터의 MIME 타입을 지정할 수 있습니다.
        - 예를 들어, JSON, XML 형식의 응답을 지정할 수 있습니다.

```java
@GetMapping(value = "/users", produces = "application/json")
public List<User> getAllUsers() {
    return userService.getAllUsers();
}
```

- **`params`**
    - 특정 쿼리 파라미터가 존재하는 경우에만 해당 메서드가 매핑되도록 제한할 수 있습니다.

```java
@GetMapping(value = "/users", params = "active")
public List<User> getActiveUsers() {
    return userService.getActiveUsers();
}
```

- 위 예시에서 `active`라는 쿼리 파라미터가 있어야만 이 메서드가 호출됩니다.

### 5. `@GetMapping` VS `@RequestMapping`
- `@RequestMapping` 을 사용하면 HTTP 메서드를 지정해야 합니다.

```java
// @RequestMapping
@RequestMapping(value = "/users", method = RequestMethod.GET)
public List<User> getAllUsers() {
    return userService.getAllUsers();
}

// @GetMapping
@GetMapping("/users")
public List<User> getAllUsers() {
    return userService.getAllUsers();
}
```

- `@GetMapping`은 GET 요청 전용으로 보다 간결하게 작성할 수 있는 방법입니다.

**리팩토링 포인트**
- **1.** `@RequestMapping` 은 다양한 HTTP 메서드(GET, POST, PUT, DELETE 등)를 처리할 수 있도록 설계된 범용 애너테이션입니다. 그러나 `GET` 요청만을 처리할 때는 `@GetMapping`을 사용하여 코드를 간결하게 만들 수 있습니다.

- **2.** `method = RequestMethod.GET` 대신 `@GetMapping`을 사용함으로써 더 직관적이고 간단하게 GET 요청을 처리할 수 있습니다.

- **3.** `@GetMapping`은 기본적으로 `GET` 요청을 처리하므로 `value` 속성에 경로만 명시하면 됩니다.

### 6. 요약.
- **주요 역할**
    - HTTP GET 요청을 특정 메서드에 매핑하여 요청 처리.
- **간결한 문법**
    - `@RequestMapping`의 GET 요청 처리를 간결하게 대체.
- **주로 사용되는 곳**
    - 리소스 조회, 데이터 가져오기 등의 역할을 수행할 때 사용.

## 5️⃣ MIME(Mutipurpose Internet Mail Extensions) 타입.
- **MIME(Mutipurpose Internet Mail Extensions) 타입** 은 인터넷에서 전송되는 데이터의 형식을 나타내는 표준입니다.
- MIME 타입은 클라이언트와 서버 간에 주고받는 데이터가 어떤 형식인지 정의하여, 이를 통해 클라이언트(브라우저 등)는 데이터의 처리를 어떻게 해야 헐지 알 수 있습니다.

### 1. MIME 타입의 구성.

MIME 타입은 크게 두 부분으로 나뉩니다.
- **타입(Type)**
    - 데이터의 일반적인 범주를 나타냅니다.(예: `text`, `image`, `application` 등).

- **서브타입(Subtype)**
    - 데이터의 구체적인 형식을 나타냅니다.(예: `html`, `plain`, `json`, `jpeg` 등).

형식은 `/`(슬래시)로 구분되며, 예를 들어 `text/html`은 HTML 문서라는 의미입니다.

### 2. MIME 타입의 예시.
- **1. 텍스트 형식.**
    - `text/plain`
        - 일반 텍스트(ASCII 또는 UTF-8로 인코딩된 텍스트).
    - `text/html`
        - HTML 문서.
    - `text/css`
        - CSS 파일.
    - `text/javascript`
        - JavaScript 파일.

- **2. 이미지 형식.**
    - `image/jpeg`
        - JPEG 이미지.
    - `image/png`
        - PNG 이미지.
    - `image/gif`
        - GIF 이미지.

- **3. 응용 프로그램 형식.**
    - `application/json`
        - JSON 형식의 데이터.
    - `application/xml`
        - XML 문서.
    - `appliccation/pdf`
        - PDF 파일.
    - `application/octet-stream`
        - 일반적인 바이너리 데이터.
        - 파일 다운로드 시 주로 사용됩니다.

- **4. 멀티미디어 형식.**
    - `audio/mpeg`
        - MP3 오디오 파일.
    - `video/mp4`
        - MP4 비디오 파일.

- **5. 기타 형식.**
    - `multipart/form-data`
        - 주로 파일 업로드나 폼 데이터를 전송할 때 사용되는 형식.

### 3. MIME 타입의 역할.

MIME 타입은 웹 브라우저와 같은 클라이언트가 서버로부터 받은 데이터의 형식을 이해하고 적절하게 처리하는 데 중요한 역할을 합니다.

예를 들어

- `text/html`
    - 브라우저는 이 MIME 타입을 받으면 이를 HTML 문서로 해석하고 화면에 렌더링합니다.
- `application/json`
    - 브라우저는 이 MIME 타입을 받으면 이를 JSON 형식의 데이터로 인식하고, 보통 개발자 도구에서 구조화된 형식으로 보여줍니다.
- `application/octet=stream`
    - 바이너리 파일(예: 프로그램 파일, 압축 파일)을 다운로드할 때 사용되며, 클라이언트는 이 데이터를 파일로 저장할 수 있습니다.

### 4. MIME 타입 지정.

- 서버가 클라이언트에 데이터를 보낼 때 MIME 타입을 지정하는데, HTTP 응답 헤더의 `Content-Type` 필드를 통해 MIME 타입이 정의됩니다.

**예시: `Content-Type` 헤더**

```http
HTTP/1.1 200OK
Content=Type: application/json

{
    "name": "John",
    "age": 30
}
```

- 위 예시에서는 서버가 JSON 형식의 데이터를 응답하며, `Content-Type: application/json` 을 통해 MIME 타입을 명시하고 있습니다.

### 5. Spring에서의 MIME 타입 지정.
- Spring에서는 `@GetMapping` 또는 `@PostMaping` 과 같은 애너테이션을 사용할 때 `produces` 속성을 통해 MIME 타입을 지정할 수 있습니다.

**예시 : JSON 형식의 응답 지정.**

```java
@GetMapping(value = "/users", produces = "application/json")
public List<User> getAllUsers() {
    return userService.getAllUsers();
}
```

- 위 코드에서는 `/users`로 들어오는 GET 요청에 대해 JSON 형식 (`application/json`)으로 데이터를 응답하도록 지정하고 있습니다.

### 6. 요약.
- **MIME 타입** 은 인터넷에서 전송되는 데이터의 형식을 나타내는 표준.
- 주로 클라이언트와 서버 간에 데이터를 주고받을 때 데이터 형식을 지정하여 클라이언트가 이를 적절히 처리할 수 있도록 도와줌
- **형식** 은 `타입/서브타입` 으로 구성되며, 예를 들어 `text/html` 은 HTML 문서를 나타냄.
- Spring과 같은 프레임워크에서도 응답 형식에 따라 적절한 MIME 타입을 지정할 수 있음.

## 6️⃣ `@RequestParam` 애너테이션.
- **`@RequestParam` 애너테이션** 은 Spring MVC에서 HTTP 요청의 쿼리 파라미터, 폼 데이터 또는 URL에 포함된 파라미터를 메서드 파라미터에 바인딩하는 데 사용됩니다.
- 주로 GET 요청에서 뭐리 스트링의 파라미터 값을 가져오거나, POST 요청에서 폼 데이터로 전달된 파라미터 값을 처리하는 데 사용됩니다.

### 1. 기본 사용법.
- **`@RequestParam`** 을 사용하면 클라이언트가 요청한 URL의 파라미터 값을 메서드의 인자로 받을 수 있습니다.
    - 예를 들어, 사용자가 `GET /users?name=Jhon` 과 같이 요청하면 `name` 파라미터 값을 메서드에서 받을 수 있습니다.

#### 예시 1: 단일 쿼리 파라미터 받기.
```java
@GetMapping("/users")
public String getUserByName(@RequestParam String name) {
    return "Requested user: " + name;
}
```

위 코드에서
- `@RequestParam String name`
    - 쿼리 파라미터 `name`의 값을 받아서 메서드 파라미터로 전달합니다.
        - 예를 들어 `/users?name=Jhon`으로 요청하면 `name`에 `"John"` 값이 들어갑니다.

#### 예시 2: 여러 쿼리 파라미터 받기.
```java
@GetMapping("/users")
public String getUser(@RequestParam String name, @RequestParam int age) {
    return "User: " + name + ", Age: " + age;
}
```

- 이 코드는 두 갸의 쿼리 파라미터(`name`과 `age`)를 받습니다.
- 클라이언트가 `/users?name=Jhon&age=25`로 요청하면 `name`은 `"Jhon"`, `age`는 `25`가 됩니다.

### 3. 선택적 파라미터와 기본값.
- `@RequestParam`의 속성을 이용하면 선택적인 파라미터를 정의할 수 있습니다.
- 파라미터가 없을 경우 기본값을 설정할 수 있습니다.

#### 예시 3: 기본값 설정.
```java
@GetMapping("/users")
pulbic String getUser(@RequestParam(defaultValue = "Unknown") String name) {
    return "Requested user: " + name;
}
```

- 위 코드에서 클라이언트가 `/users`로만 요청을 보내면, `name`의 기본값으로 `"Unknown"`이 설정됩니다.
- `/users?name=John`으로 요청하면 `name`은 `"John"` 값이 됩니다.

#### 예시 4: 필수 여부 설정.
- `required` 속성을 사용하면 파라미터가 필수인지 선택적인지 설정할 수 있습니다.
    - 기본적으로 `@RequestParam`은 필수입니다.
    - 하지만, `required = false`로 설정하면 파라미터가 없어도 예외를 발생시키지 않습니다.
```java
@GetMapping("/users")
public String getUser(@RequestParam(required = false) String name) {
    return "Requested user: " + (name != null ? name : "Guest");
}
```

- 이 경우, 클라이언트가 `name` 파라미터를 포함하지 않고 요청할 경우 `name` 값은 `null`이 되고, `Guest`로 대체 될 수 있습니다.

### 4. 배열 또는 리스트 파라미터 처리.
- `@RequestParam`을 사용하여 여러 값을 한 번에 받을 수도 있습니다.

#### 예시 5: 배열로 파라미터 받기.
```java
@GetMapping("/users")
public String getUsersByNames(@RequestParam List<String> names) {
    return "Requested users: " + String.join(", ", names);
}
```

- 위 코드에서 `/users?names=John&names=Jane`과 같이 요청하면 `names`는 `["John", "Jane"]` 리스트로 전달됩니다.

### 5. 요약.
- `@RequestParam`은 URL 쿼리 파라미터나 폼 데이터를 컨트롤러 메서드의 파라미터로 매핑하는 데 사용됩니다.
- 필수 여부(`required`), 기본값(`defaultValue`) 설정을 통해 유연하게 파라미터를 처리할 수 있습니다.
- 여러 파라미터나 배열/리스트 형태의 파라미터도 받을 수 있습니다.

## 7️⃣ 폼 데이터(Form Data).
- **폼 데이터(Form Data)** 는 웹 애플리케이션에서 사용자가 웹 브라우저를 통해 서버로 제출하는 데이터를 말합니다.
- 주로 HTML `<form>` 요소를 사용하여 사용자 입력을 서버로 전송하며, 사용자가 입력한 텍스트, 파일, 선택한 옵션 등의 다양한 데이터를 포함할 수 있습니다.

### 1. 폼 데이터의 구성.
- 폼 데이터는 여러 종류의 입력 필드를 통해 생성되며, 전송 방식에 따라 다르게 처리됩니다.
- HTML `<form>` 태그를 이용하여 입력 폼을 만들고, 사용자가 입력한 데이터를 서버로 전송할 수 있습니다.

#### HTML 폼의 예시.
```html
<form action="/submit" method="post">
    <label for="name">Name:</label>
    <input type="text" id="name" name="name">
    
    <label for="age">Age:</label>
    <input type="number" id="age" name="age">
    
    <label for="file">Profile Picture:</label>
    <input type="file" id="file" name="profilePicture">
    
    <button type="submit">Submit</button>
</form>
```

- 이 폼에서는 다음과 같은 데이터를 서버로 전송할 수 있습니다.
    - **Name :** 텍스트 입력.
    - **Age :** 숫자 입력.
    - **Profile Picture :** 파일 업로드.

### 2. 폼 데이터의 전송 방식.

#### 1. GET 메서드
- 폼이 `GET` 메서드를 사용하여 데이터를 전송할 때는 데이터가 URL의 쿼리 스트링(Query String)으로 전달 됩니다.
- URL은 보통 다음과 같은 형식을 가집니다.
    - `example.com/submit?name=Jhon&age=30`
- 데이터는 URL에 포함되기 떄문에 보안에 취약하고, 전송 가능한 데이터의 양이 제한적입니다.
    - 따라서 검색 쿼리나 필터와 같은 간단한 데이터를 전송할 때 주로 사용됩니다.

```html
<form action="/submit" method="get">
    <!-- 입력 필드 -->
    <button type="submit">Submit</button>
</form>
```

#### 2. POST 메서드
- `POST` 메서드를 사용하면 데이터가 HTTP 요청 본문(Body)에 포함되어 전송됩니다.
    - 이 방식은 URL에 데이터를 노출하지 않기 때문에 더 안전하며, 대용량 데이터를 전송할 수 있습니다.
- 파일 업로드나 민감한 정보를 전송할 때 주로 사용됩니다.

```html
<form action="/submit" method="post">
    <!-- 입력 필드 -->
    <button type="submit">Submit</button>
</form>
```

### 3. 전송되는 데이터 형식.

폼 데이터가 전송될 때, 브라우저는 **`Content-Type`** 헤더를 통해 서버에 어떤 방식으로 데이터를 인코딩했는지 알려줍니다.

폼 데이터의 인코딩 방식에 따라 서버에서 데이터를 처리하는 방식이 달라집니다.

- **1. `application/x-www.form-urlencoded`**
    - 기본 폼 인코딩 방식입니다.
    - 폼 데이터를 URL 인코딩하여 키-값 쌍으로 전송합니다.
    - 예를 들어, `name=John&age=30`처럼 키와 값을 `&`로 구분하여 서버로 전송합니다.
    - `POST` 요청의 본문에 이 형식으로 데이터가 포함됩니다.
    - 예시
    ```plaintext
    name=Jhon&age=30
    ```

- **2. `multipart/form-data`**
    - 파일 업로드가 포함된 폼 데이터를 전송할 때 사용되는 인코딩 방식입니다.
    - 데이터와 파일을 여러 부분으로 나누어 서버로 전송합니다.
    - 파일뿐만 아니라 텍스트 필드도 함께 전송할 수 있습니다.
    - 예시: 다음과 같은 방식으로 파일과 데이터를 함께 전송합니다.
    ```plaintext
    --boundary
    Content-Disposition: form-data; name="name"
    
    John
    --boundary
    Content-Disposition: form-data; name="profilePicture"; filename="profile.jpg"
    Content-Type: image/jpeg
    
    [바이너리 파일 데이터]
    --boundary--
    ```

- **3. `text/plain`**
    - 데이터를 단순한 텍스트 형식으로 전송합니다.
    - 주로 API와의 간단한 텍스트 전송에 사용되며, 폼 데이터로는 잘 사용되지 않습니다.

### 4. 서버에서 폼 데이터 처리.
- 서버는 클라이언트가 전송한 폼 데이터를 처리합니다.
    - 예를 들어, Spring Boot에서는 `@RequestParam` 또는 `@ModelAttribute` 등을 사용하여 폼 데이터를 쉽게 처리할 수 있습니다.

#### 예시: Spring에서 폼 데이터 받기.
```java
@PostMapping("/submit")
public String handleForm(@RequestParam String name, @RequestParam int age) {
    // 폼 데이터 처리
    return "Name: " + name + ", Age: " + age;
}
```

- 서버는 클라이언트가 보낸 폼 데이터에서 `name`과 `age` 값을 추출하여 처리합니다.

### 5. 요약.
- 폼 데이터는 사용자가 웹 브라우저의 폼을 통해 서버로 전송하는 데이터를 의미합니다.
- 폼 데이터는 GET 또는 POST 메서드를 통해 전송되며, URL 인코딩 방식이나 `multipart/form-data` 방식으로 서버에 전달됩니다.
- 서버는 해당 데이터를 받아 처리하여 사용자 요청에 맞는 결과를 반환합니다.

## 8️⃣ `@RestController` 애너테이션.
- **`@RestController` 애너테이션** 은 Spring Framework에서 RESTful 웹 서비스의 컨트롤러를 정의할 때 사용하는 애너테이션입니다.
- 이 애너테이션은 컨트롤러 클래스에 붙이며, 이 클래스가 JSON이나 XML과 같은 데이터를 HTTP 응답 본문으로 직접 반환하도록 해줍니다.

### 1. `@RestController`의 특징.

#### 1. `@Controller` + `@ResponseBody`의 결합.
- `@RestController`는 내부적으로 `@Controller`와 `@ResponseBody` 애너테이션을 결합한 것입니다.
- `@Controller`는 Spring MVC에서 View를 반환하는 전통적인 컨트롤러를 정의하는 데 사용되지만, `@RestController`는 View를 반환하지 않고, 객체나 데이터를 직접 HTTP 응답 본문으로 반환합니다.
- `@ResponseBody`는 메서드가 반환하는 객체를 JSON이나 XML로 직렬화하여 HTTP 응답의 본문으로 반환하도록 하는 역할을 합니다.
- `@RestController`는 클래스 레벨에서 모든 메서드에 `@ResponseBody`가 자동으로 적용됩니다.

#### 2. RESTful 웹 서비스에 적합.
- `@RestController`는 주로 RESTful 웹 서비스 개발에 사용되며, 클라이언트가 서버로부터 JSON, XML 등의 데이터를 받을 수 있도록 설계되었습니다.
- 전통적인 MVC 패턴에서는 데이터를 뷰 페이지(HTML 등)로 전달하지만, RESTful 웹 서비스에서는 데이터 자체(JSON, XML 등)를 응답으로 전달합니다.

### 2. 예시 코드.
```java
@RestController
public class UserController {
    
    @GetMapping("/users")
    public List<User> getAllUsers() {
        // 사용자 목록을 반환하는 예시
        return userService.getAllUsers();
    }
    
    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        // 새로운 사용자를 생성하는 예시
        return userService.saveUser(user);
    }
}
```

- `@RestController`
    - 이 클래스는 RESTful 컨트롤러이며, 모든 메서드는 JSON 또는 XML과 같은 데이터를 HTTP 응답 본문으로 반환합니다.
    - `@GetMapping("/users")`
        - GET 요청을 처리하며, `List<User>` 객체를 JSON 형식으로 반환합니다.
    - `@PostMapping("/users")`
        - POST 요청을 처리하며, 클라이언트가 보낸 `User` 데이터를 받아 새로운 사용자를 생성하고 그 결과를 JSON 형식으로 반환합니다.

### 3. `@RestController`와 일반 컨트롤러(`@Controller`)의 차이.
- **1. 주된 목적.**
    - `@RestController`는 데이터(주로 JSON, XML)를 직접 반환하는 데 사용되며, API 서버를 구축할 때 주로 사용됩니다.
    - `@Controller`는 뷰(HTML, JSP 페이지 등)를 반환하는 데 주로 사용되며, 웹 애플리케이션의 화면을 보여줄 때 적합합니다.

- **2. View 해석.**
    - `@RestController`는 데이터를 응답 본문에 바로 전달하며, JSP나 Thymeleaf 같은 뷰 템플릿 엔진을 사용하지 않습니다.
    - `@Controller`는 뷰 이름을 반환하고, Spring MVC는 이 뷰 이름을 해석하여 JSP나 Thymeleaf 같은 템플릿 엔진을 사용해 화면을 렌더링합니다.

- **3. `@ResponseBody` 필요 여부.**
    - `@RestController`를 사용하면 메서드마다 `@ResponseBody`를 붙일 필요가 없습니다.
        - 클래스 레벨에서 자동으로 적용됩니다.
    - `@Controller`를 사용할 경우, 데이터를 반환하려면 메서드마다 `@ResponseBody`를 붙여서 응답 본문에 데이터를 보내도록 명시해야 합니다.
        - **일반 컨트롤러(`@Controller`) 예시**
        ```java
        @Controller
        public class ViewController {
            
            @GetMapping("/home")
            public String home() {
                // "home.html"이라는 뷰를 반환함
                return "home";
            }
        }
        ```
        - 이 코드는 "home"이라는 뷰 이름을 반환하며, Spring은 `home.html` 또는 `home.jsp`를 찾아서 렌더링하게 됩니다.

### 4. 요약.

- `@RestController`는 RESTful 웹 서비스 개발에 사용되며, 데이터를 JSON이나 XML 등의 형식으로 반환합니다.
- `@RestController`는 `@Controller`와 `@ResponseBody`의 결합으로, 데이터를 응답 본문에 바로 반환하도록 설계되었습니다.
- Spring MVC에서 웹 애플리케이션의 뷰를 렌더링하는 `@Controller` 와 달리, `@RestController`는 API 개발에 더 적합합니다.
