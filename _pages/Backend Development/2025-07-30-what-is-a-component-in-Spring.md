---
title: "📚[Backend Development] Spring에서의 컴포넌트란?"
tags:
    - Backend Ddevelopment
    - Component
    - Server
    - Build System
date: "2025-07-30"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 📚[Backend Development] Spring에서의 컴포넌트란?

Spring에서 자주 쓰이는 **컴포넌트(Component)** 개념을 정리해봅시다. 🎁

## ✅ 1. 컴포넌트란 무엇인가요?
- **Spring에서 관리하는 객체(Bean)** 를 의미합니다.
- `@Component` 애노테이션을 불티면 해당 클래스는 **Spring 컨테이너가 자동으로 탐색(컴포넌트 스캔)하여 Bean으로 등록합니다.**
- 즉, 개발자가 직접 `@Bean`으로 등록하지 않아도 Spring이 자동으로 객체를 생성하고 관리합니다.

```java
@Component
public class MyService {
    public void doSomething() {
        System.out.println("Service logic!");
    }
}
```

## ✅ 2. 컴포넌트의 역할은 무엇인가요?
- **자동 등록 :** 클래스에 `@Component`를 붙이면 스프링이 자동으로 객체를 생성하고, 의존성을 주입할 수 있도록 관리합니다.
- **의존성 주입(DI) :** 등록된 컴포넌트는 다른 클래스에 `@Autowired` 또는 생성자 주입으로 쉽게 사용할 수 있습니다.
- **애플리케이션 구성 요소 구분 :** `@Service`, `@Repository`, `@Controller` 등은 모두 `@Component`의 특화된 버전입니다. 역할별로 구분하기 좋게 도와줍니다.

## ✅ 3. 컴포넌트는 언제 사용하나요?
- 일반적인 애플리케이션 로직 클래스를 Bean으로 등록하고 싶을 때 사용합니다.
- 자동 등록이 가능한 경우 사용합니다.(Spring이 직접 생성할 수 있는 클래스)
- 주요 사례:
    - 1. **서비스 클래스(@Service) :** 비즈니스 로직 담당
    - 2. **리포지토리 클래스(@Repository) :** DB 접근 담당
    - 3. **컨트롤러 클래스(@Controller, @RestController) :** API/화면 담당
    - > 위 애노테이션들은 모두 내부적으로 @Component를 포함하고 있어서 자동 등록됩니다.

## 📌 요약

| 질문 | 요약 답변 |
| -------- | -------- |
| 컴포넌트란? | Spring이 자동으로 Bean으로 등록하는 클래스|
| 역할은? | 객체를 자동 생성 및 관리하고 DI를 지원 |
| 언제 사용하나요? | 서비스, 리포지토리, 컨트롤러 등 자동 등록 가능한 클래스일 때 |

## 🙌 추가로 알아두면 좋은 점
- @Component는 자동 등록, @Bean은 수동 등록이라는 차이가 있습니다.
- 자동 등록이 불가능한 외부 라이브러리나 복잡한 생성 로직이 있으면 @Bean을 쓰고, 일반적인 애플리케이션 클래스라면 @Component를 쓰는 것이 기본입니다.
