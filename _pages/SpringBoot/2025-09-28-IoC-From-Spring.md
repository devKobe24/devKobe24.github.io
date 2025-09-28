---
title: 🏛️[SpringBoot] 스프링 부트의 핵심 개념`:` 제어의 역전(IoC) - 스프링에서의 제어의 역전
tags:
    - SpringBoot
date: "2025-09-28"
thumbnail: "/assets/img/thumbnail/springboot.jpg"
---

# 🏛️[SpringBoot] 스프링 부트의 핵심 개념`:` 제어의 역전(IoC) - 스프링에서의 제어의 역전

## 📋 목차
1. [의존성 주입의 기본](#의존성-주입의-기본)
2. [다중 빈 문제와 해결책](#다중-빈-문제와-해결책)
3. [실제 사용 예제](#실제-사용-예제)
4. [정리](#정리)

---

## 의존성 주입의 기본

Spring에서 `@Autowired` 애노테이션을 사용하면, 스프링 컨테이너가 해당 인터페이스를 구현한 객체를 자동으로 찾아서 주입해줍니다.

### 기본 구조 예제

```java
// 인터페이스
public interface CoffeeMachine {
    String brew();
}

// 구현체
@Component
public class EspressoMachine implements CoffeeMachine {
    @Override
    public String brew() {
        return "Brewing coffee with Espresso Machine";
    }
}

// 의존성 주입
@Component
public class CoffeeMaker {
    @Autowired
    private CoffeeMachine coffeeMachine;
    
    @PostConstruct
    public void makeCoffee() {
        System.out.println(coffeeMachine.brew());
    }
}
```

---

## 다중 빈 문제와 해결책

### 🚨 문제 상황

동일한 인터페이스를 구현한 클래스가 여러 개 있을 때 문제가 발생합니다:

```java
@Component
public class EspressoMachine implements CoffeeMachine {
    @Override
    public String brew() {
        return "Brewing coffee with Espresso Machine";
    }
}

@Component
public class DripCoffeeMachine implements CoffeeMachine {
    @Override
    public String brew() {
        return "Brewing coffee with Drip Coffee Machine";
    }
}
```

이 경우 다음과 같은 오류가 발생합니다:

```
*********************
APPLICATION FAILED TO START
*********************

Description:
Field coffeeMachine in com.example.demo.CoffeeMaker required a single bean
but 2 were found:
    - dripCoffeeMachine: defined in file [...\DripCoffeeMachine.class]
    - espressoMachine: defined in file [...\EspressoMachine.class]

Action:
Consider marking one of the beans as @Primary, updating the consumer to accept
multiple beans, or using @Qualifier to identify the bean that should be consumed
```

### 💡 해결 방법

#### 방법 1: `@Primary` 사용

우선순위가 높은 빈을 지정합니다:

```java
@Component
@Primary  // 기본으로 선택될 빈
public class DripCoffeeMachine implements CoffeeMachine {
    @Override
    public String brew() {
        return "Brewing coffee with Drip Coffee Machine";
    }
}

@Component
public class EspressoMachine implements CoffeeMachine {
    @Override
    public String brew() {
        return "Brewing coffee with Espresso Machine";
    }
}
```

#### 방법 2: `@Qualifier` 사용

빈에 이름을 지정하고, 주입받을 때 특정 이름을 명시합니다:

```java
// 1. 빈 등록 시 이름 지정
@Component("dripCoffeeMachine")
public class DripCoffeeMachine implements CoffeeMachine {
    @Override
    public String brew() {
        return "Brewing coffee with Drip Coffee Machine";
    }
}

@Component("espressoMachine")
public class EspressoMachine implements CoffeeMachine {
    @Override
    public String brew() {
        return "Brewing coffee with Espresso Machine";
    }
}

// 2. 주입받을 때 특정 빈 지정
@Component
public class CoffeeMaker {
    @Autowired
    @Qualifier("dripCoffeeMachine")  // 특정 빈 선택
    private CoffeeMachine coffeeMachine;
    
    @PostConstruct
    public void makeCoffee() {
        System.out.println(coffeeMachine.brew());
    }
}
```

#### 방법 3: 모든 빈을 리스트로 주입

모든 구현체를 사용하고 싶다면 `List`로 주입받습니다:

```java
@Component
public class CoffeeMaker {
    @Autowired
    private List<CoffeeMachine> coffeeMachines;  // 모든 구현체 주입
    
    @PostConstruct
    public void makeCoffee() {
        for (CoffeeMachine coffeeMachine : coffeeMachines) {
            System.out.println(coffeeMachine.brew());
        }
    }
}
```

**출력 결과:**
```
Brewing coffee with Drip Coffee Machine
Brewing coffee with Espresso Machine
```

---

## 실제 사용 예제

### 시나리오별 사용법

#### 🎯 특정 구현체만 사용하고 싶은 경우
```java
@Service
public class CafeService {
    @Autowired
    @Qualifier("premiumEspressoMachine")
    private CoffeeMachine coffeeMachine;
}
```

#### 🎯 기본 구현체를 설정하고 싶은 경우
```java
@Component
@Primary
public class DefaultCoffeeMachine implements CoffeeMachine {
    // 기본으로 사용할 구현체
}
```

#### 🎯 모든 구현체에 대해 작업을 수행하고 싶은 경우
```java
@Service
public class CoffeeTestService {
    @Autowired
    private List<CoffeeMachine> allMachines;
    
    public void testAllMachines() {
        allMachines.forEach(machine -> 
            System.out.println("Testing: " + machine.brew())
        );
    }
}
```

---

## 정리

### 🔑 핵심 포인트

1. **기본 동작**: `@Autowired`는 인터페이스 타입에 맞는 빈을 자동으로 주입
2. **다중 빈 문제**: 같은 인터페이스를 구현한 빈이 여러 개일 때 충돌 발생
3. **해결 방법**:
   - `@Primary`: 우선순위 지정
   - `@Qualifier`: 특정 빈 선택
   - `List<T>`: 모든 빈 주입

### ⚡ 베스트 프랙티스

- **명확한 빈 이름 사용**: `@Component("구체적인이름")`
- **@Primary 적절히 활용**: 기본값이 명확할 때
- **@Qualifier 적극 활용**: 특정 구현체가 필요할 때
- **인터페이스 설계**: 역할과 책임을 명확히 분리

### 🌟 Spring IoC의 핵심 철학

Spring 애플리케이션에서는 **모든 클래스가 스프링 빈으로 등록**되고, **스프링 컨테이너가 의존성을 관리**합니다. 개발자는 객체 생성과 의존성 주입을 직접 관리하지 않고, Spring이 이를 대신 처리하여 **느슨한 결합(Loose Coupling)** 과 **높은 테스트 가능성**을 제공받을 수 있습니다.
