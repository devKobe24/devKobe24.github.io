---
title: 🛠️[개발 도구 및 환경] 빌드된 프로젝트 실행하기.
tags:
    - Development tools
    - Enviroments
date: "2024-11-23"
thumbnail: "/assets/img/thumbnail/tools.jpg"
---

# 🛠️[개발 도구 및 환경] 빌드된 프로젝트 실행하기.
## 1️⃣ plain.jar와 executable.jar.
- AWS Linux 2023 EC2 내에서 도서 관리 애플리케이션을 빌드하니 새로운 `build` 파일이 생겨났습니다.
    - `/build/libs/` 경로를 따라 들어가면 **plain.jar 파일과 executable.jar 파일이 생성되어 있는 것을 확인할 수 있습니다.**
```bash
library-app-0.0.1-SNAPSHOT-plain.jar  // <- plain.jar 파일
library-app-0.0.1-SNAPSHOT.jar    // <- executable.jar 파일
```
- 이 두 가지 파일을 예로 들어 각각의 의미와 차이를 설명해보겠습니다.

## 2️⃣ library-app-0.0.1-SNAPSHOT-plain.jar
- **설명** 
    - "Plain JAR"로 불리며, 프로젝트의 클래스 파일과 리소스만 포함된 JAR입니다.
    - 의존성 라이브러리나 실행 진입점(main())이 설정되지 않은 단순 JAR 파일입니다.
- **포함 내용**
    - 프로젝트 소스에서 빌드되고 컴파일된 클래스 파일(.class).
    - META-INF 디렉토리와 리소스 파일.
- **사용 목적**
    - 독립 실행이 불가능하며, 다른 프로젝트에서 의존성으로 사용할 때 활용될 수 있습니다.
    - 라이브러리 형태로 배포하거나 특정 작업에 사용됩니다.

## 3️⃣ library-app-0.0.1-SNAPSHOT.jar
- **설명**
    - "Executable JAR"로 불리며, 실행 가능한 JAR 파일입니다.
    - 이 파일에는 모든 의존성 라이브러리와 실행 진입점(main() 메서드)을 포함합니다.
- **포함 내용**
    - library-app-0.0.1-SNAPSHOT-plain.jar의 모든 내용.
    - 의존성 라이브러리들이 포함되어 실행 가능한 환경이 됩니다.
    - META-INF/MANIFEST.MF 파일에 Main-Class가 설정되어 있어, java -jar 명령으로 실행할 수 있습니다.
- **사용 목적**
    - 애플리케이션을 실행하거나 배포할 때 사용됩니다.
    - 독립 실행 가능한 애플리케이션 JAR로 사용됩니다.

## 4️⃣ 주요 차이점.

| 구분 | plain.jar | executable.jar |
| -------- | -------- | -------- |
| 의존성 포함 여부 | 포함되지 않음 | 포함되어 있음|
| 실행 가능 여부 | 실행 불가능 | 실행 가능 |
| 사용 목적 | 라이브러리 형태로 사용 | 독립 실행 가능한 애플리케이션 |
| Manifest 설정 | Main-Class 없음 | Main-Class 포함 |

## 5️⃣ 선택 기준.
- **실행 가능한 애플리케이션으로 사용하려면**
    - libaray-app-0.0.1-SNAPSHOT.jar를 사용하여 실행.
```bash
java -jar library-app-0.0.1-SNAPSHOT.jar
```

- **라이브러리나 종속 프로젝트로 활용하려면**
    - library-app-0.0.1-SNAPSHOT-plain.jar를 배포하거나 의존성으로 추가.
