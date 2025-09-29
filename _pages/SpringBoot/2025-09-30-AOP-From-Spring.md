---
title: 🏛️[SpringBoot] 스프링 부트의 핵심 개념 - 관점 지향 프로그래밍
tags:
    - SpringBoot
date: "2025-09-30"
thumbnail: "/assets/img/thumbnail/springboot.jpg"
---

# 🏛️[SpringBoot] 스프링 부트의 핵심 개념 - 관점 지향 프로그래밍

## AOP란?

자바에서는 재사용 가능한 코드를 만들기 위해 **객체 지향 프로그래밍(OOP)**을 사용합니다. 클래스를 작성하고 객체를 생성하며, 공통 기능은 부모 클래스로 작성해 상속을 통해 재사용합니다.

하지만 자바는 수직적 상속 외에도 **수평적으로 공통 관심사를 구현하는 방법**을 제공하는데, 이것이 바로 **관점 지향 프로그래밍(AOP, Aspect Oriented Programming)**입니다.

Spring Boot는 AOP를 쉽고 편리하게 구현할 수 있도록 **Spring AOP**를 제공합니다.

---

## 실전 예제: 메서드 실행 시간 측정

### 문제 상황

몬테카를로 기법으로 원주율을 계산하는 메서드가 있다고 가정해봅시다. 이 메서드의 실행 시간을 측정하려면 다음과 같이 코드를 작성해야 합니다.

```java
// PiCalculator.java
public class PiCalculator {
    public static void main(String[] args) {
        PiCalculator pi = new PiCalculator();
        System.out.println(pi.calculate(100000000));
    }
    
    double calculate(int points) {
        long start = System.currentTimeMillis();
        int circle = 0;
        
        for (long i = 0; i < points; i++) {
            double x = Math.random() * 2 - 1;
            double y = Math.random() * 2 - 1;
            if (x * x + y * y <= 1) {
                circle++;
            }
        }
        
        long executionTime = System.currentTimeMillis() - start;
        System.out.println("executed in " + executionTime + "ms.");
        return 4.0 * circle / points;
    }
}
```

**문제점**: 실행 시간을 측정하고 싶은 메서드가 여러 개라면 매번 이런 코드를 반복해야 합니다.

### AOP를 활용한 해결책

다음과 같이 애노테이션 하나만 추가하면 자동으로 실행 시간이 측정된다면 얼마나 편할까요?

```java
@PrintExecutionTime
double calculate(int points) {
    int circle = 0;
    for (long i = 0; i < points; i++) {
        double x = Math.random() * 2 - 1;
        double y = Math.random() * 2 - 1;
        if (x * x + y * y <= 1) {
            circle++;
        }
    }
    return 4.0 * circle / points;
}
```

이것이 바로 AOP의 장점입니다!

---

## Spring AOP 설정

### 1. 의존성 추가

`build.gradle` 파일에 다음 의존성을 추가합니다.

```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-aop'
}
```

> ⚠️ Spring AOP는 Spring Initializr에서 직접 추가할 수 없으므로 수동으로 추가해야 합니다.

---

## AOP 구현하기

### 2. 커스텀 애노테이션 정의

```java
// PrintExecutionTime.java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface PrintExecutionTime {
}
```

**주요 설정**:
- `@Target(ElementType.METHOD)`: 메서드에만 적용 가능
- `@Retention(RetentionPolicy.RUNTIME)`: 런타임에 동작 (컴파일 시점이 아님)

### 3. Aspect 클래스 구현

```java
// PrintExecutionTimeAspect.java
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Component
@Aspect
public class PrintExecutionTimeAspect {
    
    @Around("@annotation(PrintExecutionTime)")
    public Object printExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object result = joinPoint.proceed();
        long executionTime = System.currentTimeMillis() - start;
        
        System.out.println("executed " + joinPoint.toShortString() + 
                           " with " + joinPoint.getArgs().length + 
                           " args in " + executionTime + "ms.");
        return result;
    }
}
```

**동작 원리**:
1. `@PrintExecutionTime`이 붙은 메서드가 호출되면
2. 실제 메서드 대신 `printExecutionTime` 메서드가 먼저 실행됨
3. `joinPoint.proceed()`로 원래 메서드를 호출
4. 전후로 추가 로직(실행 시간 측정) 수행

### 4. 애노테이션 적용

```java
// Pi.java
import org.springframework.stereotype.Component;

@Component
public class Pi {
    
    @PrintExecutionTime
    double calculate(int points) {
        int circle = 0;
        for (long i = 0; i < points; i++) {
            double x = Math.random() * 2 - 1;
            double y = Math.random() * 2 - 1;
            if (x * x + y * y <= 1) {
                circle++;
            }
        }
        return 4.0 * circle / points;
    }
}
```

### 5. 실행 예제

```java
// PiApplication.java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.stereotype.Component;

@Component
public class PiApplication implements ApplicationRunner {
    
    @Autowired
    private Pi pi;
    
    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("PI with 10,000 points = " + pi.calculate(10000));
        System.out.println("PI with 100,000 points = " + pi.calculate(100000));
        System.out.println("PI with 1,000,000 points = " + pi.calculate(1000000));
        System.out.println("PI with 10,000,000 points = " + pi.calculate(10000000));
        System.out.println("PI with 100,000,000 points = " + pi.calculate(100000000));
    }
}
```

### 실행 결과

```
executed execution(Pi.calculate(..)) with 1 args in 1ms.
PI with 10,000 points = 3.1376
executed execution(Pi.calculate(..)) with 1 args in 4ms.
PI with 100,000 points = 3.14336
executed execution(Pi.calculate(..)) with 1 args in 18ms.
PI with 1,000,000 points = 3.141108
executed execution(Pi.calculate(..)) with 1 args in 169ms.
PI with 10,000,000 points = 3.1411124
executed execution(Pi.calculate(..)) with 1 args in 1724ms.
PI with 100,000,000 points = 3.14153364
```

---

## Spring AOP 주요 애노테이션

### 1. @Before

메서드 실행 **전**에 수행할 로직을 구현합니다.

```java
@Component
@Aspect
public class PrintExecutionTimeAspect {
    
    @Before("@annotation(PrintExecutionTime)")
    public void beforePrintExecutionTime(JoinPoint joinPoint) {
        System.out.println("do something before " + joinPoint.toShortString() +
                          " with " + joinPoint.getArgs().length + " args.");
    }
}
```

### 2. @After

메서드 실행 **후**에 수행할 로직을 구현합니다 (예외 발생 여부와 무관하게 항상 실행).

```java
@Component
@Aspect
public class PrintExecutionTimeAspect {
    
    @After("@annotation(PrintExecutionTime)")
    public void afterPrintExecutionTime(JoinPoint joinPoint) {
        System.out.println("do something after " + joinPoint.toShortString() +
                          " with " + joinPoint.getArgs().length + " args.");
    }
}
```

### 3. @AfterReturning

메서드가 정상적으로 **반환값을 리턴**한 후에 수행할 로직을 구현합니다.

```java
@Component
@Aspect
public class PrintExecutionTimeAspect {
    
    @AfterReturning(
        pointcut = "@annotation(PrintExecutionTime)",
        returning = "result"
    )
    public void afterReturning(JoinPoint joinPoint, Object result) {
        System.out.println("afterReturning " + joinPoint.toShortString()
                          + " with " + joinPoint.getArgs().length
                          + " args returning " + result.toString());
    }
}
```

### 4. @AfterThrowing

메서드 실행 중 **예외가 발생**했을 때 수행할 로직을 구현합니다.

```java
@Component
@Aspect
public class PrintExecutionTimeAspect {
    
    @AfterThrowing(
        pointcut = "@annotation(PrintExecutionTime)",
        throwing = "ex"
    )
    public void afterThrowing(JoinPoint joinPoint, Exception ex) {
        System.out.println("afterThrowing " + joinPoint.toShortString()
                          + " with " + joinPoint.getArgs().length
                          + " args throwing " + ex.toString());
    }
}
```

### 5. @Around

메서드 실행 **전후 모두**를 제어할 수 있는 가장 강력한 애노테이션입니다.

---

## AOP 애노테이션 비교표

| 애노테이션 | 실행 시점 | 예외 처리 | 반환값 접근 | 메서드 실행 제어 | 주요 용도 |
|-----------|----------|---------|-----------|----------------|----------|
| **@Before** | 메서드 실행 전 | 무관 | ✗ | ✗ | 인증, 로깅, 파라미터 검증 |
| **@After** | 메서드 실행 후(항상) | 포함 | ✗ | ✗ | 리소스 정리, 공통 후처리 |
| **@AfterReturning** | 메서드 정상 종료 후 | 예외 시 미실행 | ✓ | ✗ | 반환값 로깅, 응답 후처리 |
| **@AfterThrowing** | 메서드 예외 발생 후 | 전용 | ✗ | ✗ | 예외 로깅, 예외 전환 |
| **@Around** | 메서드 실행 전/후 모두 | 포함 | ✓ | ✓ | 트랜잭션, 성능 측정, 전체 제어 |

---

## 실무에서의 AOP 활용

Spring Boot는 AOP를 기반으로 다양한 애노테이션을 제공합니다. 가장 대표적인 예가 **@Transactional**입니다.

```java
@Transactional
public void saveUser(User user) {
    userRepository.save(user);
    // 메서드 실행 전: BEGIN TRANSACTION
    // 메서드 실행 후: COMMIT (또는 예외 시 ROLLBACK)
}
```

`@Transactional` 애노테이션만 추가하면 자동으로 트랜잭션 처리가 되어 매우 편리합니다.

---

## 정리

**AOP의 핵심 장점**:
- ✅ 중복 코드 제거
- ✅ 비즈니스 로직과 부가 기능 분리
- ✅ 코드 유지보수성 향상
- ✅ 수평적 관심사 처리

Spring AOP를 활용하면 로깅, 트랜잭션, 보안, 성능 측정 등의 공통 관심사를 효율적으로 관리할 수 있습니다.
