---
title: 🍃[Spring] Spring 컨테이너.
tags:
    - Spring
    - Framework
date: "2024-10-05"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Spring 컨테이너.

**Spring 컨테이너**는 Spring Framework의 핵심 구성 요소로, 애플리케이션의 **빈(Bean)** 을 생성하고 관리하는 **IoC(Inversion Of Control) 컨테이너**를 의미합니다.

Spring 컨테이너는 애플리케이션이 동작하는 동안 객체(Bean, 빈)의 생명주기를 관리하며, **의존성 주입(Dependency Injection, DI)** 을 통해 객체 간의 의존성을 자동으로 처리합니다.

## 1️⃣ Spring 컨테이너의 주요 역할.

### 1. 빈 생성 및 관리
- Spring 컨테이너는 애플리케이션 내에서 필요한 빈을 생성하고 그 생명주기를 관리합니다.
- 빈의 생성, 초기화, 의존성 주입, 소멸의 모든 과정을 컨테이너가 제어합니다.

### 2. 의존성 주입(Dependency Injection)
- 컨테이너는 빈 간의 의존성을 분석하고, 필요한 경우 의존성을 자동으로 주입합니다.
- 이를 통해 개발자는 객체를 직접 생성하거나 연결할 필요 없이, 필요한 객체를 컨테이너가 주입해줍니다.

### 3. 빈 설정 및 구성.
- Spring 컨테이너는 XML 파일, 자바 설정 클래스, 어노테이션 등을 통해 설정된 빈의 구성을 관리합니다.
- 설정 파일이나 어노테이션을 통해 각 빈이 어떤 다른 빈을 필요로 하는지 정의할 수 있습니다.

### 4. 빈의 생명주기 관리.
- 컨테이너는 빈의 생명주기(생성, 초기화, 사용, 소멸)를 제어합니다.
- 예를 들어, 애플리케이션이 시작될 때 컨테이너는 필요한 빈을 생성하고, 종료될 때 빈의 자원을 해제하는 등의 역할을 수행합니다.

### 5. 스코프 관리.
- Spring 컨테이나는 빈의 스코프를 관리합니다.
- 싱글콘 스코프(애플리케이션 전체에서 하나의 인스턴스만 존재) 또는 프로토타입 스코프(요청마다 새로운 인스턴스 생성)등의 다양한 스코프를 지원합니다.

## 2️⃣ Spring 컨테이너의 유형.

Spring 컨테이너는 다양한 유형이 있으며, 이들은 모두 기본적으로 동일한 IoC 기능을 제공하지만, 사용 목적이나 구성이 다를 수 있습니다.

### 1. `BeanFactory`
- Spring의 가장 기본적인 IoC 컨테이너입니다 
- **지연 로딩(lazy loading)** 을 사용하여 빈이 실제로 필요할 때까지 생성하지 않습니다.
- 이 방식은 리소스가 제한된 환경에서 유용하지만, 복잡한 해플리케이션에서는 거의 사용되지 않습니다.

### 2. `ApplicationContext`
- Spring 컨테이너의 보다 확장된 형태로, **즉시 로딩(eager loading)** 방식을 사용해 애플리케이션 시작시 빈을 미리 생성합니다.
- `ApplicationContext`는 `BeanFactory`의 모든 기능을 포함하여, 추가적인 기능을 제공합니다.
- 이 컨테이너는 대부분의 Spring 애플리케이션에서 사용됩니다.

### 주요 구현체.
- `ClassPathXmlApplicationContext` : XML 설정 파일을 사용해 애플리케이션 컨텍스트를 구성합니다.
- `AnnotationConfigApplicationContext` : 자바 기반 설정을 사용해 애플리케이션 컨텍스트를 구성합니다.
- `WebApplicationContext` : Spring MVC 애플리케이션에서 사용되는 컨테이너로, 웹 환경에 맞는 기능을 제공합니다.

## 3️⃣ Spring 컨테이너의 동작 과정.

### 1. 빈 정의 및 등록
- 애플리케이션에서 사용할 빈을 설정 파일(XML, 자바 클래스) 또는 어노테이션을 통해 정의합니다.
- 이 빈 정의는 Spring 컨테이너가 관리할 객체의 청사진 역할을 합니다.

### 2. 컨테이너 초기화
- 애플리케이션이 시작될 때 Spring 컨테이너가 초기화되며, 빈을 생성하고 의존성을 주입합니다.
- 이때 컨테이너는 설정에 따라 빈을 생성하고 빈 사이의 의존 관계를 설정합니다.

### 3. 빈 요청 및 사용
- 애플리케이션에서 빈이 필요할 때, 컨테이너에서 빈을 요청합니다.
- 컨테이너는 요청된 빈을 반환하며, 이 빈은 애플리케이션 내에서 사용됩니다.

### 4. 빈 소멸
- 애플리케이션이 종료되거나 빈이 더 이상 필요하지 않으면, 컨테이너는 빈의 소멸 메서드를 호출하여 빈을 적절히 정리합니다.

## 4️⃣ Spring 컨테이너의 예시

### 1. XML 설정 기반 컨테이너
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userService" class="com.example.UserService">
        <property name="userRepository" ref="userRepository"/>
    </bean>

    <bean id="userRepository" class="com.example.UserRepository"/>
</beans>
```

```java
ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
UserService userService = context.getBean(UserService.class);
```

### 2. 자바 설정 기반 컨테이너
```java
@Configuration
public class AppConfig {
    
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

```java
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
UserService userService = context.getBean(UserService.class);
```

### 3. 어노테이션 기반 설정
```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
}
```

```java
@ComponentScan(basePackages = "com.example")
@Configuration
public class AppConfig {}
```

```java
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
UserService userService = context.getBean(UserService.class);
```

## 5️⃣ 결론
Spring 컨테이너는 Spring 애플리케이션에서 객체(빈)의 생성, 관리, 의존성 주입 등을 자동으로 처리하는 핵심 인프라입니다.
이를 통해 개발자는 객체 생성 및 관리의 복잡성을 줄이고, 코드의 모듈화와 유연성을 높일 수 있습니다.
다양한 설정 방식(XML, 자바 설정, 어노테이션 등)을 통해 개발자는 컨테이너와 빈을 쉽게 구성하고 관리할 수 있습니다.
