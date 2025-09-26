---
title: 🏛️[SpringBoot] 스프링 부트의 핵심 개념 - 의존성 주입
tags:
    - SpringBoot
date: "2025-09-26"
thumbnail: "/assets/img/thumbnail/springboot.jpg"
---

# 🏛️[SpringBoot] 스프링 부트의 핵심 개념 - 의존성 주입
## 📖 개요

Spring Framework의 핵심 개념 중 하나인 **의존성 주입(Dependency Injection)** 과 **제어의 역전(Inversion of Control)** 에 대해 알아봅니다. 이는 SpringBoot 애플리케이션 개발의 기초가 되는 매우 중요한 개념입니다.

---

## 🧩 의존성(Dependency)이란?

### 기본 개념
- **의존성**: 한 클래스가 동작하기 위해 다른 클래스가 필요한 관계
- **표현 방식**: "A 클래스가 B 클래스에 의존한다" 또는 "B 클래스가 A 클래스의 의존성이다"

### Java 프로그래밍에서의 의존성
```java
public class OrderService {
    private PaymentService paymentService;
    
    public OrderService() {
        this.paymentService = new PaymentService(); // OrderService가 PaymentService에 의존
    }
}
```

---

## ☕ 실습 예제: 커피 메이커 시스템

### 1단계: 기본적인 의존성 관계 구현

#### EspressoMachine 클래스
```java
public class EspressoMachine {
    public String brew() {
        return "Brewing coffee with Espresso Machine";
    }
}
```

#### CoffeeMaker 클래스
```java
public class CoffeeMaker {
    private EspressoMachine espressoMachine;
    
    public CoffeeMaker() {
        this.espressoMachine = new EspressoMachine(); // 직접 의존성 생성
    }
    
    public void makeCoffee() {
        System.out.println(espressoMachine.brew());
    }
}
```

#### 실행 코드
```java
public class Main {
    public static void main(String[] args) {
        CoffeeMaker coffeeMaker = new CoffeeMaker();
        coffeeMaker.makeCoffee();
        // 출력: Brewing coffee with Espresso Machine
    }
}
```

---

## 🚨 단단한 결합(Tight Coupling)의 문제점

### 문제 상황
새로운 드립 커피 머신을 추가하고 싶다면?

#### DripCoffeeMachine 클래스
```java
public class DripCoffeeMachine {
    public String brew() {
        return "Brewing coffee with Drip Coffee Machine";
    }
}
```

#### 기존 CoffeeMaker 클래스 수정 필요
```java
public class CoffeeMaker {
    // private EspressoMachine espressoMachine;     // 삭제
    private DripCoffeeMachine dripCoffeeMachine;    // 추가
    
    public CoffeeMaker() {
        // this.espressoMachine = new EspressoMachine();        // 삭제
        this.dripCoffeeMachine = new DripCoffeeMachine();       // 추가
    }
    
    public void makeCoffee() {
        // System.out.println(espressoMachine.brew());          // 삭제
        System.out.println(dripCoffeeMachine.brew());           // 추가
    }
}
```

### 단단한 결합의 단점
- 📝 **코드 수정 범위 확대**: 의존성 클래스 변경 시 사용하는 모든 클래스도 수정 필요
- 🔄 **유지보수 어려움**: 여러 곳에 흩어진 코드를 모두 찾아서 수정
- 🧪 **테스트 어려움**: Mock 객체 사용이 어려움
- 📈 **확장성 부족**: 새로운 구현체 추가 시 기존 코드 변경 불가피

---

## 🔗 느슨한 결합(Loose Coupling)으로의 전환

### 1단계: 인터페이스 정의
```java
public interface CoffeeMachine {
    String brew();
}
```

### 2단계: 구현체들이 인터페이스 구현
```java
public class EspressoMachine implements CoffeeMachine {
    @Override
    public String brew() {
        return "Brewing coffee with Espresso Machine";
    }
}

public class DripCoffeeMachine implements CoffeeMachine {
    @Override
    public String brew() {
        return "Brewing coffee with Drip Coffee Machine";
    }
}
```

### 3단계: CoffeeMaker 클래스 리팩토링
```java
public class CoffeeMaker {
    private CoffeeMachine coffeeMachine;
    
    // 의존성을 외부에서 주입받는 메서드
    public void setCoffeeMachine(CoffeeMachine coffeeMachine) {
        this.coffeeMachine = coffeeMachine;
    }
    
    public void makeCoffee() {
        System.out.println(coffeeMachine.brew());
    }
}
```

### 4단계: 외부에서 의존성 주입
```java
public class Main {
    public static void main(String[] args) {
        CoffeeMaker coffeeMaker = new CoffeeMaker();
        
        // 드립 커피 머신 사용
        coffeeMaker.setCoffeeMachine(new DripCoffeeMachine());
        coffeeMaker.makeCoffee();
        
        // 에스프레소 머신으로 쉽게 변경
        coffeeMaker.setCoffeeMachine(new EspressoMachine());
        coffeeMaker.makeCoffee();
    }
}
```

---

## ✨ 의존성 주입의 장점

### 🎯 **유연성 (Flexibility)**
```java
// 새로운 모카 머신 추가 - CoffeeMaker 코드 변경 없음!
public class MokaCoffeeMachine implements CoffeeMachine {
    @Override
    public String brew() {
        return "Brewing coffee with Moka Coffee Machine";
    }
}

// 사용 시
coffeeMaker.setCoffeeMachine(new MokaCoffeeMachine());
```

### 🧪 **테스트 용이성 (Testability)**
```java
// 테스트를 위한 Mock 객체 쉽게 사용 가능
public class MockCoffeeMachine implements CoffeeMachine {
    @Override
    public String brew() {
        return "Mock brewing for test";
    }
}

// 테스트 코드에서
coffeeMaker.setCoffeeMachine(new MockCoffeeMachine());
```

### 🔧 **유지보수성 (Maintainability)**
- 새로운 구현체 추가 시 기존 코드 변경 불필요
- 각 클래스의 책임이 명확하게 분리
- 인터페이스 기반 설계로 확장성 확보

---

## 🏗️ Spring에서의 의존성 주입

### Spring Container의 역할
SpringBoot에서는 **Spring Container**가 다음과 같은 역할을 담당합니다:

1. 📦 **객체 생성**: 애플리케이션에 필요한 모든 Bean 객체를 생성
2. 🔗 **의존성 연결**: 객체들 간의 의존성을 자동으로 주입
3. ♻️ **생명주기 관리**: 객체의 생성부터 소멸까지 전체 생명주기를 관리

### Spring의 의존성 주입 방법

#### 1. 생성자 주입 (권장)
```java
@Service
public class CoffeeMaker {
    private final CoffeeMachine coffeeMachine;
    
    public CoffeeMaker(CoffeeMachine coffeeMachine) {
        this.coffeeMachine = coffeeMachine;
    }
}
```

#### 2. Setter 주입
```java
@Service
public class CoffeeMaker {
    private CoffeeMachine coffeeMachine;
    
    @Autowired
    public void setCoffeeMachine(CoffeeMachine coffeeMachine) {
        this.coffeeMachine = coffeeMachine;
    }
}
```

#### 3. 필드 주입
```java
@Service
public class CoffeeMaker {
    @Autowired
    private CoffeeMachine coffeeMachine;
}
```

### Spring Bean 등록
```java
@Component
public class EspressoMachine implements CoffeeMachine {
    // 구현 내용
}

@Component
public class DripCoffeeMachine implements CoffeeMachine {
    // 구현 내용
}
```

---

## 🎯 핵심 개념 정리

### 의존성 주입 (Dependency Injection)
> 객체가 필요로 하는 의존성을 **외부에서 생성하여 주입**하는 설계 패턴

### 제어의 역전 (Inversion of Control)
> 객체의 생성과 관리 책임을 **개발자에서 프레임워크(Spring Container)로 이전**

### 장점 요약
- ✅ **결합도 감소**: 클래스 간 의존성을 인터페이스로 추상화
- ✅ **재사용성 증가**: 다양한 구현체를 쉽게 교체 가능
- ✅ **테스트 용이**: Mock 객체를 통한 단위 테스트 간편화
- ✅ **유지보수성**: 새로운 기능 추가 시 기존 코드 변경 최소화

---

## 🚀 다음 단계

의존성 주입을 이해했다면 다음 개념들을 학습해보세요:

- **Spring Bean과 Component Scan**
- **다양한 의존성 주입 애노테이션 (@Autowired, @Qualifier, @Primary)**
- **Bean Scope (Singleton, Prototype 등)**
- **관점 지향 프로그래밍 (AOP)**

---

## 💡 실무 팁

### 생성자 주입을 권장하는 이유
```java
@Service
public class OrderService {
    private final PaymentService paymentService;
    private final EmailService emailService;
    
    // final 필드로 불변성 보장
    public OrderService(PaymentService paymentService, EmailService emailService) {
        this.paymentService = paymentService;
        this.emailService = emailService;
    }
}
```

1. **불변성 보장**: final 키워드로 객체 상태 보호
2. **필수 의존성 보장**: 객체 생성 시 모든 의존성이 주입됨을 보장
3. **테스트 친화적**: 생성자를 통해 쉽게 Mock 객체 주입 가능

Spring Framework는 이러한 의존성 주입을 통해 **느슨한 결합**을 유지하면서도 **강력한 기능**을 제공하는 애플리케이션 개발을 가능하게 합니다.
