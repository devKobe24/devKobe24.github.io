---
title: 🍃[Spring] `@EnableTransactionManagement`이란 무엇일까요?
tags:
    - Spring
    - Framework
date: "2024-12-11"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] `@EnableTransactionManagement`이란 무엇일까요?
- `@EnableTransactionManagement`는 Spring Framework에서 선언적 트랜잭션 관리 기능을 활성화하는 데 사용되는 애노테이션입니다.
    - 이 애노테이션을 사용하면 Spring 애플리케이션에서 메서드 단위의 트랜잭션 처리를 구성하고 관리할 수 있습니다.

## 1️⃣ 기능.
### 1️⃣ 트랜잭션 관리 활성화.
- `@EnableTransactionManagement`는 애플리케이션 컨텍스트에서 트랜잭션 관련 빈을 생성하고 트랜잭션 관리 기능을 활성화합니다.
- `@Transactional` 애노테이션이 붙은 메서드의 트랜잭션 경계를 정의하고 자동으로 트랜잭션을 관리합니다.

### 2️⃣ 프록시 기반 트랜잭션 관리.
- Spring은 AOP(Aspect-Oriented Programming)를 사용하여 트랜잭션 기능을 제공합니다.
- `@Transactional`이 붙은 메서드를 호출하면 Spring이 자동으로 프록시를 생성하여 트랜잭션 시작, 커밋, 롤백 등을 처리합니다.

## 2️⃣ 사용 방법.
```java
@Configuration
@EnableTransactionManagement
public class AppConfig {
    
    @Bean
    public PlatformTransactionManager transactionManager(DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }
}
```
- 위의 코드에서:
    - 1. `@EnableTransactionManagement`: 트랜잭션 관리 기능을 활성화.
    - 2. `PlatformTransactionManager` : Spring이 트랜잭션을 관리하는 데 사용하는 트랜잭션 매니저를 정의.

## 3️⃣ 동작 원리.
### 1️⃣ `@Transactional` 애노테이션이 붙은 메서드를 호출하면:
- Spring은 AOP 프록시를 통해 트랜잭션 시작.
- 메서드 실행 후 정상적으로 완료되면 트랜잭션 커밋.
- 예외가 발생하면 트랜잭션 롤백.

### 2️⃣ Spring은 PlatformTransactionManager를 사용하여 데이터베이스 트랜잭션을 제어합니다.

## 4️⃣ 예제.
### 1️⃣ 트랜잭션 관리 활성화.
```java
@Configuration
@EnableTransactionManagement
public class TransactionConfig {
    
    @Bean
    public PlatformTransactionManager transactionManager(DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }
}
```

### 2️⃣ 서비스 클래스에서 트랜잭션 사용.
```java
@Service
public class UserService {
    
    @Transactional
    public void createUser(User user) {
        // 트랜잭션 안에서 실행되는 코드
        userRepository.save(user);
        
        if (user.getName().equals("error")) {
            throw new RuntimeException("Rollback test");
        }
    }
}
```
- 위 코드에서:
    - `@Transactional` : createUser 메서드를 호출할 때 트랜잭션이 시작됩니다.
    - 예외 발생 시 트랜잭션이 롤백됩니다.

## 5️⃣ 옵션.
- `@EnableTransactionManagement`에는 다음과 같은 옵션이 있습니다.

### 1️⃣ mode
- 트랜잭션 관리 모드를 설정.
- 기본값은 AdviceMode, PROXY, AOP 프록시를 사용.
```java
@EnableTransactionManagement(mode = AdviceMode.ASPECTJ)
```

### 2️⃣ proxyTargetClass
- CGLIB 기반 프록시를 생성할지 여부를 설정.
- 기본값은 false(인터페이스 기반 프록시 사용).
```java
@EnableTransactionManagement(proxyTargetClass = true)
```

## 6️⃣ 요약.
- `@EnableTransactionManagement`는 Spring에서 선언적 트랜잭션 관리 기능을 활성화하는 애노테이션입니다.
- `@Transactional` 애노테이션과 함께 사용하여 메서드 단위의 트랜잭션 처리를 간단히 구성할 수 있습니다.
- Spring AOP를 사용하여 트랜잭션 경계를 설정하고, 트랜잭션 매니저를 통해 데이터베이스 트랜잭션을 제어합니다.
