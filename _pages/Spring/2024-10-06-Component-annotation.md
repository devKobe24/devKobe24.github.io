---
title: 🍃[Spring] `@Component` 어노테이션.
tags:
    - Spring
    - Framework
date: "2024-10-06"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] `@Component` 어노테이션.

- `@Component` 어노테이션은 Spring Framework에서 **빈(Bean)** 으로 등록할 클래스를 지정하기 위해 사용하는 **클래스 레벨 어노테이션**입니다.
- 이 어노테이션을 사용하면 해당 클래스가 **Spring IoC 컨테이너에 의해 자동으로 관리되는 빈**으로 등록됩니다.
- 주로 애플리케이션에서 자동으로 빈을 등록하고 싶을 때 사용 됩니다.

> 🙋‍♂️ [Spring 컨테이너](https://www.devkobe24.com/Backend/Spring/2024-10-05-Spring-container.html)
> 🙋‍♂️ [Spring 컨테이너를 사용하는 이유](https://www.devkobe24.com/Backend/Spring/2024-10-05-why-use-spring-containers.html)


> 📝 **Spring IoC 컨테이너와 Spring 컨테이너는 다른 개념인가요?**
> 
> **Spring IoC 컨테이너**와 **Spring 컨테이너**는 **같은 개념**을 의미하는 용어입니다.
> 이 두 용어는 모두 Spring Framework에서 **객체(Bean, 빈)의 생성, 관리, 의존성 주입, 생명주기 관리 등을 담당하는 컨테이너를 가리킵니다.**
> 
> 📝 **같은 개념에 대한 다양한 표현**
> - **Spring IoC 컨테이너**는 더 구체적인 용어로, Spring에서 **Inversion Of Control(제어의 역적)** 원칙을 구현하는 **빈 관리 시스템**을 가리킵니다.
>> - 이 용어는 주로 **제어의 역전(Inversion of Control)** 이라는 프로그래밍 원칙을 강조하기 위해 사용됩니다.
>> - IoC는 개발자가 직접 객체를 생성하고 관리하지 않고, 컨테이너가 객체의 생성과 의존성을 관리하는 방식입니다.
> - **Spring 컨테이너**는 더 일반적인 용어로, Spring Framework가 제공하는 객체 관리 시스템을 가리키는 표현입니다.
>> - 이 용어는 Spring이 제공하는 컨테이너의 기능을 포괄적으로 표현하며, 그 핵심 기능은 **IoC 컨테이너** 입니다.
>
> 📝 **결론**
> - Spring IoC 컨테이너와 Spring 컨테이너는 **같은 개념으로 볼 수 있으며, 두 용어 모두 Spring의 핵심 기능인 객체 관리와 의존성 주입을 담당하는 시스템을 가리킵니다.**
> - `IoC`는 이 컨테이너의 작동 원리를 설명하는 용어이고, `Spring 컨테이너`는 이를 일반적으로 부를 때 사용하는 표현힙니다.

## 1️⃣ 주요 기능 및 특징.

### 1. 자동 빈 등록.
- `@Component` 어노테이션이 붙은 클래스는 Spring의 **컴포넌트 스캔** 가능에 의해 자동으로 감지되고, Spring IoC 컨테이너에 빈으로 등록됩니다.
- 개발자가 직접 빈을 등록하는 대신, Spring이 클래스 경로에서 이를 탐색하고 관리하게 됩니다.

### 2. 다른 특화된 어노테이션의 기반
- `@Component`는 Spring에서 일반적인 빈을 등록하는 용도로 사용되며, 이를 기반으로 한 더 구체적인 어노테이션이 있습니다.
- 예를 들어, `@Service`, `@Repository`, `@Controller` 등이 `@Component`의 특수화된 형태로, 각각의 역할에 맞는 빈을 구체적으로 정의합니다.
    - `@Service`: 서비스 레이어를 정의하는 클래스에 사용.
    - `@Repository`: 데이터 엑세스 레이어(DAO)에 사용.
    - `@Controller`: 웹 컨트롤러(프레젠테이션 레이어)에 사용.

### 3. 간편한 빈 관리
- `@Component`는 특별한 설정 없이 클래스에 간단히 어노테이션만 붙여서 Spring IoC 컨테이너에서 관리되는 빈으로 만들 수 있습니다.
    - Spring이 제공하는 기본적인 빈 등록 방식 중 하나입니다.

## 2️⃣ 예시
```java
@Component
public class MyComponent {
    public void doSomething() {
        System.out.println("Hello from MyComponent!");
    }
}
```

- 위 코드에서 `MyComponent` 클래스는 `@Component` 어노테이션 덕분에 Spring IoC 컨테이너에 자동으로 빈으로 등록됩니다.
    - 이제 Spring이 이 빈을 관리하며, 다른 곳에서 의존성 주입을 통해 사용할 수 있습니다.

```java
@Service
public class MyService {
    
    private final MyComponent myComponent;
    
    @Autowired
    public MyService(MyComponent myComponent) {
        this.myComponent = myComponent;
    }
    
    public void performAction() {
        myComponent.doSomthing();
    }
}
```

- 이 예에서는 `MyService` 클래스가 `MyComponent` 빈을 주입받아 사용합니다.
    - `MyComponent` 가 자동으로 빈으로 등록되었기 때문에, `@Autowired`를 통해 `MyService`에 주입될 수 있습니다.

## 3️⃣ `@Component`와 컴포넌트 스캔
- `@Component` 어노테이션이 사용되려면 Spring이 **컴포넌트 스캔**을 통해 해당 클래스를 찾아야 합니다.
- 일반적으로 `@ComponentScan` 어노테이션이나 Spring Boot에서는 `@SpringBootApplication` 어노테이션을 통해 자동으로 컴포넌트 스캔이 활성화됩니다.

```java
@SpringBootApplication
public class MySpringBootApplication {
    public static void main(String[] args) {
        SpringApplication.run(MySpringBootApplication.class, args);
    }
}
```

- 이 코드에서 `@SpringBootApplication`은 `@ComponentScan`을 포함하고 있어, `@Component`가 붙은 모든 클래스를 자동으로 검색하고 빈으로 등록합니다.

## 4️⃣ `@Component`와 다른 빈 등록 방식 비교.
- `@Component` vs `@Bean`
    - `@Component`는 **클래스 레벨**에서 자동으로 빈을 등록하며, 컴포넌트 스캔을 통해 Spring이 클래스를 감지합니다.
    - `@Bean`은 **메서드 레벨**에서 수동으로 빈을 등록하는 방식으로, 자바 설정 클래스(`@Configuration`)내에서 사용됩니다.
        - 개발자가 직접 메서드를 통해 빈을 생성하고 초기화할 수 있습니다.
- `@Component` vs `@Service`, `@Repository`, `@Controller`
    - 이들 어노테이션은 모두 `@Component`를 기반으로 하며, 특정 레이어의 역할을 나타내기 위해 사용됩니다.
        - 예를 들어, `@Service`는 서비스 계층을 나타내고, `@Repository`는 데이터 엑세스 계층을 `@Controller`는 프레젠테이션 계층을 나타냅니다.
    - 기능적으로는 동일하지만, 역할에 따라 어노테이션을 사용함으로써 코드의 가독성과 의미를 더 명확하게 할 수 있습니다.

## 5️⃣ 언제 사용해야 하는가?
- **일반적인 빈 등록이 필요할 때**
    - 비즈니스 로직, 유틸리티 클래스, 도메인 객체 등 Spring IoC 컨테이너에 의해 관리되어야 하는 일반적인 클래스를 빈으로 등록할 때 사용합니다.
- **특정 역할이 없는 클래스**
    - `@Service`, `@Repository`, `@Contoroller`와 같은 명시적인 역할이 없는 경우, `@Component`를 사용하여 클래스를 빈으로 등록할 수 있습니다.

## 6️⃣ 결론.
- `@Component`는 Spring 애플리케이션에서 자동으로 빈을 등록하는 데 사용되는 기본적인 어노테이션입니다.
    - 이를 사용하면 Spring IoC 컨테이너가 클래스 경로에서 자동으로 해당 클래스를 찾아 빈으로 관리하게 됩니다.
- 일반적인 빈 등록을 위한 용도로 사용되며, 더 구체적인 역할을 나태내기 위해 `@Service`, `@Repository`, `@Controller`와 같은 특화된 어노테이션이 존재합니다.
- `@Component`는 간단하게 빈을 관리하고자 할 때 유용하게 사용됩니다.
