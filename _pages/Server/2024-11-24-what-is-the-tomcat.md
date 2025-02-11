---
title: "🧑‍💻[Server] Tomcat이란 무엇일까요?"
tags:
    - Server
date: "2024-11-24"
thumbnail: "/assets/img/thumbnail/server.jpg"
---

# 🧑‍💻[Server] Tomcat이란 무엇일까요?
- **Tomcat은 Apache Software Foundation에서 개발 및 유지보수하는 Java 기반의 웹 애플리케이션 서버(WAS, Web Application Server)입니다.**
    - 정식 명칭은 **Apache Tomcat이며, 서블릿(Servlet)과 JSP(JavaSserver Page) 기술을 지원하는 가장 널리 사용되는 WAS 중 하나입니다.**

## 1️⃣ Tomcat의 주요 역할.
- Tomcat은 **Java EE(Java Platform, Enterprise Edition)의 일부인 서블릿과 JSP를 실행할 수 있는 환경을 제공합니다.**
    - 이를 통해 다음과 같은 작업이 가능합니다.
        - **HTTP 요청 처리 :** HTTP 클라이언트(브라우저)로부터 요청을 받아 처리하고 응답을 보냅니다.
        - **서블릿 실행 :** Java Servlet은 서버 측에서 실행되는 Java 프로그램으로, 동적인 웹 콘텐츠를 생성합니다.
        - **JSP 실행 :** JSP(JavaServer Page)는 HTML 문서에 Java 코드를 삽입하여 동적인 웹 콘텐츠를 생성합니다.
        - **새션 관리 :** 사용자의 상태를 유지하기 위한 세션(Session)을 관리합니다.
        - **HTTP/HTTPS 지원 :** HTTP 및 HTTPS 프로토콜을 통해 웹 애플리케이션과 클라이언트 간의 통신을 처리합니다.

## 2️⃣ Tomcat의 주요 구성 요소.
- Tomcat은 여러 구성 요소로 이루어져 있으며, 각 요소는 특정 역할을 담당합니다.

### 1️⃣ Catalina
- Tomcat의 핵심인 **서블릿 컨테이너입니다.**
- 서블릿을 로드하고 실행하며, HTTP 요청을 처리합니다.

### 2️⃣ Coyote
- HTTP 커넥터로, 클라이언트의 HTTP 요청을 받아 Catalina에 전달합니다.
- HTTP/1.1 및 HTTPS를 지원합니다.

### 3️⃣ Jasper
- JSP 엔진으로, JSP 파일을 서블릿으로 변환하고 실행합니다.

### 4️⃣ Cluster
- 클러스터링 기능을 제공하여 여러 Tomcat 서버 간의 세션 데이터를 공유합니다.

### 5️⃣ Web Application
- Tomcat에 배포된 웹 애플리케이션을 관리하고 실행합니다.
- WAR 파일(Web Application Archive) 형식으로 애플리케이션을 배포할 수 있습니다.

## 3️⃣ Tomcat의 특징.
### 1️⃣ 경량화된 서버.
- Tomcat은 WAS 중에서도 가볍고 설치 및 실행이 쉽습니다.

### 2️⃣ 무료 및 오픈소스.
- Apache Software Foundation이 관리하며, 누구나 무료로 사용할 수 있습니다.

### 3️⃣ 서블릿과 JSP 표준 지원.
- Java EE 표준 기술을 지원합니다.

### 4️⃣ 확장 가능성.
- 다양한 추가 모듈과 플러그인을 통해 확장할 수 있습니다.

### 5️⃣ 유연성.
- 필요에 따라 설정과 구성을 쉽게 변경할 수 있습니다.

## 4️⃣ Tomcat의 작동 방식.
### 1️⃣ 요청 수신.
- Coyote가 클라이언트로부터 HTTP 요청을 수신합니다.

### 2️⃣ 서블릿 처리.
- Catalina가 요청을 분석하여 적절한 서블릿을 호출합니다.
- 서블릿이 요청을 처리하고 결과를 생성합니다.

### 3️⃣ JSP 처리.
- 요청된 JSP 파일이 있다면 Jasper가 이를 서블릿으로 변환합니다.
- 변환된 서블릿이 실행되어 결과를 생성합니다.

### 4️⃣ 응답 반환.
- 처리 결과를 HTTP 응답으로 클라이언트에 반환합니다.

## 5️⃣ Tomcat의 장단점.
### 1️⃣ 장점.
- **가벼움 :** 풀 스택 Java EE 애플리케이션 서버(예: WildFly, WebLogic)보다 가볍고 빠릅니다.
- **무료 :** 상용 WAS 대비 비용이 들지 않습니다.
- **사용 용이성 :** 설치와 설정이 간단하며, 개발 및 테스트 환경에서 자주 사용됩니다.
- **커뮤니티 지원 :** 오픈소스 프로젝트로 커뮤니티에서 다양한 도움과 자료를 제공받을 수 있습니다.

### 2️⃣ 단점.
- **기능 제한 :** Java EE의 모든 표준(예: EJB, JCA)을 지원하지 않습니다. 이는 WildFly, WebLogic과 같은 풀 스택 WAS와의 주요 차이점입니다.
- **고급 기능 부족 :** 엔터프라이즈급 기능(트랜잭션 관리, 메시징 등)이 부족합니다.

## 6️⃣ Tomcat이 주로 사용되는 사례.
- **개발 및 테스트 환경 :** 가볍고 빠르기 때문에 개발 단계에서 많이 사용됩니다.
- **소규모 애플리케이션 :** 큰 부하가 없는 간단한 웹 애플리케이션에서 많이 사용됩니다.
- **Spring Boot 애플리케이션 :** Spring Boot는 Tomcat을 기본 내장 서버로 사용하여 JAR 파일로 실행 가능하게 합니다.

## 7️⃣ Tomcat의 배포 방법.
### 1️⃣ WAR 파일 배포.
- webapps/ 디렉터리에 WAR 파일을 복사합니다.
- Tomcat이 자동으로 애플리케이션을 로드합니다.

### 2️⃣ JAR 파일 실행.
- Spring Boot와 같은 프레임워크를 사용하여 JAR 파일 형태로 배포 가능합니다. 이 경우 내장 Tomcat이 포함됩니다.

### 3️⃣ 설정 파일 사용.
- server.xml 및 web.xml을 사용하여 서버와 애플리케이션 설정을 조정할 수 있습니다.

## 8️⃣ 결론.
- Tomcat은 서블릿과 JSP 기술을 기반으로 한 Java 웹 애플리케이션 서버로, 경량화와 사용 용이성 덕분에 많은 개발자가 사용합니다.
- 특히 개발 및 테스트 환경에서 인기가 높으며, Spring Boot와 같은 최신 프레임워크에서도 내장 WAS로 활용됩니다.
