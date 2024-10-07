---
title: 🍃[Spring] `@Qualifier` 어노테이션.
tags:
    - Spring
    - Framework
date: "2024-10-06"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] `@Qualifier` 어노테이션.

- `@Qualifier` 어노테이션은 Spring Framework에서 **빈 주입 시 모호성을 해결하기 위해 사용되는 어노테이션입니다.**
- Spring은 기본적으로 **타입을 기준으로 빈을 주입**하지만, 동일한 타입의 빈이 여러 개 존재할 경우 어느 빈을 주입할지 모호성이 발생할 수 있습니다.
- 이때 **`@Qualifier`** 어노테이션을 사용하여 **특정 빈을 명시적으로 지정할 수 있습니다.**

## 1️⃣ `@Qualifier`의 주요 기능.

### 1. 명시적 빈 선택.
- 여러 개의 동일한 타입의 빈이 존재할 때, `@Qualifier`를 사용하여 어떤 빈을 주입할지 **명시적으로 지정**할 수 있습니다.
    - 이를 통해 Spring이 주입해야 할 빈을 명확하게 구분할 수 있습니다.

### 2. 빈 이름 기반 주입.
- `@Qualifier`는 빈의 이름을 기준으로 주입할 빈을 선택합니다.
    - `@Autowired`와 함께 사용되며, 이를 통해 Spring이 어떤 빈을 주입할지 결정할 수 있습니다.

## 2️⃣ 사용 예시

### 1. 동일한 타입의 여러 빈이 있을 때.
```java
@Component
public class FirstService implements MyService {
    // FirstService 구현
}

@Component
public class SecondService implements MyService {
    // SecondService 구현
}
```

- 위 코드에서 `FirstService`와 `SecondService`가 모두 `MyService` 타입으로 정의된 빈입니다.
    - 이 경우 `MyService` 타입의 빈을 주입받으려 하면 Spring이 어느 빈을 주입해야 할지 모호성이 발생합니다.

### 2. `@Qualifier`로 특정 빈 주입하기.
```java
@Service
public class MyClient {
    
    private final MyService myService;
    
    @Autowired
    public MyClient(@Qualifier("secondService") MyService myService) {
        this.myService = myService;
    }
    
    public void execute() {
        myService.performAction();
    }
}
```

- **설명**
    - 위 코드에서 `@Qualifier("secondService")`는 `SecondService` 빈을 명시적으로 주입하도록 지정하고 있습니다.
    - 따라서 Spring은 `SecondService` 빈을 주입하게 됩니다.
    - 동일한 타입의 여러 빈이 있을 때, 이와 같이 명시적으로 선택할 수 있습니다.

## 3️⃣ 사용 상황.
- **동일한 타입의 빈이 여러 개 있을 때**
    - `Qualifier`는 여러 개의 동일한 타입의 빈이 등록되어 있을 때, 어느 빈을 주입해야 할지 명확히 지정해야 하는 경우에 사용됩니다.
- **특정 빈을 주입하고 싶을 때**
    - 일반적인 상황에서 기본 빈이 아닌, 특정한 빈을 주입하고자 할 때 사용할 수 있습니다.

### 빈 이름 지정.
- 빈 이름을 명시적으로 지정하려면, `@Component` 또는 `@Bean` 어노테이션에 이름을 지정할 수 있습니다.
```java
@Component("firstService")
public class FirstService implements MyService {
    // 구현 내용
}

@Component("secondService")
public class SecondService implements MyService {
    // 구현 내용
}
```

- 이렇게 빈의 이름을 명시적으로 설정한 후 `@Qualifier`로 해당 이름을 지정하여 주입할 수 있습니다.

## 4️⃣ `@Qualifier`와 `@Primary`의 차이
- **`@Primary`** 
    - 기본적으로 사용될 빈을 지정합니다.
    - 동일한 타입의 여러 빈이 존재할 때, `@Primary`가 지정된 빈이 우선적으로 주입됩니다.
    - 다만, 명시적으로 `@Qualifier`가 사용되면 `@Primary`는 무시됩니다.
- **`@Qualifier`**
    - 특정 빈을 명시적으로 주입할 때 사용됩니다.
    - `@Primary`가 설정된 빈이 있더라도, `@Qualifier`로 명시된 빈이 우선됩니다.

## 5️⃣ 예시: `@Primary`와 `@Qualifier` 함께 사용.
```java
@Component
@Primary
public class FirstService implements MyService {
    // FirstService 구현
}

@Component
public class SecondService implements MyService {
    // SecondService 구현
}
```

```java
@Service
public class MyClient {
    
    private final MyService myService;
    
    @Autowired
    public MyClient(@Qualifier("secondService") MyService myService) {
        this.myService = myService;
    }
    
    public void execute() {
        myService.performAction();
    }
}
```

- **설명**
    - 여기서 `FirstService`는 `@Primary`로 기본 빈으로 설정되었지만, `@Qualifier("secondService")`를 사용해 `SecondService` 빈이 명시적으로 주입됩니다.
    - `@Primary`는 기본 빈을 설정할 때 유용하고, `@Qualifier`는 특정 상황에서 특정 빈을 주입할 때 사용됩니다.

## 6️⃣ 결론
- `@Qualifier` 어노테이션은 Spring에서 **동일한 타입의 여러 빈 중 특정 빈을 명시적으로 주입**해야 할 때 사용됩니다.
- 이를 통해 빈 주입 과정에서 발생할 수 있는 모호성을 해결할 수 있으며, 개발자가 원하는 빈을 명확히 지정할 수 있습니다.
- `@Primary`와 함께 사용하여 기본 빈과 특정 빈을 관리할 수 있습니다.
