---
title: 🍃[Spring] 의존성(Dependency).
tags:
    - Spring
    - Framework
date: "2024-10-04"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 의존성(Dependency).

Java 백엔드 애플리케이션에서 **의존성(Dependency)** 이란 한 클래스 또는 모듈이 다른 클래스 또는 모듈의 기능을 필요로 하거나, 그 존재에 따라 동작하는 관계를 의미합니다.

간단히 말해, 의존성은 **어떤 클래스가 다른 클래스에 의존하여 해당 클래스의 메서드나 기능을 호출하고 사용하는 것을 말합니다.**

의존성은 객체 지향 프로그래밍에서 자연스럽게 발생하는 관계로, 특정 객체를 필요로 할 때 발생합니다.

예를 들어, 서비스 클래스가 데이터베이스와 상호작용하기 위해 리포지토리 클래스에 의존하거나, 컨트롤러가 비즈니스 로직을 수행하기 위해 서비스 클래스에 의존하는 경우가 해당됩니다.

## 1️⃣ 의존성의 예시

### 간단한 의존성 예

<img src = "https://github.com/devKobe24/images2/blob/main/springboot/dependency.png?raw=true">

```java
public class UserService {
    
    private UserRepository userRepository;
    
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    public User getUserById(Long id) {
        return userRepository.findById(id).orElse(null);
    }
}
```

- `UserService` 클래스는 `UserRepository` 클래스에 의존합니다.
- `UserService`는 비즈니스 로직을 처리하기 위해 `UserRepository`의 메서드를 호출합니다.
- 만약 `UserRepository`가 없으면 `UserService`는 정상적으로 작동할 수 없습니다.
- 따라서, 이 두 클래스 간에는 의존성이 존재합니다.

## 2️⃣ 의존성 관리의 중요성.

의존성(Dependency)은 자연스럽게 발생하지만, 의존성을 관리하지 않으면 애플리케이션이 **결합도**가 높아지고, 변경에 취약해지며, 유지보수가 어려워질 수 있습니다.

따라서 의존성을 적절히 관리하는 것이 중요합니다.

의존성 관리의 좋은 방법으로는 **의존성 주입(Dependency Injection, DI)** 이 있습니다.

## 3️⃣ 의존성 주입(Dependency Injection, DI)

- 의존성 주입(Dependency Injection, DI)은 객체 간의 의존성을 외부에서 주입해주는 디자인 패턴으로, 의존성 주입을 통해 클래스는 자신이 사용할 객체를 직접 생성하지 않고, 외부에서 생성된 객체를 주입받아 사용하게 됩니다.
- 이를 통해 클래스 간의 결합도를 낮추고, 유연성과 테스트 가능성을 높일 수 있습니다.

### 의존성 주입 예시(Spring Framework)
- Spring Framework에서 의존성 주입은 매우 흔하게 사용됩니다.
- Spring은 객체 간의 의존성을 관리하고, 필요한 객체를 자동으로 주입해 줍니다.

```java
@Service
public class UserService {
    
    private final UserRepository userRepository;
    
    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    public User getUserById(Long id) {
        return userRepository.findById(id).orElse(null);
    }
}
```

- `@Autowired`
    - Spring이 `UserRepository`의 구현체를 자동으로 주입해줍니다.
    - 개발자는 `UserService`가 `UserRepository`에 의존한다는 사실만 명시하면 되고, `UserRepository` 객체의 생성을 직접적으로 관리할 필요가 없습니다.

## 4️⃣ 의존성의 종류.

### 1. 강한 의존성 (Tight Coupling)
- 클래스 A가 클래스 B의 구체적인 구현에 의존할 때 발생합니다.
- 이 경우 클래스 B가 변경되면 클래스 A도 수정이 필요할 수 있습니다.
- 예를 들어, `new` 키워드를 사용해 직접 객체를 생성하면 강한 의존성이 발생합니다.

### 2. 약한 의존성(Loose Coupling)
- 클래스 A가 클래스 B의 구체적인 구현이 아닌, 인터페이스나 추상 클래스에 의존할 때 발생합니다.
- 이는 결합도를 낮추어 클래스 간의 변경에 더 유연하게 대처할 수 있게 합니다.
- 의존성 주입을 통해 약한 의존성을 실현할 수 있습니다.

## 5️⃣ 의존성 관리의 이점.

### 1. 유지보수성 향상.
- 의존성이 적절히 관리되면, 코드가 더 모듈화되고 변경 사항이 발생할 때 수정해야 할 부분이 줄어듭니다.
- 예를 들어, 특정 클래스의 구현을 변경하더라도 의존성 주입을 통해 인터페이스만 유지하면 다른 클래스에는 영향을 주지 않습니다.

### 2. 테스트 가능성.
- 의존성을 외부에서 주입받으면, 테스트 시에 Mock 객체를 주입하여 쉽게 단위 테스트를 수행할 수 있습니다.
- 이를 통해 더 작은 단위 테스트가 가능해지고, 테스트가 독립적으로 수행될 수 있습니다.

### 3. 재사용성 향상.
- 클래스가 다른 클래스에 강하게 의존하지 않으면, 해당 클래스를 다른 곳에서도 재사용하기 쉽습니다.
- 인터페이스를 사용하고, 의존성 주입을 통해 다양한 구현체를 주입받아 사용할 수 있기 때문입니다.

## 6️⃣ 결론.
Java 백엔드 애플리케이션에서 의존성은 클래스 간의 관계를 의미하며, 이를 잘 관리하는 것이 중요합니다.
**의존성 주입**을 사용하면 결합도를 낮추고 유연성을 높일 수 있으며, 코드의 유지보수성과 테스트 가능성을 크게 향상 시킬 수 있습니다.
Spring과 같은 프레임워크에서는 이러한 의존성 관리와 주입을 매우 쉽게 처리할 수 있도록 지원하고 있습니다.
