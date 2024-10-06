---
title: 🍃[Spring] 언제 `@Component`와 `@Bean`을 함께 사용할까?
tags:
    - Spring
    - Framework
date: "2024-10-06"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 언제 `@Component`와 `@Bean`을 함께 사용할까?

- `@Component`와 `@Bean`은 둘 다 Spring에서 **빈(Bean)** 을 정의하고, Spring 컨테이너에 등록하기 위한 어노테이션이지만, 그 사용 목적과 방식에는 차이가 있습니다.
- 이 두 어노테이션을 함께 사용하는 경우는 흔하지 않지만, 특정 상황에서는 함께 사용하여 필요한 기능을 구현할 수 있습니다.

## 1️⃣ 각 어노테이션의 차이점.

### 1. `@Component`
- **클래스 레벨**에서 사용됩니다.
- 주로 **자동 감지(자동 스캔)** 를 통해 빈을 등록하는 데 사용됩니다.
    - 즉, Spring은 특정 패키지 내에서 `@Component`가 붙은 클래스를 찾아 자동으로 빈을 등록합니다.
- `@Service`, `@Repository`, `@Controller` 등은 `@Component` 의 특수화된 형태입니다.

### 2. `@Bean`
- **메서드 레벨**에서 사용됩니다.
- 수동으로 빈을 등록할 때 사용되며, 자바 설정 클래스(`@Configuration`) 내에서 **메서드가 반환하는 객체를** 빈으로 등록합니다.
- 보통 외부 라이브러리에세 제공하는 클래스를 빈으로 등록할 때나, 클래스의 인스턴스를 특정 방식으로 초기화하거나 설정해야 할 때 사용됩니다.

## 2️⃣ 함께 사용하는 상황
- `@Component`와 `@Bean`을 **함께 사용하는** 이유는 매우 특정한 경우에 발생하며, 이러한 상황은 두 가지의 요구사항이 충돌할 때 나타납니다.
- 다음과 같은 경우에 **`@Component`** 와 **`@Bean`** 을 함께 사용할 수 있습니다.

### 1. 자동 스캔을 통해 등록되지 않는 클래스의 인스턴스를 빈으로 등록해야 할 때
- 외부 라이브러리나 Spring의 자동 스캔 대상이 아닌 클래스를 빈으로 등록해야 할 때 `@Bean`을 사용합니다.
- 하지만 이 경우에도 애플리케이션의 일부가 `@Component`로 등록된 다른 빈을 사용하고 있다면, 두 방법을 함께 사용할 수 있습니다.
- 예를 들어, 외부 라이브러이에서 제공하는 `DataSource` 객체를 수동으로 빈으로 등록해야 할 때
```java
@Configuration
public class DataSourceConfig {
    
    @Bean
    public DataSource dataSource() {
        return new HikariDataSource(); // 외부 라이브러리로부터 얻은 빈 등록
    }
}
```
- 여기서 `DataSource`는 외부 라이브러리에서 제공되므로 자동 스캔되지 않습니다.
- 따라서 `@Bean`을 사용하여 빈으로 등록합니다.
- 동시에, 이 `DataSource` 빈을 사용하는 애플리케이션의 서비스 클래스는 `@Component`로 등록될 수 있습니다.

```java
@Component
public class MyService {
    
    private final DataSource dataSource;
    
    @Autowired
    public MyService(DataSource dataSource) {
        this.dataSource = dataSource;
    }
    
    // 서비스 로직
}
```
- 이 예시에서 `@Component`와 `@Bean`을 함께 사용하여 자동 스캔된 빈(`MyService`)이 수동으로 등록된 빈(`DataSource`)을 의존성 주입받아 사용할 수 있습니다.

### 2. 동일한 클래스에 대해 서로 다른 초기화 방벙을 사용해야 할 때
- 경우에 따라 **동일한 클래스**라도 서로 다른 초기화 방식이나 설정을 통해 다른 빈을 만들고 싶을 수 있습니다.
- 이럴 때 `@Component`로 기본 빈을 자동 등록하고, 특정 상황에 맞게 다르게 초기화된 인스턴스를 `@Bean`으로 등록할 수 있습니다.
- 예를 들어, 기본적으로 `@Component`로 등록된 빈과는 다른 설정을 가진 또는 다른 빈이 필요할 때
```java
@Component
public class MyComponent {
    // 기본 설정을 가진 빈
}

@Configuration
public class MyConfig {
    
    @Bean
    public MyComponent customComponent() {
        MyComponent component = new MyComponent();
        // 커스텀 초기화 로직
        return component;
    }
}
```
- 여기서 `MyComponent` 클래스는 기본적으로 `@Component`로 빈이 등록되지만, `customComponent()` 메서드를 통해 다른 설정을 가진 인스턴스를 추가로 등록할 수 있습니다.

## 3️⃣ 언제 함께 사용하지 않아야 하나?

일반적으로는 `@Component`와 `@Bean`은 **함께 사용되지 않는 경우가 대부분**입니다.
보통은 두 가지 방법 중 하나를 선택하여 사용합니다.

- **자동 빈 등록이 가능할 때**
    - 클래스가 애플리케이션의 일부로 직접 개발된 경우 `@Component`나 특수화 어노테이션(`@Service`, `@Repository`, `@Controller`)을 사용해 자동으로 빈을 등록합니다.
- **수동 빈 등록이 필요할 때**
    - 외부 라이브러리나 자동으로 관리되지 않은 클래스를 빈으로 등록하거나, 빈의 생성과정을 수동으로 제어하고자 할 때 `@Bean`을 사용합니다.

따라서, 이 두 어노테이션을 함께 사용할 필요는 거의 없지만, **자동 빈 등록**과 **수동 빈 등록**이 공존해야 하는 특정 상황에서 필요에 따라 함께 사용할 수 있습니다.

## 4️⃣ 결론
- `@Component`와 `@Bean`은 각각 **자동 빈 등록**과 **수동 빈 등록**을 위한 어노테이션으로, 서로 다른 목적을 위해 사용됩니다.
- 이 둘을 함께 사용하는 경우는 드물지만, 외부 라이브러리나 자동으로 스캔되지 않는 객체를 빈으로 등록하고 싶거나, 같은 클래스에 대해 서로 다른 초기화 방법을 적용해야 하는 특수한 경우에 함께 사용할 수 있습니다.
