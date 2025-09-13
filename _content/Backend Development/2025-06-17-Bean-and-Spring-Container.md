---
title: "📚[Backend Development] 빈(Bean)과 스프링 컨테이너(Spring Container)"
tags:
    - Backend Ddevelopment
    - Database
    - Server
    - Build System
date: "2025-06-17"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 📚[Backend Development] 빈(Bean)과 스프링 컨테이너(Spring Container)
**Bean과 Spring Container**는 Spring Framework의 핵심 개념으로, **스프링이 객체를 관리하는 방식을 이해**하는 데 매우 중요합니다.

## ✅ 1. 빈(Bean)이란?
### 📌 정의
**스프링 컨테이너에 의해 생성되고 관리되는 객체**
- 개발자가 @Component, @Service, @Repository, @Configuration, @Bean 등의 어노테이션으로 등록하면 스프링이 해당 객체를 생성하고, 의존성을 주입하며, 생명 주기를 관리합니다.

🔧**Bean의 예시**
```java
@Component
public class UserService {
    // 이 클래스는 스프링 빈이다.
}
```

또는

```java
@Configuration
public class AppConfig {
    
    @Bean
    public ArticleService articleService() {
        retutn new ArticleService();
    }
}
```

## ✅ 2. 스프링 컨테이너(Spring Container)란?
### 📌 정의
**빈을 생성하고 관리하는 객체 저장소**
- **ApplicationContext** 또는 **BeanFactory**가 대표적인 컨테이너입니다.
- 애플리케이션 실행 시 컨테이너가 초기화되며, 내부에 Bean 객체들을 싱글톤으로 보관하고 관리합니다.

### 🧠 비유
> "스프링 컨테이너는 빈을 담는 **객체 창고**다. 필요한 객체(Bean)를 꺼내 쓰면 된다."

### ⛓️ 주요 역할
- Bean 생성 및 초기화
- 의존성 주입 (DI)
- Bean 생명주기 관리(@PostConstruct, @PreDestory 등)
- AOP 기능 제공

### ✅ 관계 정리

| 개념 | 설명 |
| -------- | -------- |
| Bean | 스프링이 관리하는 객체 |
| Container | Bean들을 생성하고 관리하는 스프링의 핵심 엔진 |
| 등록 방법 | @Component, @Service, @Repository, @Configuration + @Bean 등 |
| 제공 클래스 | ApplicationContext, AnnotationConfigApplicatioonContext, WebAppplicationContext 등 |

### ✅ 그림으로 비유
```
[Spring Container]
 ├── UserService (Bean)
 ├── ArticleService (Bean)
 ├── UserRepository (Bean)
 └── DataSource (Bean)
```
➞ 개발자는 직접 new 하지 않고, 스프링이 대신 생성해주는 Bean을 받아서 사용합니다.

### ✅ 요약

| 개념 | 설명 |
| -------- | -------- |
| Bean | 스프링이 생성하고 관리하는 객체 |
| Spring Container | Bean의 생성, 주입, 생명주기를 관리하는 객체 저장소 |
| 의미 | IoC/DI를 실현하는 핵심 구조 |
