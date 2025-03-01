---
title: 🛠️[개발 도구 및 환경] `./gradlew build -x test` 명령어는 무엇인가요?
tags:
    - Development tools
    - Enviroments
date: "2024-11-23"
thumbnail: "/assets/img/thumbnail/tools.jpg"
---

# 🛠️[개발 도구 및 환경] `./gradlew build -x test` 명령어는 무엇인가요?
- `./gradlew build -x test` 명령어는 Gradle 빌드 시스템에서 프로젝트를 빌드(Build)하면서 **테스트(Test) 단계를 건너뛰기** 위한 명령어입니다.

## 1️⃣ 명령어 구성.
### 1️⃣ ./gradlew
- Gradle Wrapper를 실행하는 명령어입니다.
- 프로젝트 내에 포함된 Gradle Wrapper(gradlew)를 사용하여 Gradle 명령어를 실행합니다.
- Gradle Wrapper를 사용하면 프로젝트에 설정된 특정 Gradle 버전을 자동으로 다운로드하고 실행할 수 있습니다.

### 2️⃣ build
- Gradle의 기본 **빌드(Build) 작업(Task)입니다.**
- 프로젝트를 빌드하여 컴파일, 테스트, 패키징(JAR/ZIP 생성 등)을 수행합니다.
- build는 여러 단계를 포함하며, 일반적으로 다음을 실행합니다.
    - **compileJava :** 자바 소스 코드 컴파일.
    - **processResources :** 리소스 파일 처리.
    - **test :** 테스트 실행.
    - **assemble :** 아티팩트 생성(JAR 등).
    - **check :** 품질 검사 및 테스트 통합.
    - **build :** 위 단계들을 통합하여 실행.

### 3️⃣ -x test
- -x는 특정 작업(Task)을 **제외(Exclude)한다는 옵션입니다.**
- 여기서는 test **작업을 실행하지 않고 건너뛴다**는 의미입니다.
- test는 JUnit등으로 작성된 단위 테스트를 실행하고 결과를 확인하는 작업입니다.

## 2️⃣ 명령어의 동작.
- ./gradlew build -x test는 **테스트 단계를 제외하고 프로젝트를 빌드합니다.**
- 빌드 과정에서:
    - 소스 코드는 컴파일 되고,
    - 리소스 파일은 처리되며,
    - 결과물(JAR, ZIP 등)이 생성됩니다.
    - 단, **테스트는 실행되지 않습니다.**

## 3️⃣ 언제 사용하나요?
### 1️⃣ 테스트 실행 시간이 오래 걸릴 때.
- 테스트 단계는 시간이 오래 걸릴 수 있습니다.
- 빠르게 빌드를 완료하고 결과물(JAR 파일 등)만 생성하려는 경우 유용합니다.

### 2️⃣ 테스트가 실패하거나 불완전한 상태일 때.
- 테스트 코드가 완벽하지 않거나 실패하지만, 결과물만 생성이 필요한 경우 사용할 수 있습니다.

### 3️⃣ CI/CD 파이프라인에서 선택적으로 테스트를 건너뛸 때
- 테스트 단계가 별도의 프로세스에서 실행되는 경우, 빌드 시 테스트를 생략할 수 있습니다.

## 4️⃣ 예시.
### 1️⃣ 테스트 포함 빌드.
```bash
./gradlew build
```
- 테스트 단계도 포함됩니다.

### 2️⃣ 테스트 제외 빌드.
```bash
./gradlew build -x test
```
- 테스트 단계를 제외하고 빌드가 진행됩니다.

## 5️⃣ 주의사항.
- 테스트를 생략하면 코드의 품질이나 안정성을 보장할 수 없습니다.
- 최종 배포나 프로덕션 환경에서는 테스트 단계를 포함한 빌드를 사용하는 것이 권장됩니다.

## 6️⃣ 요약.
- ./gradlew build -x test 명령어는 Gradle을 사용해 프로젝트를 빌드하면서 **테스트 단계를 생략**하는 명령어입니다.
- 빠른 빌드가 필요하거나 테스트 상태와 관계없이 결과물만 생성하려는 상황에서 유용합니다.
