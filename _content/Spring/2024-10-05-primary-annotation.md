---
title: 🍃[Spring] `@Primary` 어노테이션
tags:
    - Spring
    - Framework
date: "2024-10-05"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] `@Primary` 어노테이션

`@Primary` 어노테이션은 Spring Framework에서 **빈의 우선 순위를 지정할 때 사용하는 어노테이션**입니다.

만약 여러 개의 빈이 동일한 타입으로 등록되어 있을 때, 어떤 빈을 주입할지 명시적으로 지정하지 않으면 Spring은 자동으로 `@Primary`가 붙은 빈을 우선적으로 주입하게 됩니다.

이를 통해 다수의 구현체가 있을 경우 기본적으로 사용될 빈을 설정할 수 있습니다.

## 1️⃣ `@Primary`의 동작 방식.
- Spring 은 같은 타입의 빈이 여러 개 존재할 때 의존성 주입 시 어느 빈을 사용할지 결정해야 합니다.
- 기본적으로 타입에 맞는 빈을 자동으로 주입하려 하지만, 같은 타입의 빈이 두 개 이상 등록되어 있다면 모호성이 발생할 수 있습니다.
- 이때 `@Primary`를 사용하면 해당 빈을 기본적으로 주입할 빈으로 설정할 수 있습니다.

### 예시
```java
@Component
public class FirstService implement MyService {
    // FirstService 구현
}

@Component
@Primary
public class SecondService implements MyService {
    // SecondService 구현
}
```

- 위 예시에서 `FirstService`와 `SecondService`가 모두 `MyService` 타입의 빈으로 등록되어 있습니다.
- `SecondService`에는 `@Primary` 어노테이션이 붙어 있으므로, `MyService` 타입의 빈이 필요할 때 Spring은 자동으로 `SecondService`를 주입하게 됩니다.

```java
@Service
public class MyClient {
    private final MyService myService;
    
    @Autowired
    public MyClient(MyService myService) {
        this.myService = myService;
    }
    
    // MyClient는 SecondService 빈을 주입받게 됩니다.
}
```

## 2️⃣ `@Primary` 와 `@Qualifier`의 차이.
- `@Priamry`는 기본 빈을 지정할 때 사용되지만, 더 구체적인 빈을 주입해야 할 때는 `@Qualifier` 어노테이션을 사용할 수 있습니다.
- `@Qualifier`는 특정 이름을 가진 빈을 명시적으로 지정하여 주입할 수 있도록 도와줍니다.

### `@Qualifier` 예시
```java
@Component
@Qualifier("firstService")
public class FirstService implement MyService {
    // FirstService 구현
}

@Component
@Qualifier("secondService")
@Primary
public class SecondService implement MyService {
    // SecondService 구현
}
```

```java
@Service
public class MyClient {
    
    private final MyService myService;
    
    @Autowired
    public MyClient(@Qualifier("firstService") MyService myService) {
        this.myService = myService;
    }
    
    // MyClient는 FirstService 빈을 주입받게 됩니다.
}
```

- 이 경우, `@Primary` 가 `SecondService`에 지정되어 있어도 `@Qualifier("firstService")`가 지정된 경우에는 `FirstService`가 주입됩니다.

## 3️⃣ 결론.
- `@Primary`는 **동일한 타입의 빈이 여러 개 등록된 경우, 기본적으로 사용될 빈을 지정하는 어노테이션**입니다.
- 이 어노테이션을 사용하면 같은 타입의 빈이 여러 개 있을 때, 명시적으로 지정하지 않으면 Spring은 `@Primary`가 붙은 빈을 우선적으로 선택해 주입합니다.
- 보다 구체적으로 어떤 빈을 주입할지 명시할 필요하 있을 때는 `@Qualifier`를 함께 사용할 수 있습니다.
