---
title: 🍃[Spring] 언제 `@Configuration`와 `@Bean`을 함께 사용할까?
tags:
    - Spring
    - Framework
date: "2024-10-06"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 언제 `@Configuration`와 `@Bean`을 함께 사용할까?

- `@Configuration`과 `@Bean`을 함께 사용하는 경우는 **자바 기반의 설정 클래스** 를 정의할 때입니다.
- 이 방식은 **수동으로 빈을 정의**하고 **설정 과정**을 세밀하게 제어하고자 할 때 사용됩니다.
- 특히 외부라이브러리나 프레임워크에서 제공하는 객체들을 빈으로 등록하거나, 빈의 생성과 초기화 과정을 커스터마이징해야 할 때 유용합니다.

## 1️⃣ `@Configuration`과 `@Bean`을 함께 사용하는 주요 이유.

### 1. 외부 라이브러리 클래스의 빈 등록.
- `@Configuration`과 `@Bean`을 사용하면 외부 라이브러리의 클래스나, Spring이 직접 관리하지 않는 객체들을 빈으로 등록할 수 있습니다.
- Spring의 자동 스캔 기능은 개발자가 작성한 클래스만 대상으로 하기 때문에, 외부 라이브러리에서 제공하는 객체는 수동으로 빈으로 등록해야 합니다.

### 2. 커스텀 빈 초기화 및 설정.
- `@Bean` 어노테이션을 사용하면 빈이 생성되는 방식을 커스터마이징할 수 있습니다.
- 생성자 파라미터, 초기화 과정, 의존성 주입 방식 등을 세부적으로 설정할 수 있으며, 이 설정을 Java 코드로 작성함으로써 XML 기반 설정을 대체할 수 있습니다.

### 3. 복잡한 빈 생성 로직 처리.
- 빈 생성 과정이 복잡하거나 여러 의존성을 필요로 할 경우, `@Configuration` 클래스에서 이를 제어할 수 있습니다.
- 빈 간의 의존성, 특정 상황에 따른 빈 생성 로직 등을 자바 코드로 명확하게 관리할 수 있습니다.

### 4. XML 설정의 대체.
- Spring은 과거에 XML 기반 설정을 주로 사용했지만, `@Configuration`과 `@Bean`을 사용하면 이러한 설정을 자바 코드로 관리할 수 있습니다.
- 자바 기반 설정은 타입 안정성을 제공하며, 코드와 설정이 하나의 파일 내에 통합되어 유지보수하기 쉬워집니다.

## 2️⃣ 예시

### 1. 외부 라이브러리 빈 등록
- 예를 들어, 외부 라이브러리의 데이터베이스 커넥션 풀(`HikariDataSource`)을 빈으로 등록하고, 이를 커스터마이징하는 경우입니다.

```java
@Configuration
public class DataSourceConfig {
    
    @Bean
    public DataSource dataSource() {
        HikariDataSource dataSource = new HikariDataSource();
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/mydb");
        dataSource.setUsername("root");
        dataSource.setPassword("password");
        dataSource.setMaximumPoolSize(10);
        return dataSource;
    }
}
```

- `@Configuration`
    - 이 클래스가 Spring의 설정 파일로 사용되며, 빈을 정의하는 클래스임을 나타냅니다.
- `@Bean`
    - 이 메서드가 반환하는 `HikariDataSource` 객체는 스프링 컨테이너에 빈으로 등록됩니다.
    - 외부 라이브러리 객체이므로 자동 스캔이 불가능하며, 이를 수동으로 등록하는 방식입니다.

### 2. 복잡한 빈 초기화
- 다음은 서비스 빈이 여러 가지 의존성을 필요로 하고, 빈 초기화 과정에서 특정 설정이 필요한 경우입니다.

```java
@Configuration
public class AppConfig {
    
    @Bean
    public UserService userService(UserRepository userRepository, NotificationService notificationService) {
        UserService userService = new UserService(userRepository);
        userService.setNotificationService(notificationService); // 추가 설정
        return userService;
    }
    
    @Bean
    public UserRepository userRepository() {
        return new UserRepository();
    }
    
    @Bean
    public NotificationService notificationService() {
        return new EmailNotificationService();
    }
}
```
- 이 예에서는 `UserService`가 여러 의존성을 필요로 하고, 특정 추가 설정이 필요한 상황을 처리합니다.
    - `Configuration` : 애플리케이션의 설정 클래스임을 명시합니다.
    - `@Bean` : `UserService`, `UserRepository`, `NotificationService`를 빈으로 등록하고, 의존성을 수동으로 주입하고 추가 설정을 할 수 있습니다.

### 3. 빈 간 의존성 관리.
- `@Configuration` 클래스에서 빈 간의 의존성을 명확하게 관리할 수 있습니다.
- 다음은 여러 빈 간의 의존성을 처리하는 예시입니다.
```java
@Configuration
public class ServiceConfig {
    
    @Bean
    public UserService userService() {
        return new UserService(userRepository());
    }
    
    @Bean
    public UserRepository userRepository() {
        return new UserRepository();
    }
}
```

- 여기서 `UserService`는 `UserRepository`에 의존하며, `@Bean` 메서드를 통해 `UserRepository` 빈을 참조합니다.
    - 스프링은 이러한 의존성을 자동으로 해결하여 빈을 등록합니다.

## 3️⃣ 언제 `@Configuration`과 `@Bean`을 사용해야 할까?

### 1. 외부 라이브러리 또는 스프링이 자동으로 관리하지 않는 클래스 등록
- 외부 라이브러리의 클래스나 다른 프레임워크에서 제공하는 객체들을 빈으로 등록해야 할 때 `@Configuration`과 `@Bean`을 함께 사용합니다.

### 2. 복잡한 빈 생성 로직이 필요할 때
- 빈 생성 시 의존성 주입 외에 추가적인 설정이 필요한 경우(특정 메서드 호출, 객체 초기화, 추가 설정 등) `@Bean`을 사용하여 수동으로 빈을 생성하고, 이를 커스터마이징할 수 있습니다.

### 3. 빈 간의 명시적 의존성 관리
- 서로 의존하는 빈들이 있을 때, `@Configuration` 클래스에서 빈의 생성 순서와 의존성을 명시적으로 관리할 수 있습니다.

### 4. 유연한 설정이 필요할 때
- 애플리케이션 설정을 자바 코드로 관리하면서, 조건부 빈 생성, 프로파일 기반 빈 관리 등과 같이 더 복잡하고 유연한 설정이 필요할 때 유용합니다.

## 4️⃣ 결론
- `@Configuration`과 `@Bean`은 스프링 애플리케이션에서 **빈을 수동으로 등록하고 설정하는 방식**으로 사용됩니다.
- 이 두 어노테이션을 함께 사용함으로써 스프링은 자바 기반으로 빈을 생성하고 관리할 수 있게 되며, 외부 라이브러리나 스프링이 자동으로 관리하지 않는 객체들도 빈으로 등록할 수 있습니다.
- 이러한 방식은 특히 **더 복잡한 빈 생성 로직**을 필요로 하거나 **외부 리소스와의 통합**이 필요할 때 유용합니다.
