---
title: 🛠️[개발 도구 및 환경] `org.springframework.boot` 플러그인의 역할?
tags:
    - Development tools
    - Enviroments
date: "2024-12-11"
thumbnail: "/assets/img/thumbnail/tools.jpg"
---

# 🛠️[개발 도구 및 환경] `org.springframework.boot` 플러그인의 역할?
- Spring Boot 플러그인(`org.springframework.boot`)은 **Spring Boot 기반 프로젝트를 효율적으로 빌드하고 실행할 수 있도록 돕는 도구입니다.**
    - Gradle 또는 Maven과 같은 빌드 도구에서 사용되며, 다양한 작업을 자동화하여 개발 생산성을 높입니다.

## 1️⃣ 주요 역할.
### 1️⃣ Spring Boot 애플리케이션 실행 지원.
- Spring Boot 애플리케이션을 직접 실행할 수 있는 bootRun(Gradle) 또는 spring-boot:run(Maven) 작업을 제공합니다.
- 애플리케이션 실행에 필요한 클래스패스 설정 및 시작 클래스를 자동으로 감지합니다.
- 실행 명령 예:
```java
./gradlew bootRun
mvn spring-boot:run
```

### 2️⃣ 애플리케이션 패키징.
- Spring Boot 애플리케이션을 **실행 가능한 JAR 또는 WAR 파일로 패키징합니다.**
- 패키징된 파일에는 필요한 모든 의존성과 내장 웹 서버(예: Tomcat, Jetty)가 포함됩니다.
- 이를 통해 추가 설정 없이 독립 실행형 애플리케이션으로 배포가 가능합니다.
- Gradle:
```bash
./gradlew bootJar
./gradlew bootWar
```
- Maven:
```bash
mvn package
```

### 3️⃣ 의존성 관리.
- Spring Boot의 의존성 관리 기능을 제공합니다.
- spring-boot-dependencies BOM(Bill of Materials)을 통해 프로젝트에서 사용해야 할 의존성 버전을 자동으로 관리합니다.
- 이를 통해 호환성 문제를 최소화하고, 안정적인 의존성 구성을 유지할 수 있습니다.

### 4️⃣ 내장 웹 서버 통합.
- 내장된 Tomcat, Jetty, Undertow와 같은 웹 서버를 활용하여 웹 애플리케이션을 실행할 수 있도록 설정합니다.
- 개발 중에는 bootRun 명령으로 내장 서버를 쉽게 실행하고 테스트할 수 있습니다.

### 5️⃣ 설정 및 환경 관리.
- Spring Boot의 `application.properties` 또는 `application.yml`과 같은 설정 파일을 읽고 실행 시 적용합니다.
- 프로파일(Profiles)을 통해 다양한 실행 환경(dev, test, prod)을 쉽게 구성할 수 있습니다.

### 6️⃣ 개발 및 디버깅 지원.
- 애플리케이션 코드를 변경하면 자동으로 반영하는 **핫 리로드(Hot Reload)** 기능을 Spring DevTools와 연계하여 제공합니다.
- 이를 통해 코드 변경 후 서버를 다시 시작하지 않고도 테스트할 수 있습니다.

### 7️⃣ 테스트 지원.
- test 작업을 통해 Spring Boot 테스트를 실행합니다.
- Gradle/Maven 빌드 도구에 기본 제공되는 테스트 프레임워크(JUnit, TestNG)와 쉽게 통합됩니다.

### 8️⃣ 추가 작업 제공.
- Gradle
    - bootBuildeImage : 애플리케이션을 OCI(컨테이너) 이미지로 빌드.
- Maven
    - spring-boot:build-image : 컨테이너 이미지를 생성.

### 9️⃣ Spring Boot Gradle 플러그인 예제.
#### build.gradle 설정.
```groovy
plugins {
    id 'org.springframework.boot' version '3.4.0' // Spring Boot 플러그인
    id 'io.spring.dependency-management' version '1.1.3' // 의존성 관리
    id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
java {
    sourceCompatibility = '17'
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```
- 주요 명령어.
    - 애플리케이션 실행: `./gradlew bootRun`
    - JAR 생성: `./gradlew bootJar`
    - WAR 생성: `./gradlew bootWar`

### 1️⃣0️⃣ Spring Boot Maven 플러그인 예제.
#### pom.xml 설정.
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```
- 주요 명령어.
    - 애플리케이션 실행: `mvn spring-boot:run`
    - 패키징: `mvn package`

### 1️⃣1️⃣ 요약.
- Spring Boot 플러그인 Spring Boot 프로젝트를 쉽게 **실행, 패키징, 의존성 관리할 수 있는 도구입니다.**
- 아 플러그인을 활용하면 개발 속도가 크게 향상되고, 배포 및 관리가 간단해집니다.
