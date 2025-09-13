---
title: 🍃[Spring] Spring에서 빈(Bean)을 주입받는 방법들.
tags:
    - Spring
    - Framework
date: "2024-10-06"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Spring에서 빈(Bean)을 주입받는 방법들.

- Spring에서 **빈(Bean)** 을 주입받는 방법에는 여러 가지가 있으며, 주로 **의존성 주입(Dependency Injection, DI)** 이라는 개념을 통해 이루어집니다.
- Spring IoC 컨테이너는 객체 간의 의존성을 관리하고, 필요한 곳에 자동으로 빈을 주입합니다.
- 빈을 주입받는 방법에는 **생성자 주입, 세터 주입, 필드 주입**이 있습니다.

> 🙋‍♂️ [의존성(Dependency)](https://www.devkobe24.com/Backend/Spring/2024-10-04-Dependency.html)
> 🙋‍♂️ [Spring 컨테이너를 사용하는 이유](https://www.devkobe24.com/Backend/Spring/2024-10-05-why-use-spring-containers.html)
> 🙋‍♂️ [Spring 컨테이너](https://www.devkobe24.com/Backend/Spring/2024-10-05-Spring-container.html)
> 🙋‍♂️ [Spring 빈(Bean)](https://www.devkobe24.com/Backend/Spring/2024-10-04-bean.html)

## 1️⃣ 생성자 주입(Constructor Injection)
- 생성자 주입은 의존성을 주입할 때 **생성자를 통해** 빈을 주입하는 방식입니다.
- **가장 권장되는 방식 중 하나**로, **의존성을 강제하고 불변성을 보장**할 수 있습니다.
- 또한, **테스트하기 용이한 방식**입니다.

### 예시
```java
@Service
public class UserService {
    
    private final UserRepository userRepository;
    
    @Autowired // Spring 4.3+ 에서는 생략 가능
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    public void process() {
        userRepository.save();
    }
}
```

### 설명.
- `UserService`는 `UserRepository` 빈을 생성자를 통해 주입받습니다.
- `@Autowired`를 통해 스프링이 `UserRepository` 빈을 자동으로 주입하게 됩니다.
- `@Autowired`는 Spring 4.3 이후 생성자 주입에서는 생략 가능하지만, 명시적으로 적는 경우도 있습니다.

## 2️⃣ 세터 주입(Setter Injection)
- 세터 주입은 **세터 메서드**를 통해 빈을 주입하는 방식입니다.
- 선택적인 의존성을 주입할 때 유용하며, 주입받은 빈을 변경할 수 있는 유연성을 제공합니다.

### 예시
```java
@Service
public class UserService {
    
    private UserRepository userRepository;
    
    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRespository = userRepository;
    }
    
    public void process() {
        userRepository.sava();
    }
}
```

### 설명.
- `UserSevice`는 `setUserRepository`라는 세터 메서드를 통해 `UserRepository` 빈을 주입받습니다.
    - `@Autowired` 어노테이션을 통해 스프링이 적절한 빈을 주입하게 됩니다.

## 3️⃣ 필드 주입(Field Injection)
- 필드 주입은 **직접 필드에 `@Autowired` 어노테이션을 붙여서 빈을 주입하는 방식**입니다.
- 가장 간단한 방식이지만, **테스트하기 어려운 구조를 만들 수 있고, 주입된 필드가 'final'로 설정되지 않기 때문에 '불변성'이 보장되지 않습니다.**
- 일반적으로는 **지양하는 방식**입니다.

### 예시
```java
@Servicee
public class UserService {
    
    @Autowired
    private UserRepository userRepository;
    
    public void process() {
        userRepository.save();
    }
}
```

### 설명.
- `UserService`는 `UserRepository` 빈을 필드에 직접 주입받습니다.
- 필드 주입 방식은 코드가 간결하지만, 테스트나 유지보수 측면에서 불리할 수 있습니다.

## 4️⃣ 각 주입 방식의 비교.

### 1. 생성자 주입(Constructor Injection)
- **장점**
    - 의존성이 필수적임을 강제할 수 있고, 불변성을 보장하며, 테스트하기 용이합니다.
    - 의존성이 주입되지 않으면 컴파일 타임에 오류를 발견할 수 있습니다.
- **단점**
    - 클래스가 많은 의존성을 가질 경우, 생성 인자가 많아질 수 있습니다.

### 2. 세터 주입(Setter Injection)
- **장점**
    - 선택적인 의존성 주입이 가능하며, 객체 생성 후에 주입할 수 있어 유연성을 제공합니다.
- **단점**
    - 의존성이 주입되지 않은 상태로 사용될 위험이 존재하며, 객체의 상태가 변경될 수 있습니다.

### 3. 필드 주입(Field Injection)
- **장점**
    - 코드가 간결하고 가장 쉽습니다.
- **단점**
    - 테스트하기 어렵고, 의존성을 강제하지 않으며, 리플렉션을 사용하기 때문에 불변성이 보장되지 않습니다.
    - 또한, 필드에 접근하는 방식이기 때문에 SRP(Single Responsibility Principle)를 위반할 가능성이 높습니다.

> 🙋‍♂️ [SOLID 원칙](https://www.devkobe24.com/CS/2024/2024-10-03-SOLID.html)

## 5️⃣ 결론
- **생성자 주입(Constructor Injection) :** **생성자 주입(Constructor Injection)** 은 의존성 강제, 불변성 보장, 테스트 용이성 측면에서 **가장 권장되는 방식**입니다.
- **세터 주입(Setter Injection) :** 선택적 의존성을 주입할 때 유용하지만, 세터 메서드가 공용으로 노출된다는 단점이 있습니다.
- **필드 주입(Field Injection) :** 가장 간단한 방식이지만, 테스트가 어렵고 불변성을 보장하지 않기 때문에 지양하는 방식입니다.
