---
title: 🛠️[개발 도구 및 환경] Docker와 application.yml
tags:
    - Development tools
    - Enviroments
    - Docker
date: "2025-07-21"
thumbnail: "/assets/img/thumbnail/tools.jpg"
---

# 🔍 핵심 차이 요약

| 구분 | Docker | application.yml |
| -------- | -------- | -------- |
| 종류 | 도구/플랫폼 | 설정 파일|
| 역할 | 컨테이너 실행 및 배포 환경 구성 | Spring Boot 등 앱의 내부 설정 정의 |
| 대상 | 인프라, 네트워크, 이미지, 컨테이너 등 | DB 정보, 포트, 보안 등 앱 내부 설정 |
| 예시 | Dockerfile, docker-compose.yml | spring.datasource.url, jwt.secret-key 등 |

---

# ✅ 자세히 설명
## 1. 🐳 Docker란?
- **애플리케이션을 컨테이너로 실행하는 플랫폼**
- 애플리케이션이 어떤 환경에서도 일관되게 실행될 수 있게 해줌
- 구성:
    - Dockerfile: 컨테이너 만들기 위한 이미지 정의
    - docker-compose.yml: 여러 컨테이너를 한번에 관리

## 2. 📝 application.yml이란?
- Spring Boot의 **애플리케이션 설정 파일**
- DB 연결, 포트, 인증 키, CORS, 로그 등 **앱 내부 설정값을 정의**
```yaml
# 예시: application.yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/blog
    username: root
    password: secret
server:
  port: 8080
```
---

## 3. 🎯 함께 사용하는 예시
Docker에서 컨테이너를 실행할 때, 환경 변수로 application.yml에서 사용할 값을 외부에서 주입할 수 있어요:
```yaml
# docker-compose.yml
services:
  blog-backend:
    image: blog-api:latest
    ports:
      - "8080:8080"
    environment:
      - SPRING_PROFILES_ACTIVE=prod
      - DB_PASSWORD=${DB_PASSWORD}
```
그리고 application-prod.yml에는 아래처럼 작성해 놓을 수 있죠:
```yaml
spring:
  datasource:
    password: ${DB_PASSWORD}
```

---

## 4. 🗂️ 전체 구성 흐름 구조도.
예를 들어 하나의 Spring Boot 프로젝트에 다음 4가지 파일이 있을 때의 관계 구조도입니다.:
- application.yml
- application-prod.yml
- Dockerfile
- docker-compose.yml

```plaintext
📦 프로젝트 루트
│
├── 📄 application.yml                 ← 기본 설정 (공통)
├── 📄 application-prod.yml           ← 프로필이 prod일 때만 적용 (DB 등 실서버용)
│
├── 🐳 Dockerfile                     ← Spring Boot 앱을 Docker 이미지로 만듦
│     └── 내부에서 JAR 실행 (ex: java -jar app.jar)
│
├── 🐳 docker-compose.yml             ← 여러 서비스 컨테이너 일괄 실행
│     └── 환경 변수로 SPRING_PROFILES_ACTIVE=prod 설정
│
└── 🏃 최종 실행 흐름
      docker-compose → 컨테이너 실행 → Dockerfile로 만든 이미지 실행
        └─ Spring Boot 앱 시작
              └─ application.yml 로딩
              └─ (prod 프로필이면) application-prod.yml도 함께 로딩
```

---

## 5. 🔄 관계 흐름 요약
1. **docker-compose.yml**
➞ 컨테이너를 띄우고 SPRING_PROFILES_ACTIVE=prod 환경 변수 전달
➞ 예:
```yaml
environment:
  - SPRING_PROFILES_ACTIVE=prod
```

2. **Dockerfile**
➞ Spring Boot 앱을 이미지로 만들고 실행(java -jar)
➞ JAR 실행 시, Spring Boot는 application.yml을 먼저 읽고 이어서 application-prod.yml을 덮어쓰기 형식으로 추가 적용

3. **application.yml / application-prod.yml**
➞ 설정 파일들:
    - application.yml: 공통 설정
    - application-prod.yml: 운영 환경에서 덮어쓸 설정(e.g. 실 DB, JWT 키 등)

---

## 6. 🔍 시각적 흐름 이미지 (ASCII 구조도)
```plaintext
[docker-compose.yml]
       │
       ▼
Set SPRING_PROFILES_ACTIVE=prod
       │
       ▼
  Run container
       │
       ▼
[Dockerfile → java -jar app.jar]
       │
       ▼
Spring Boot App Start
       │
       ├── application.yml          ← 기본 설정 로드
       └── application-prod.yml     ← prod 프로필이면 병합 적용
```

---

## 7. 🙌 실무 팁
- **Local 환경 :** SPRING_PROFILES_ACTIVE=local
➞ application-local.yml 따로 설정
- **Dev, Prod 분리 :** application-dev.yml, application-prod.yml 로 나눠 관리
- application.yml에는 공통 설정만 두고, 나머지는 프로필별로 override

---

## ✅ 결론

> **Docker는 배포/실행 환경을 구성하는 도구이고, application.yml은 애플리케이션 내부 설정을 정의하는 파일입니다.**
> 서로 다른 책임을 가지지만, **함께 조합해서 실무에서 자주 사용됩니다.**
