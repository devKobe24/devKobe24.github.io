---
title: "☁️[AWS] `chmod +x ./gradlew` 명령어는 무엇일까요?"
tags:
    - AWS
    - Network
date: "2024-11-22"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] `chmod +x ./gradlew` 명령어는 무엇일까요?
- `chmod +x ./gradlew` 명령어는 Unix/Linux 시스템에서 특정 파일(./gradlew)에 실행 권한(Executable Permission)을 부여하는 명령어입니다.

## 1️⃣ 명령어 구성.
- **1. chmod**
    - Change Mode의 약자로, 파일 또는 디렉터리의 권한을 변경하는 명령어입니다.
- **2. +x**
    - 실행 권한을 추가하라는 의미입니다.
    - 여기서 x는 Excutable(실행 기능)을 나타냅니다.
- **3. ./gradlew**
    - 현재 디렉터리(./)에 있는 gradlew 파일을 지정합니다.
    - gradlew는 Gradle Wrapper 실행 스크립트로, 프로젝트에서 Gradle을 실행하기 위한 스크립트 파일입니다.

## 2️⃣ 왜 사용하나요?
- **gradlew를 실행 파일로 만들기 위해**
    - 기본적으로 다운로드한 파일은 실행 권한이 없을 수 있습니다.
    - 실행 권한을 부여하지 않으면 gradlew를 실행하려고 할 때 "Permission denied" 오류가 발생합니다.
    - 실행 권한을 부여하면 터미널에서 ./gradlew 명령으로 Gradle Warpper를 실행할 수 있습니다.

## 3️⃣ 실행 과정.
### 1️⃣ 권한 확인 전.
```bash
ls -l ./gradlew
```
- **실행 권한이 없는 경우.**
```bash
-rw-r---r-- 1 user group 1234 Nov 10 12:00 gradlew
```

### 2️⃣ chmod +x ./gradlew 실행.
- 실행 권한을 부여.
```bash
chmod +x ./gradlew
```

### 3️⃣ 권한 확인 후.
```bash
ls -l ./gradlew
```
- 실행 권한이 추가된 경우.
```bash
-rwxr-xr-x 1 user group 1234 Nov 10 12:00 gradlew
```
- x는 실행 권한을 의미하며, 파일 소유자 그룹, 기타 사용자에게 각각 실행 권한이 부여됩니다.

## 4️⃣ ./gradlew란?
- **Gradle Wrapper 스크립트**
    - Gradle을 설치하지 않아도 프로젝트에 정의된 Gradle 버전을 다운로드하고 실행할 수 있도록 돕는 스크립트.
    - 플랫폼에 따라 .bat(Windows) 또는 스크립트 파일(gradlew) 형태로 제공됩니다.

## 5️⃣ 요약.
- chmod +x ./gradlew 명령어는 gradlew 파일에 실행 권한을 부여하여 Gradle Wrapper를 실행할 수 있도록 준비하는 과정입니다.
    - 이 명령을 통해 Gradle을 명령어로 직접 실행할 수 있습니다.

### 👉 예: Gradle Wrapper 실행.
- 권한을 부여한 후.
```bash
./gradlew build
```
- 위 명령어로 Gradle 프로젝트를 빌드할 수 있습니다.
