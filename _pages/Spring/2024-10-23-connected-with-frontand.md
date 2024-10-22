---
title: 🍃[Spring] Spring Boot를 사용하여 HTML, CSS, JS로 만든 Frontend 페이지를 로컬 서버와 연결하는 방법은 무엇일까요?
tags:
    - Spring
    - Framework
date: "2024-10-23"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Spring Boot를 사용하여 HTML, CSS, JS로 만든 Frontend 페이지를 로컬 서버와 연결하는 방법은 무엇일까요?

- Spring Boot를 사용하여 HTML, CSS, JavaScript로 만든 프런트엔드 페이지를 로컬 서버와 연결하려면 다음과 같은 단계가 필요합니다.
    - 이 과정에서는 정적 리소스를 Spring Boot 프로젝트에 포함하고, 컨트롤러를 통해 요청을 처리하여 정적페이지를 제공하는 방법을 설명합니다.

## 1️⃣ Spring Boot 프로젝트에 정적 파일 추가.
- HTML, CSS, JavaScript 파일은 Spring Boot 프로젝트의 정적 리소스 디렉토리에 배치합니다.

### 👉 프로젝트 디렉토리 구조.
```bash
src
 └── main
     ├── java
     │    └── com.example.demo (패키지 구조)
     │         └── DemoApplication.java
     ├── resources
     │    ├── static
     │    │    ├── css
     │    │    │    └── styles.css
     │    │    ├── js
     │    │    │    ├── scripts.js
     │    │    │    └── login-scripts.js
     │    │    ├── signup.html
     │    │    └── login.html
     │    └── templates (필요한 경우 Thymeleaf 템플릿 파일)
     └── application.properties
```
- static 디렉토리는 기본적으로 정적 리소스를 제공하는 역할을 하며, 여기서 CSS, JavaScript, 이미지 및 HTML 파일을 로드할 수 있습니다.
- HTML 파일(signup.html, login.html)은 직접 static 디렉토리에 배치하여 서버가 요청을 받으면 직접 로드되도록 합니다.

## 2️⃣ Spring Boot 컨트롤러 설정.
- Spring Boot 에서 `/login`과 `/signup` 요청을 처리하도록 간단한 컨트롤러를 만듭니다.

```java
package com.example.demo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class AuthController {
    
    @GetMapping("/login")
    public String loginPage() {
        return "login.html"; // static/login.html 파일을 반환
    }
    
    @GetMapping("/signup")
    public String signupPage() {
        return "signup.html"; // static/signup.html 파일을 반환
    }
}
```

## 3️⃣ 정적 파일 및 자원 접근
- HTML 파일에서 CSS 및 JavaScript 파일에 대한 경로를 수정하여 Spring Boot에서 제공하는 정적 리소스를 제대로 불러올 수 있게합니다.

### 👉 login.html(예시)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login Page</title>
    <link rel="stylesheet" href="/css/styles.css"> <!-- 절대 경로로 수정 -->
</head>
<body>
    <div class="login-container">
        <form id="loginForm" class="login-form">
            <h2>Login</h2>
            <div class="input-group">
                <label for="login-email">Email</label>
                <input type="email" id="login-email" name="login-email" required>
            </div>
            <div class="input-group">
                <label for="login-password">Password</label>
                <input type="password" id="login-password" name="login-password" required>
            </div>
            <button type="submit">Login</button>
            <p class="signup-link">Don't have an account? <a href="/signup">Sign up here</a></p> <!-- Spring Boot 경로 사용 -->
        </form>
    </div>
    <script src="/js/login-scripts.js"></script> <!-- 절대 경로로 수정 -->
</body>
</html>
```

## 4️⃣ Spring Boot 애플리케이션 실행
- 이제 Spring Boot 애플리케이션을 실행하면 로컬 서버에서 `/login` 및 `/signup` 경로로 접근하여 프런트엔드 페이지를 볼 수 있습니다.

### 👉 애플리게이션 실행 명령어
```bash
./gradlwe bootRun #Gradle 사용시
```

```bash
mvn spring-boot:run # Maven 사용시
```

## 5️⃣ 추가 설정(필요한 경우)

### 1️⃣ CORS 설정
- 프런트엔드 페이지에서 다른 서버의 API에 접근하려면 CORS 설정이 필요할 수 있습니다.
    - WebMvcConfigurer를 사용하여 다음과 같이 설정할 수 있습니다.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**")
                        .allowedOrigins("http://localhost:8080")
                        .allowedMethods("GET", "POST", "PUT", "DELETE");
            }
        };
    }
}
```

> 📝 CORS(Cross-Origin Resource Sharing)
> 
> 웹 브라우저에서 실행되는 웹 애플리케이션이 **다른 도메인**에서 호스팅 되는 리소스에 접근할 수 있도록 하는 **보안 메커니즘**입니다.
> 기본적으로 웹 브라우저는 보안상의 이유로, 한 도메인에서 로드된 웹 페이지가 다른 도메인에 요청을 보내는 것을 제한합니다.
> 이를 **"동일 출처 정책(Same-Origin-Policy)"라고** 합니다.

### 2️⃣ API와 연결.
- 로그인, 회원가입 등의 기능을 구현하기 위해 백엔드 API를 작성할 수 있습니다.
    - 예를 들어, `/login` API를 만들어 JavaScript에서 해당 API로 AJAX 요청을 보낼 수 있습니다.

> 📝 AJAX(Asynchronous JavaScript and XML)
> 
> 웹 페이지를 **새로고침하지 않고** 비동기 방식으로 서버와 데이터를 주고받을 수 있게 해주는 기술을 말합니다.
> 웹 페이지를 동적으로 업데이트 할 수 있기 때문에 사용자가 더 빠르고 직관적인 경험을 할 수 있도록 도와줍니다.

## 6️⃣ 요약.
- Spring Boot 프로젝트의 static 디렉토리에 HTML, CSS, JavaScript 파일을 넣어 정적 리소스를 제공합니다.
- 컨트롤러를 통해 `/login` 및 `/signup` 경로를 설정하고 HTML 파일을 로드합니다.
- JavaScript 코드는 프런트엔드에서 필요한 동작을 처리하며, 추가로 API를 호출할 수 있습니다.
