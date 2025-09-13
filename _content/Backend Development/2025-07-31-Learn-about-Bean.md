---
title: "📚[Backend Development] @Bean에 대해서 알아보기 🙌"
tags:
    - Backend Ddevelopment
    - DI
    - Server
    - CS
date: "2025-07-31"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# ✅ Intro.
Java 기반의 Spring Framework에서 @Bean은 의존성 주입(Dependency Injection)과 객체 관리를 이해하는 데 핵심적인 개념입니다.
아래에서 하나씩 알아봅시다. 🙌

## ✅ 1. @Bean은 무엇인가요?
- @Bean은 Spring Framework에서 **개발자가 직접 정의한 객체를 Spring 컨테이너에 등록할 때** 사용하는 **애노테이션(Annotation)** 입니다.
- 이 애노테이션은 @Configuration 클래스 내에서 사용되며, 메서드에 붙어서 **해당 메서드가 반환하는 객체를 Bean으로 등록합니다.**

```java
@Configuration
public class AppConfig {
    
    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }
}
```

---

## ✅ 2. @Bean의 역할은 무엇인가요?
- Spring 컨테이너에 **직접 Java 코드로 객체를 생성하고 등록합니다.**
- 등록된 객체는 Spring이 관리하게 되며, 다른 Bean에 **의존성 주입(DI)** 될 수 있습니다.
- 보통 다음과 같은 경우 사용됩니다.:
    - 외부 라이브러리나 우리가 직접 구현한 클래스인데, **Spring이 자동으로 생성해주지 않는 경우**
    - XML 설정을 대신해서 **Java 코드로 설정을 하고 싶을 때**

---

## ✅ 3. @Bean은 언제 사용하나요?

**📌 주요 사용 시점:**
- **컴포넌드 스캔(@Component)으로 등록할 수 없는 객체를 등록할 때**
(예: 외부 라이브러리 클래스, 제3자 라이브러리에서 제공하는 클래스 등)
- **객체 생성 로직이 복잡해서 수동으로 등록하고 싶을 때**
- 설정 클래스를 통해 **설정값에 따라 Bean을 조건부로 생성할 때**
- **테스트 환경에서 특정 객체만 대체해서 사용**하고 싶을 때

**예시:**
```java
@Configuration
public class RedisConfig {
    
    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(factory);
        return template;
    }
}
```

---

## ✅ 요약

| 질문 | 요약 답변 |
| -------- | -------- |
| @Bean은 무엇인가요? | 개발자가 수동으로 Spring Bean을 등록할 수 있게 해주는 애노테이션 |
| 역할은? | Spring 컨테이너에 메서드가 반환하는 객체를 Bean으로 등록 |
| 언제 사용하나요? | 컴포넌트 스캔이 어려운 외부 라이브러리나 복잡한 설정 객체를 등록할 때 |
