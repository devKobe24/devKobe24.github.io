---
title: 🍃[Spring] slf4j와 logback.
tags:
    - Spring
    - Framework
date: "2024-08-22"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] slf4j와 logback.

## 1️⃣ slf4j

- **`'SLF4J(Simple Logging Facade for Java)'`** 는 Java 애플리케이션에서 로그 기록을 쉽게 관리하고 다른 로깅 프레임워크와 통합할 수 있도록 도와주는 로깅 인터페이스입니다.
- **`'SLF4J'`** 는 다양한 로깅 프레임워크(e.g, Log4j, Logback, java.util.logging 등)에 대해 공통된 인터페이스를 제공하여 개발자가 특정 로깅 프레임워크에 종속되지 않고 유연하게 로그를 관리할 수 있도록 합니다.

### 1️⃣ slf4j의 주요 기능.
- 1. **로깅 프레임워크와의 추상화**
    - slf4j는 여러 로깅 프레임워크에 종속되지 않게 합니다.
        - 예를 들어, 코드에서 slf4j 인터페이스를 사용하면 나중에 로깅 프레임워크를 쉽게 교체할 수 있습니다.
- 2. **로깅 성능 최적화**
    - slf4j는 문자열 병합에 따른 성능 문제를 피할 수 있도록 지원합니다.
        - 예를 들어, slf4j는 로그 메시지의 문자열 결합을 지연시켜, 로그가 실제로 기록될 때만 결합이 발생하도록 합니다.
- 3. **API 일관성**
    - slf4j를 사용하면 로깅을 위한 일관된 API를 제공받을 수 있으며, 이를 통해 로깅을 표준화할 수 있습니다.

### 2️⃣ 사용 방법.
- slf4j를 사용하기 위해서는, 우선 slf4j 인터페이스와 이를 구현한 로깅 프레임워크(예: Logback)를 프로젝트에 포함시켜야 합니다.
    - 코드는 일반적으로 아래와 같이 사용됩니다.
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class MyClass {
    // Logger 생성
    private static final Logger logger = LoggerFactory.getLogger(MyClass.class);
    
    public void doSomthing() {
        // 로그 메시지 기록
        logger.info("This is an info message");
        logger.debug("This is a debug message");
    }
}
```
- 이 코드는 **`'slf4j'`** 를 이용해 로그를 기록하는 예로, 로깅 메시지는 설정된 로깅 프레임워크를 통해 출력됩니다.

### ✏️ 요약.
- slf4j는 Java 애플리케이션에서 로깅 프레임워크 간의 추상화 레이어를 제공하며, 코드가 특정 로깅 프레임워크에 종속되지 않도록 합니다.
    - 이를 통해 유연한 로깅 관리가 가능해집니다.

## 2️⃣ logback
- **`'logback'`** 은 Java 애플리케이션에서 사용되는 고성능 로깅 프레임워크로, slf4j의 권장 구현체 중 하나입니다.
- **`'logback'`** 은 slf4j를 통해 접근할 수 있으며, 뛰어난 성능과 유연한 설정, 다양한 기능을 제공하는 것이 특징입니다.

### 1️⃣ logback의 주요 구성 요소.
- 1. **Logback Classic**
    - slf4j와 직접 통합되는 logback의 핵심 모듈입니다.
    - **`'Logback Classic'`** 은 Java 애플리케이션에서 로깅 기능을 수행하며, 다양한 로그 레벨(INFO, DEBUG, WARN, ERROR 등)을 지원합니다.
- 2. **Logback Core**
    - Logback Classic과 Logback Access(웹 애플리케이션용)를 기반으로 하는 일반적인 로깅 기능을 제공합니다.
    - **`'Logback Core'`** 는 Appender, Layout, Filter 등과 같은 기본 구성 요소를 포함합니다.
- 3. **Logback Access**
    - 웹 애플리케이션에서 HTTP 요청과 응답을 로깅할 수 있도록 지원하는 모듈입니다.
        - 주로 Java Servlet 환경에서 사용됩니다.

### 3️⃣ logback의 특징.
- 1. **높은 성능**
    - **`'logback'`** 은 빠른 로깅 성능을 제공하며, 특히 대규모 애플리케이션에서 효과적입니다.
- 2. **유연한 구성**
    - **`'logback'`** 은 XML 또는 Groovy 스크립트로 로깅 설정을 구성할 수 있습니다.
        - 이를 통해 다양한 조건에 따라 로깅 동작을 세밀하게 제어할 수 있습니다.
- 3. **조건부 로깅**
    - **`'logback'`** 은 특정 조건에서만 로깅을 수행하도록 설정할 수 있어, 불필요한 로그 기록을 줄이고 성능을 최적화할 수 있습니다.
- 4. **이전 로그 프레임워크와의 호환성**
    - **`'logback'`** 은 기존의 **`'Log4j'`** 설정 파일을 사용할 수 있는 기능을 제공하여, 기존 **`'Log4j'`** 사용자가 쉽게 **`'logback'`** 으로 전환할 수 있도록 돕습니다.
- 5. **다양한 출력 형식**
    - **`'logback'`** 은 콘솔, 파일, 원격 서버, 데이터베이스 등 다양한 출력 대상으로 로그를 기록할 수 있으며, 출력 형식을 자유롭게 정의할 수 있습니다.

### 4️⃣ logback 사용 예제.
```xml
<configuration>
    <!-- 콘솔에 로그를 출력하는 Appender -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>
    <!-- 파일에 로그를 기록하는 Appender -->
    <appender name="FILE" class="ch.qos.logback.core.FileAppender">
        <file>mylog.log</file>
        <append>true</append>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>
    <!-- 루트 로거 설정 -->
    <root level="debug">
        <appender-ref ref="STDOUT" />
        <appender-red red="FILE" />
    </root>
</configuration>
```

- 이 예시는 콘솔과 파일에 로그를 출력하도록 설정하는 간단한 예시입니다.
    - logback은 이외에도 복잡한 요구 사항을 충족할 수 있는 다양한 기능을 제공하고 있습니다.

### ✏️ 요약.
- logback은 Java 애플리케이션에서 사용되는 고성능 로깅 프레임워크로, slf4j와 함께 사용됩니다.
- logback은 유연한 설정과 높은 성능, 다양한 기능이 있습니다.
