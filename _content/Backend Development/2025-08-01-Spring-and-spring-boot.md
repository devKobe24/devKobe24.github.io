---
title: "📚[Backend Development] 스프링과 스프링 부트 🙌"
tags:
    - Backend Ddevelopment
    - Spring
    - Spring Boot
    - Server
    - Framework
date: "2025-08-01"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# ✅ Intro.
**Spring과 Spring Boot**는 서로 밀접한 관계에 있지만, **목적과 사용 방식, 개발 편의성**에서 큰 차이가 있습니다.
아래에 구조적으로 차이점을 정리해드릴게요 🙌

---

## ✅ 요약: 한 줄 차이

| 구분 | 설명 |
| -------- | -------- |
| Spring | 순수한 프레임워크, 유연하지만 **설정이 많음** |
| Spring Boot | Spring을 쉽게 쓰기 위한 **자동 설정 기반의 도구 세트** |

---

## ✅ 1. Spring Framework란?

<img src = "https://github.com/devKobe24/images2/blob/main/spring.png?raw=true">

- 자바 기반 **웹 애플리케이션 개발을 위한 오픈소스 프레임워크**
- 핵심 개념: **IoC (제어의 역전), DI (의존성 주입), AOP (관점 지향 프로그래밍)**
- 사용 시에는 XML 또는 자바 코드로 **직접 많은 설정을 해야 함**

```xml
<!-- Spring (전통적) 방식 예시 -->
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">...</bean>
```

---

## ✅ 2. Spring Boot란?

<img src = "https://github.com/devKobe24/images2/blob/main/spring-boot.png?raw=true">

- **Spring을 더 쉽고 빠르게 개발**하기 위해 나온 확장 도구입니다.
- **자동 설정(AutoConfiguration), 내장 서버(Embedded Tomcat), 스타터(Starter)등을 통해 설정 없이도 바로 실행 가능한 스프링 환경을 제공합니다.**

```java
@SpringBootApplication
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }
}
```

---

## ✅ 3. 차이점 비교표

| 항목 | Spring | Spring Boot |
| -------- | -------- | -------- |
| 💡 목적 | 유연하고 확장 가능한 프레임워크 제공 | 설정 없이 빠른 개발 환경 제공|
| ⚙️ 설정 | 수동 설정 많음 (XML, JavaConfig 등) | 자동 설정 중심 (AutoConfiguration) |
| 🚀 실행 | 톰캣 설치 필요 | 내장 톰캣으로 단독 실행 가능 (java -jar) |
| 📦 의존성 | 직접 관리 | Starter로 간단히 관리 (예: spring-boot-starter-web) |
| 🛠️ 프로젝트 구조 | 구조 설계부터 직접 구성 | 관례 기반 기본 구조 제공 |
| 🧪 테스트 환경 | 복잡하게 구성 | 내장된 테스트 도구 쉽게 사용 가능 |
| 📈 생산성 | 초반 진입 장벽 있음 | 매우 높음 (간편한 설정, 빠른 실행) |

---

## ✅ 결론: 언제 사용하나?

| 상황 | 추천 |
| -------- | -------- |
| 유연한 아키텍처 필요, 라이브러리 직접 제어 | 🔹 Spring (Core Framework) |
| 빠르게 웹 서비스 시작, 실무 생산성 중시 | 🔸 Spring Boot (현대 개발의 표준) |

---

## 🎯 최종 요약

> **Spring Boot**는 **Spring Framework**를 **기반으로 개발을 더 쉽게 만들어주는 "자동화 도구 세트"입니다.**
> 따라서 Spring Boot를 사용하면 Spring을 더 효율적이고 간편하게 활용할 수 있습니다.
