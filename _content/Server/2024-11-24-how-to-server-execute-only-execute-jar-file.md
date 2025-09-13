---
title: "🧑‍💻[Server] `jar` 파일만 실행했는데 서버가 동작이 가능한 이유는 무엇일까요?"
tags:
    - Server
date: "2024-11-24"
thumbnail: "/assets/img/thumbnail/server.jpg"
---

# 🧑‍💻[Server] `jar` 파일만 실행했는데 서버가 동작이 가능한 이유는 무엇일까요?
- Spring Boot 애플리케이션의 **JAR 파일**만 실행해도 서버가 동작하는 이유는 Spring Boot가 제공하는 **자체 내장 웹 서버** 덕분입니다.

## 1️⃣ Spring Boot의 내장 서버.
- Spring Boot는 웹 애플리케이션을 실행하기 위해 외부 WAS(Web Application Server, 예: Tomcat, Jetty 등)를 별도로 설치할 필요가 없습니다.
- Spring Boot는 내장 웹 서버를 포함하여 JAR 파일로 패키징되며, 실행 시 애플리케이션과 웹 서버를 동시에 구동합니다.

### 1️⃣ 주요 내장 서버.
- **Tomcat(기본)**
- **Jetty**
- **Undertow**

## 2️⃣ JAR 파일 실행과 내장 서버 구동.
- Spring Boot 애플리케이션을 빌드하면 Maven이나 Gradle에 의해 실행 가능한 JAR 파일이 생성됩니다.
    - 이 JAR 파일에는 다음이 포함됩니다.
        - 1. 애플리케이션 코드(사용자가 작성한 Java 코드 및 설정)
        - 2. 종속 라이브러리(Spring Framework, 데이터베이스 드라이버 등)
        - 3. 내장 웹 서버(예: Tomcat)
    - JAR 파일을 실행하면 다음이 일어납니다.
        - 1. java -jar 명령으로 main() 메서드가 실행됩니다.
        - 2. Spring Boot는 내장된 Tomcat 등의 서버를 시작합니다.
        - 3. 애플리케이션 컨텍스트를 초기화하고, `@Controller`, `@RestController` 등으로 정의된 엔드포인트를 매핑합니다.
        - 4. 내장 웹 서버가 지정된 포트(기본 8080)에서 HTTP 요청을 수신합니다.

## 3️⃣ JAR 파일 구조.
- Spring Boot의 실행 가능한 JAR 파일은 일반 JAR 파일과 구조가 다릅니다.
    - 이를 **"Fat JAR"** 또는 **"Uber JAR"라고** 부릅니다.
        - Fat JAR은 애플리케이션의 코드와 모든 종속 라이브러리를 포함합니다.

### 1️⃣ Fat JAR 주요 구성
```bash
library-app-0.0.1-SNAPSHOT.jar
├── BOOT-INF/
│   ├── classes/       # 애플리케이션의 컴파일된 클래스 파일
│   ├── lib/           # 모든 종속 라이브러리
│   └── META-INF/      # 메타데이터
└── org/               # Spring Boot 로더 (Spring Boot 실행 관련 코드)
```

### 2️⃣ Spring Boot Loader
- org.springframework.boot.loader 패키지의 코드는 JAR 파일을 실행 가능한 애플리케이션으로 만듭니다.
- 실행 시 내장 서버를 초기화하고, 애플리케이션의 main() 메서드를 호출합니다.

## 4️⃣ 내장 서버의 동작 원리.
- Spring Boot는 내장 서버를 자동으로 구성합니다.
    - 예를 들어 기본 내장 서버인 Tomcat의 동작은 다음과 같습니다.
        - 1. Spring Boot가 Tomcat의 EmbeddedServletContainer를 초기화합니다.
        - 2. 사용자가 작성한 `@Controller` 또는 `@RestController`에서 정의한 엔드포인트를 Tomcat의 서블릿으로 등록합니다.
        - 3. 서버는 지정된 포트(기본값: 8080)에서 요청을 수신 대기합니다.

### 1️⃣ 내장 서버와 외부 서버 비교.

| 항목 | 내장 서버 | 외부 서버 |
| -------- | -------- | -------- |
| 설치 필요 여부 | 설치 불필요 | 사전 설치 불필요 |
| 애플리케이션 배포 | JAR 파일로 실행 가능 | WAR 파일을 서버에 배포해야 함 |
| 사용 편의성 | 간단하고 빠름 | 설정 및 유지보수 필요 |

## 5️⃣ 실행 가능한 JAR 파일의 장점.
- **1. 단순화 :** 개발, 빌드, 배포, 실행 과정이 간단합니다.
- **2. 이식성 :** JAR 파일만 있으면 Java가 설치된 모든 환경에서 실행 가능합니다.
- **3. 독립성 :** 외부 WAS가 필요하지 않으므로 종속성이 줄어듭니다.
- **4. 빠른 실행 :** 별도의 서버 설정 없이 바로 애플리케이션을 구동할 수 있습니다.

## 6️⃣ 결론.
- Spring Boot 애플리케이션의 JAR 파이란 실행했을 때 서버가 동작하는 이유
    - 1. **내장된 웹 서버(Tomcat, Jetty, Undertow 등)가 포함되어 있기 때문입니다.**
    - 2. **JAR 파일 안에 애플리케이션 코드와 모든 종속 라이브러리가 포함된 Fat JAR 형태로 패키징되기 때문입니다.**
        - 이 접근 방식은 배포와 실행을 간소화하고, 개발자가 빠르게 애플리케이션을 실행할 수 있도록 지원합니다.
