---
title: 🔐 Spring Security와 Spring Boot의 관계
tags:
  - Spring Security
  - Framework
  - Spring Boot
  - Auto-Configuration
date: "2025-10-23"
thumbnail: "/assets/img/thumbnail/springsecurity.jpg"
---

# 🔐 Spring Security와 Spring Boot의 관계

## 목차

- [핵심 개념](#핵심-개념)
- [Spring Security의 독립성](#spring-security의-독립성)
- [Spring Boot의 역할](#spring-boot의-역할)
- [실전 예시](#실전-예시)
- [정리](#정리)

---

## 핵심 개념

> **Spring Security는 Spring Boot에 포함된 것이 아니라,
> Spring Boot가 Spring Security를 매우 쉽게 사용할 수 있도록 지원하는 구조입니다.**

간단히 말해:

- Spring Security == 보안 전문 프레임워크 (독립적)
- Spring Boot == 여러 Spring 프로젝트를 쉽게 조립해주는 도구

---

## Spring Security의 독립성

### 📦 별개의 프로젝트

Spring Security는 **"Spring 생태계"에 속한 독립적인 프로젝트**입니다.

```
Spring 생태계
├── Spring Framework (Core)
├── Spring Boot (Auto-Configuration)
├── Spring Security (보안)
├── Spring Data JPA (데이터 접근)
├── Spring Web MVC (웹)
├── Spring Cloud (마이크로서비스)
└── ... 기타 프로젝트들
```

### 🕰️ 역사적 맥락

- **Spring Security**: 2003년 "Acegi Security"로 시작
- **Spring Boot**: 2014년 첫 릴리즈

Spring Boot가 탄생하기 **10년 이상 전**부터 Spring Security는 존재했습니다.

### 🎯 독립적 사용 가능

Spring Security는 Spring Boot 없이도 사용할 수 있습니다.

```xml
<!-- Spring Framework + Spring Security (Spring Boot 없이) -->
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-web</artifactId>
    <version>6.2.0</version>
</dependency>
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-config</artifactId>
    <version>6.2.0</version>
</dependency>
```

하지만 이 경우 **모든 설정을 개발자가 직접 해야 합니다** (매우 복잡함).

---

## Spring Boot의 역할

### 🚀 Auto-Configuration의 힘

Spring Boot는 복잡한 설정을 자동화하여 개발자의 삶을 획기적으로 개선했습니다.

#### ❌ Spring Boot 이전 (전통적인 Spring)

```xml
<!-- web.xml에 필터 등록 -->
<filter>
    <filter-name>springSecurityFilterChain</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>

<filter-mapping>
    <filter-name>springSecurityFilterChain</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

```java
// SecurityConfig.java - 수많은 빈 설정 필요
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Bean
    public AuthenticationManager authenticationManager() {
        // 복잡한 설정...
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public UserDetailsService userDetailsService() {
        // 사용자 정보 로드 설정...
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 필터 체인 수동 구성
        http
            .authorizeRequests()
            .antMatchers("/public/**").permitAll()
            .anyRequest().authenticated()
            .and()
            .formLogin()
            .and()
            .logout();
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        // 인증 매니저 설정...
    }
}
```

#### ✅ Spring Boot 사용 시

```gradle
// build.gradle - 단 한 줄의 의존성 추가
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-security'
}
```

```xml
<!-- 또는 pom.xml -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

**이것만으로 끝!** Spring Boot가 자동으로:

- Spring Security 필터 체인 등록
- 기본 로그인 페이지 생성
- 세션 기반 인증 설정
- CSRF 보호 활성화
- 기본 사용자 계정 생성 (username: user, password: 콘솔에 출력)

```
Using generated security password: 8e557245-73e2-4286-969a-ff57fe326336
```

### 🎛️ 커스터마이징도 간단

필요한 부분만 오버라이드하면 됩니다.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/public/**").permitAll()
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .formLogin(Customizer.withDefaults())
            .oauth2Login(Customizer.withDefaults());  // OAuth2도 쉽게!

        return http.build();
    }
}
```

---

## 실전 예시

### 📋 의존성 관계 이해하기

Spring Boot는 여러 Spring 프로젝트를 **조립하여 쉽게 사용하게 해주는 '조립 도구'**입니다.

```
┌─────────────────────────────────────┐
│         Spring Boot                 │
│    (조립 도구 / 런처)                │
└─────────────────────────────────────┘
              ↓ 선택적으로 통합
    ┌─────────┬─────────┬─────────┐
    │         │         │         │
┌───▼───┐ ┌──▼───┐ ┌───▼───┐ ┌──▼───┐
│Spring │ │Spring│ │Spring │ │Spring│
│Security│ │Data  │ │  Web  │ │Cloud │
│       │ │  JPA │ │  MVC  │ │      │
└───────┘ └──────┘ └───────┘ └──────┘
```

### 🛠️ 실제 프로젝트 설정

#### 1. 기본 웹 애플리케이션

```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
}
```

이 상태에서는 **보안이 전혀 없습니다** (모든 엔드포인트 공개).

#### 2. 보안 추가

```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-security'  // 추가
}
```

이제 **모든 엔드포인트가 보호**됩니다 (기본 폼 로그인 활성화).

#### 3. JWT 인증으로 확장

```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-security'
    implementation 'io.jsonwebtoken:jjwt-api:0.12.3'
    runtimeOnly 'io.jsonwebtoken:jjwt-impl:0.12.3'
    runtimeOnly 'io.jsonwebtoken:jjwt-jackson:0.12.3'
}
```

```java
@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable())  // Stateless이므로 CSRF 비활성화
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            )
            .addFilterBefore(jwtAuthenticationFilter(),
                           UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
}
```

### 🔄 선택의 자유

개발자는 `build.gradle` 또는 `pom.xml`을 통해 **필요한 것만 선택**합니다.

| 필요한 기능        | 추가할 Starter                               |
| ------------------ | -------------------------------------------- |
| 웹 애플리케이션    | `spring-boot-starter-web`                    |
| 보안               | `spring-boot-starter-security`               |
| 데이터베이스 (JPA) | `spring-boot-starter-data-jpa`               |
| OAuth2 클라이언트  | `spring-boot-starter-oauth2-client`          |
| OAuth2 리소스 서버 | `spring-boot-starter-oauth2-resource-server` |

---

## 정리

### 🎯 핵심 요약

| Spring Security                   | Spring Boot                              |
| --------------------------------- | ---------------------------------------- |
| **독립적인** 보안 전문 프레임워크 | 여러 Spring 프로젝트를 **조립하는** 도구 |
| 2003년부터 존재 (Acegi Security)  | 2014년 탄생                              |
| Spring Boot 없이도 사용 가능      | Security 없이도 사용 가능                |
| 인증/인가의 모든 기능 제공        | Auto-Configuration으로 설정 자동화       |

### 📌 관계 정리

```
Spring Security ≠ Spring Boot의 일부

Spring Security = Spring 생태계의 독립 프로젝트
Spring Boot = Spring Security를 쉽게 사용하게 해주는 지원 도구
```

### 💡 실무 관점

**"Spring Boot를 사용한다"**는 것은:

- Spring Framework를 기반으로
- 필요한 Spring 프로젝트들(Security, Data JPA 등)을 선택하여
- Auto-Configuration의 도움으로 빠르게 개발하는 것

**"Spring Security를 사용한다"**는 것은:

- 애플리케이션에 인증/인가 기능을 추가하는 것
- Spring Boot가 없어도 가능하지만, Spring Boot와 함께 쓰면 훨씬 편리함

### 🚀 다음 학습 주제

- Spring Boot Auto-Configuration 내부 동작 원리
- `spring-boot-starter-security`가 자동 설정하는 Bean들
- Custom Auto-Configuration 만들기
- Spring Boot Actuator와 Security 통합

---

**결론**: Spring Security는 Spring Boot의 일부가 아니라, **Spring Boot가 강력하게 지원하고 통합해 주는 별개의 보안 전문 프레임워크**입니다.
