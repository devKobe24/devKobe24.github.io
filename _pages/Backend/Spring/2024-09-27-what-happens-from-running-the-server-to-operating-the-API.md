---
title: 🍃[Spring] 서버를 실행시켜 API를 동작시키기까지 일어나는 일
tags:
    - Spring
    - Framework
date: "2024-09-27"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 서버를 실행시켜 API를 동작시키기까지 일어나는 일

Java 백엔드 애플리케이션에서 서버를 실행시켜 **API를 동작시키기까지는** 여러 단계의 과정이 순차적으로 진행됩니다.
이 과정은 주로 Spring Boot와 같은 프레임워크를 사용한 애플리케이션을 기준으로 설명됩니다.
전체 흐름은 **서버 시작**에서 **API 요청 처리**까지의 과정으로 나눌 수 있습니다.

> 🙋‍♂️ [프레임워크와 라이브러리의 차이점](https://www.devkobe24.com/CS/2024/2024-09-26-Library-and-Framework.html)

## 1️⃣ 서버 시작.
- Java 애플리케이션에서 서버를 시작하려면 먼저 **서버 애플리케이션**이 실행되어야 합니다.
- Spring Boot에서는 `SpringApplication.run()` 메서드를 호출하여 서버 애플리케이션을 시작합니다.

### Spring Boot 애플리케이션 시작 예시
```java
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

- `@SpringBootApplication` 애너테이션이 붙은 클래스는 Spring Boot 애플리케이션의 진입점(Entry Point) 역할을 하며, `SpringApplication.run()` 메서드가 실행되면 내부적으로 **톰켓(Tomcat)** 과 같은 **임베디드 웹 서버**가 시작됩니다.

## 2️⃣ 임베디드 웹 서버 실행.
- **임베디드 톰켓 서버**는 Spring Boot에 포함된 기본 웹 서버로, 개발자가 별도로 서버를 설정하지 않아도 됩니다.
- 서버가 실행되면 지정된 포트(기본적으로 8080)에서 **클라이언트 HTTP 요청**을 수신할 준비를 합니다.
    - 이 과정에서 다음과 같은 일이 일어납니다.
        - 서버가 포트를 열고, 클라이언트로부터 들어오는 HTTP 요청을 수신할 수 있게 준비합니다.
        - Spring Boot 애플리케이션 내에 정의된 컨트롤러와 매핑된 URL을 등록하여 어떤 요청이 들어올 때 어떤 메서드가 처리해야 하는지 설정합니다.

## 3️⃣ 의존성 주입 및 컴포넌트 스캔.
- Spring Boot는 애플리케이션을 시작하면서 **의존성 주입(Dependency Injection)** 과 **컴포넌트 스캔(Component Scan)** 을 통해 애플리케이션 내에서 정의된 **Bean(객체)** 들을 찾아 **애플리케이션 컨텍스트(Application Context)** 에 등록합니다.
    - **의존성 주입**
        - 객체 간의 의존성을 자동으로 주입하여 애플리케이션 내에서 사용하는 객체 간의 상호작용을 설정합니다.
        - 예를 들어, 서비스(Service)가 컨트롤러(Controller)에 자동으로 주입됩니다.
    - **컴포넌트 스캔**
        - `@Controller`, `@Service`, `@Repository` 같은 애너테이션을 통해 정의된 Bean을 찾아서 애플리케이션 컨텍스트에 등록합니다.

## 4️⃣ URL 매핑 및 라우팅 설정.
- Spring Boot는 `@RestController`와 같은 애너테이션을 사용하여 **API 앤드포인트와 HTTP 메서드(GET, POST, PUT, DELETE)** 를 매핑합니다.
- 서버가 시작되면, URL 요청이 들어올 때 어떤 메서드가 호출될지 라우팅 정보가 설정됩니다.

### 예시: 컨트롤러 설정.
```java
@RestController
@RequestMapping("/api")
public class UserController {
    
    @GetMapping("/users")
    public List<User> getAllUsers() {
        // 비즈니스 로직을 호출하여 사용자 목록을 반환
        return userService.getAllUsers();
    }
}
```

- `/api/users`로 들어오는 GET 요청은 `getAllUsers()` 메서드로 라우팅됩니다.
- Spring은 `DispatcherServlet`이라는 중앙 제어 역할을 하는 서블릿을 통해 클라이언트로부터 들어오는 HTTP 요청을 적절한 컨트롤러로 전달하고 처리합니다.

## 5️⃣ 클라이언트 요청 처리.
- 서버가 실행된 후, **클라이언트(예: 웹 브라우저, Postman)** 가 HTTP 요청을 보냅니다.
- 클라이언트가 API 엔드포인트(예: `/api/users`)로 GET 요청을 보냈을 때, 다음과 같은 일이 발생합니다.
    - **1. HTTP 요청 수신**
        - 톰켓 서버는 지정된 포트(예: 8080)에서 HTTP 요청을 수신합니다.
    - **2. DispatcherServlet으로 전달**
        - 요청은 Spring의 **DispatcherServlet**으로 전달됩니다.
        - DispatcherServlet은 중앙 제어 역할을 하며, 요청을 적절한 컨트롤러로 라우팅하는 역할을 합니다.
    - **3. 컨트롤러 메서드 실행**
        - 요청된 URL과 HTTP 메서드(GET, POST 등)에 맞는 컨트롤러 메서드가 호출됩니다.
        - 예를 들어, `/api/users`로 들어온 GET 요청은 `getAllUsers()` 메서드를 호출합니다.
    - **4. 비즈니스 로직 실행**
        - 컨트롤러는 비즈니스 로직을 처리하기 위해 서비스 계층을 호출합니다.
        - 서비스 계층에서 데이터베이스 접근을 필요로 하는 경우, 서비스는 **Repository를 호출**하여 데이터를 가져옵니다.
    - **5. 응답 생성**
        - 컨트롤러는 처리된 결과를 **JSON 형식**으로 변환하여 클라이언트에 응답을 반환합니다.
        - Spring Boot는 자동으로 Java 객체를 **JSON**으로 직렬화(Serialization)하여 클라이언트에 반환합니다.

### 예시: 요청 및 응답.
- 클라이언트 요청.
```bash
GET http://localhost:8080/api/users
```

- 서버 응답
```json
[
    {
        "id": 1,
        "name": "Kobe",
        "email": "kobe@example.com"
    },
    {
        "id": 2,
        "name": "MinSeong",
        "email": "minseong@example.com"
    }
]
```

## 6️⃣ 데이터베이스 연동(Optional_
- 요청에 따라 비즈니스 로직에서 데이터베이스에 접근해야 하는 경우, **JPA**나 **Hibernate**와 같은 **ORM(Object-Relational Mapping)** 프레임워크를 통해 데이터베이스에서 데이터를 읽거나 저장할 수 있습니다.

### 데이터베이스 연동 예시(Repository 사용)
```java
@Repository
public interface UserRepository extend JpaRepository<User, Long> {
}
```

- 서비스 계층에서 `UserRepository`를 호출하여 데이터베이스에서 데이터를 조회하거나 저장하는 작업을 처리합니다.

## 7️⃣ 응답 전송 및 종료
- 컨트롤러에서 반환된 데이터를 **DispatcherServlet**이 받아서 적절한 **HTTP 응답(Response)** 으로 변환합니다.
- 응답 데이터는 JSON으로 변환된 후, HTTP 상태 코드와 함께 클라이언트로 전송됩니다.
    - 예를 들어, 요청이 성공적으로 **200 OK** 상태 코드와 함께 **JSON 데이터**가 전송됩니다.

## 8️⃣ 요약
- Java 백엔드 애플리케이션에서 API가 동작하는 과정은 크게 **서버 실행, 의존성 주입 및 컴포넌트 스캔, URL 매핑, 클라이언트 요청 처리, 그리고 응답 전송**의 단계로 이루어집니다.
- 클라이언트가 요청을 보내면 서버는 해당 요청을 적절한 컨트롤러 메서드로 라우팅하여 데이터를 처리하고, 그 결과를 응답으로 반환하는 일련의 과정이 수행됩니다.
