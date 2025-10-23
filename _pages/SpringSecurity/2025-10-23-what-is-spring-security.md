---
title: 🔐[Spring Security] Spring Security란?
tags:
  - Spring Security
  - Framework
  - Authentication
  - Authorization
date: "2025-10-23"
thumbnail: "/assets/img/thumbnail/springsecurity.jpg"
---

# 🔐[Spring Security] Spring Security란?

## 목차

- [Spring Security 개요](#spring-security-개요)
- [핵심 개념: 인증과 인가](#핵심-개념-인증과-인가)
- [동작 원리: 서블릿 필터 체인](#동작-원리-서블릿-필터-체인)
- [사용해야 하는 이유](#사용해야-하는-이유)
- [요약](#요약)

---

## Spring Security 개요

Spring Security는 Spring 기반 애플리케이션의 **보안(인증과 인가)을 전담**하는 강력하고 필수적인 프레임워크입니다.

### 🎯 핵심 역할

> **"우리 애플리케이션의 문지기이자 접근 제어 관리자"**

Spring Security가 없다면?

- 개발자가 직접 세션 관리 코드 작성
- URL마다 권한 체크 로직 중복 작성
- 모든 컨트롤러와 서비스에 보안 코드가 뒤섞임

Spring Security는 이 모든 것을 **애플리케이션의 핵심 로직과 분리**하여 체계적으로 처리합니다.

---

## 핵심 개념: 인증과 인가

Spring Security의 존재 이유는 바로 이 두 가지 개념을 해결하기 위함입니다.

### 1️⃣ 인증 (Authentication)

**"당신은 누구십니까?"**

사용자가 자신의 신원을 증명하는 과정입니다.

#### 인증 방법 예시

- ID/Password 입력
- 소셜 로그인 (OAuth2)
- JWT 토큰 제출
- 생체 인증

#### 동작 과정

1. 사용자가 로그인 요청
2. Spring Security가 요청을 가로채서 Credentials 검증
3. 인증 성공 시 `SecurityContext`에 `Authentication` 객체 저장

```java
// 인증 정보 조회 예시
Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
String username = authentication.getName();
```

### 2️⃣ 인가 (Authorization)

**"당신이 이 작업을 할 권한이 있습니까?"**

인증된 사용자가 특정 리소스에 접근할 권한이 있는지 확인하는 과정입니다.

#### 권한 체크 예시

- `/admin` → `ROLE_ADMIN` 권한 필요
- `/my-page` → `ROLE_USER` 권한으로 충분
- `/public` → 누구나 접근 가능

#### 설정 방법

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .requestMatchers("/user/**").hasAnyRole("USER", "ADMIN")
                .requestMatchers("/public/**").permitAll()
                .anyRequest().authenticated()
            );
        return http.build();
    }
}
```

```java
// 메소드 레벨 보안
@PreAuthorize("hasRole('ADMIN')")
public void deleteUser(Long userId) {
    // ADMIN만 실행 가능
}
```

---

## 동작 원리: 서블릿 필터 체인

이것이 Spring Security의 **가장 중요한 아키텍처**입니다.

### 📌 핵심 개념

Spring Security는 **FilterChainProxy**라는 하나의 거대한 필터를 서블릿 컨테이너(Tomcat)에 등록합니다.

이 필터 내부에는 **여러 개의 보안 필터들**로 구성된 **체인(Chain)**이 존재합니다.

### 🔄 요청 처리 흐름

모든 웹 요청은 컨트롤러에 도달하기 **전에 반드시** 이 필터 체인을 통과합니다.

```
Client Request
    ↓
┌─────────────────────────────────┐
│  Spring Security Filter Chain   │
├─────────────────────────────────┤
│  1. CorsFilter                  │
│  2. CsrfFilter                  │
│  3. JwtAuthenticationFilter     │
│  4. UsernamePasswordAuthFilter  │
│  5. ExceptionTranslationFilter  │
│  6. AuthorizationFilter         │
└─────────────────────────────────┘
    ↓
DispatcherServlet
    ↓
Controller
```

### 💡 JWT 기반 인증 시 필터 체인 동작 예시

#### 1단계: 요청 시작

```
클라이언트가 Authorization 헤더에 JWT를 담아 요청
GET /api/user/profile
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

#### 2단계: 필터 체인 통과

**① CorsFilter**

- CORS 정책 확인
- Cross-Origin 요청 허용 여부 검증

**② CsrfFilter**

- CSRF 토큰 검증 (Stateless 환경에서는 보통 비활성화)

**③ JwtAuthenticationFilter** (커스텀 필터)

```java
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain) {
        // 1. 헤더에서 JWT 추출
        String token = extractToken(request);

        // 2. JWT 유효성 검증
        if (token != null && jwtTokenProvider.validateToken(token)) {
            // 3. 토큰에서 사용자 정보 추출
            Authentication auth = jwtTokenProvider.getAuthentication(token);

            // 4. SecurityContext에 인증 정보 저장
            SecurityContextHolder.getContext().setAuthentication(auth);
        }

        filterChain.doFilter(request, response);
    }
}
```

**④ ExceptionTranslationFilter**

- 인증/인가 과정에서 발생하는 예외 처리
- `AuthenticationException`, `AccessDeniedException` 처리

**⑤ AuthorizationFilter** (과거 `FilterSecurityInterceptor`)

```java
// 사용자가 요청한 URL에 접근 권한이 있는지 최종 확인
// 예: /admin/** → ROLE_ADMIN 권한 필요
```

#### 3단계: 컨트롤러 도달

모든 필터를 통과하면 DispatcherServlet을 거쳐 **Controller의 로직이 실행**됩니다.

```java
@RestController
@RequestMapping("/api/user")
public class UserController {

    @GetMapping("/profile")
    public UserProfile getProfile() {
        // SecurityContext에서 인증된 사용자 정보 조회
        Authentication auth = SecurityContextHolder.getContext().getAuthentication();
        String username = auth.getName();

        return userService.getProfile(username);
    }
}
```

---

## 사용해야 하는 이유

### ✅ 1. 표준화된 보안

검증되고 표준화된 방식으로 보안을 적용하여 개발자의 실수로 인한 보안 허점을 최소화합니다.

### ✅ 2. 관심사의 분리 (Separation of Concerns)

보안 로직이 비즈니스 로직(컨트롤러, 서비스)과 완벽하게 분리됩니다.

```java
// ❌ Spring Security 없이
@GetMapping("/admin/users")
public List<User> getUsers(HttpSession session) {
    // 보안 로직이 비즈니스 로직과 섞임
    if (session.getAttribute("user") == null) {
        throw new UnauthorizedException();
    }
    User user = (User) session.getAttribute("user");
    if (!"ADMIN".equals(user.getRole())) {
        throw new ForbiddenException();
    }
    return userService.getAllUsers();
}

// ✅ Spring Security 사용
@GetMapping("/admin/users")
@PreAuthorize("hasRole('ADMIN')")
public List<User> getUsers() {
    return userService.getAllUsers(); // 비즈니스 로직만 집중
}
```

### ✅ 3. 강력한 방어 기능

웹의 고전적인 보안 공격을 기본적으로 방어합니다.

- **CSRF (Cross-Site Request Forgery)** 방어
- **세션 고정 (Session Fixation)** 공격 방어
- **클릭재킹 (Clickjacking)** 방어
- **XSS (Cross-Site Scripting)** 보호 헤더 자동 설정

### ✅ 4. 높은 확장성

필터를 직접 추가하거나 인증 방식을 커스터마이징하는 것이 매우 유연합니다.

```java
@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            // 커스텀 필터 추가
            .addFilterBefore(jwtAuthenticationFilter(),
                           UsernamePasswordAuthenticationFilter.class)

            // OAuth2 로그인 설정
            .oauth2Login(oauth2 -> oauth2
                .userInfoEndpoint(userInfo -> userInfo
                    .userService(customOAuth2UserService)
                )
            )

            // JWT 인증 설정
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            );

        return http.build();
    }
}
```

---

## 요약

### 🎯 Spring Security란?

Spring Security는 Spring 애플리케이션 보안의 **사실상 표준**이며, 인증/인가를 위한 **필수 인프라**를 제공하는 프레임워크입니다.

### 📋 핵심 포인트

| 구분          | 설명                                          |
| ------------- | --------------------------------------------- |
| **핵심 기능** | 인증(Authentication) + 인가(Authorization)    |
| **동작 방식** | 서블릿 필터 체인을 통한 요청 가로채기         |
| **주요 장점** | 표준화, 관심사 분리, 강력한 방어, 높은 확장성 |
| **적용 범위** | URL 기반, 메소드 기반 보안 모두 지원          |

### 🚀 다음 학습 주제

- Spring Security Configuration 심화
- JWT 인증 구현
- OAuth2 / OpenID Connect
- Method Security (`@PreAuthorize`, `@Secured`)
- Custom Filter 구현
- Password Encoding (BCrypt)
- Remember-Me 인증

---

**참고**: 이 문서는 Spring Security의 기본 개념을 다룹니다. 실제 프로젝트 적용 시에는 보안 요구사항에 맞게 세부 설정을 조정해야 합니다.
