---
title: 🍃[Spring] `@Configuration`이란 무엇일까요?
tags:
    - Spring
    - Framework
date: "2024-12-06"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] `@Configuration`이란 무엇일까요?
- `@Configuration`은 Spring Framework에서 사용되는 애너테이션으로, 스프링 컨테이너에 설정 클래스임을 명시하는 역할을 합니다.
    - 이 애너테이션이 붙은 클래스는 Bean 설정 메타데이터를 정의하는데 사용됩니다.
- 이를 통해 개발자는 `application.properties`나 `application.yml` 설정을 대신하여 Java 기반의 설정을 작성할 수 있는 방법을 재공합니다.
    - 그러나 `@Configuration`이 `application.properties`나 `application.yml` **파일 자체를 완전히 대체한다는 것은 아닙니다.**
        - 이 두 가지는 **서로 보완적인 역할을 합니다.**

## 1️⃣ `@Configuration`과 `application.properties/application.yml`의 관계
### 1️⃣ `application.properties/application.yml`
- 주로 환경 설정 값을 외부화하여 정의하는 데 사용됩니다.
- 데이터베이스 URL, 사용자 이름, 비밀번호, 포트와 같은 설정값을 담습니다.
- 파일 기반 설정이며, 스프링이 자동으로 읽어서 필요한 곳에 주입합니다.

### 2️⃣ `@Configuration`
- Java 코드로 명시적으로 Bean과 설정을 정의합니다.
- `@Bean` 메서드를 통해 동적인 설정 로직이나 복잡한 객체 생성 로직을 담을 수 있습니다.
- 설정값을 프로그래밍적으로 처리하거나 특정 조건에 따라 동적으로 구성해야 할 때 유용합니다.

## 2️⃣ 예제: Java 기반 설정으로 application.properties 대체하기.
### 👉 기존 application.properties 설정.
```properties
app.name=MyApplication
app.version=1.0.0
```

### 👉 Java 기반 설정으로 대체.
```java
@Configuration
public class AppConfig {
    
    @Bean
    public String appName() {
        return "MyApplication";
    }
    
    @Bean
    public String appVersion() {
        return "1.0.0";
    }
}
```
- 이렇게 하면 스프링 컨테이너에서 appName과 appVersion이라는 이름의 Bean을 사용할 수 있습니다.

## 3️⃣ `@Configuration`과 `application.properties`를 함께 사용하는 방법.
- 대부분의 경우, Java 기반 설정은 `application.properties`나 `application.yml`에 정의된 값을 읽어서 추가적으로 처리하는 방식으로 사용됩니다.

### 1️⃣ 예제: `@Configuration`에서 `application.properties` 사용
#### 1️⃣ application.properties 파일.
```properties
app.name=MyApplication
app.version=1.0.0
```

#### 2️⃣ `@Configuration` 클래스
```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {
    
    @Value("${app.name}")
    private String appName;
    
    @Value("${app.version}")
    private String appVersion;
    
    @Bean
    public AppInfo appInfo() {
        return new AppInfo(appName, appVersion);
    }
    
    public static class AppInfo {
        private final String name;
        private final String version;
        
        public AppInfo(String name, String version) {
            this.name = name;
            this.version = version;
        }
        
        public String getName() {
            return name;
        }
        
        public String getVersion() {
            return version;
        }
    }
}
```
- 이 방식에서는 `application.properties`에 정의된 값을 `@Value`로 주입받아 Java 코드에서 활용할 수 있습니다.

## 4️⃣ 언제 Java 기반 설정을 사용할까?
- **동적 로직이 필요한 경우.**
    - 특정 조건에 따라 Bean을 다르게 설정해야 하는 경우.
    - 복잡한 객체 생성 로직이 필요한 경우.
- **코드로 명시적으로 설정을 관리하려는 경우.**
    - 외부 설정 파일이 아닌, 코드에서 직접 관리하는 것이 더 적합한 경우.

## 5️⃣ 언제 application.properties를 사용할까?
- **환경별 설정을 관리할 때**
    - 개발/운영 환경에서 서로 다른 값을 쉽게 관리하려는 경우.
- **간단한 설정값**
    - 문자열, 숫자, 논리값과 같은 간단한 설정값.

## 6️⃣ 결론.
- `@Configuration`을 사용하면 Java 코드로 설정을 정의할 수 있고, 특정 경우에는 `application.properties`나 `application.yml`을 대체할 수도 있습니다.
- 그러나 일반적으로는 **Java 설정**과 application.properties/application.yml **파일을 함께 사용하여** 유연성과 관리 편의성을 모두 얻는 것이 가장 좋습니다.
