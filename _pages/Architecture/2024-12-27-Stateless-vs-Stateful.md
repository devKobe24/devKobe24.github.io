---
title: 🏗️[Architecture] Stateless 방식 vs Stateful 방식
tags:
    - Architecture
    - Software Architecture
    - System Design
date: "2024-12-27"
thumbnail: "/assets/img/thumbnail/architecture.jpg"
---

# 🏗️[Architecture] Stateless 방식 vs Stateful 방식.
- **Stateless(무상태)와 Stateful(상태 유지)** 방식
    - ↘︎ 시스템이나 애플리케이션이 **사용자 상태**를 어떻게 관리하는지에 대한 설계 패턴.
    - ↘︎ 주로 **인증(Authentication)과 세션 관리(Session Management)에서 중요한 개념**

## 1️⃣ Stateless(무상태) 방식.
### 📌 정의.
- 서버가 **클라이언트의 상태를 저장하지 않는 방식**
- 각 요청(Request)는 **독립적,** 이전 요청(Request)의 **상태나 정보를 기억하지 않음.**
- **JWT(JSON Web Token)와** 같은 **토큰 기반 인증**이 주로 **사용.**

### 🛠️ 작동 방식.
- 1️⃣ 클라이언트가 서버에 로그인 요청을 함.
- 2️⃣ 서버는 JWT(Access Token + Refresh Token)를 발급함.
- 3️⃣ 클라이언트는 요청 시마다 Access Token을 헤더에 포함해 보냄.
- 4️⃣ 서버는 해당 Access Token의 유효성을 검증함.
- 5️⃣ 상태(세션 등)는 **토큰 내 페이로드(Payload)에 저장됨.**

### 🙌 특징.
- **서버 확장성.**
    - ↘︎ 여러 서버에 분산 배포하기 쉬움.
- **메모리 사용량 최소화.**
    - ↘︎ 서버는 세션 정보를 저장하지 않으므로 메모리 소모가 적음.
- **유연한 인증 처리.**
    - ↘︎ 중앙 집중식 인증 서버로 구성 가능.
- **보안 취약점.**
    - ↘︎ 토큰 유출 시 악용될 위험이 있음.(토큰 만료 시간, HTTPS 사용 필요.)

### 📝 예시.
- **JWT(JSON Web Token)를 사용한 인증**
- **API Gateway를 통해 Stateless 인증 처리**

### 👍 장점.
- 서버가 세션을 저장할 필요가 없어 확장성이 뛰어남.
- 로드 밸런싱(Load Balancing)에 유리.
- 요청마다 인증이 독립적으로 수행.

### ⚠️ 단점
- 토큰이 클라이언트에 저장되므로 노출 위험이 존재.
- 로그아웃 처리 시 Access Token은 무효화하기 어려움.
    - ↘︎ Refresh Token을 무효화하여 처리함.

## 2️⃣ Stateful(상태 유지) 방식.
### 📌 정의.
- 서버가 **클라이언트의 상태 정보를 저장하고 관리함.**
- 주로 **세션(Session)을** 사용해 **클라이언트의 상태를 유지함.**
- 사용자의 로그인 상태나 권한 정보가 서버 메모리 또는 데이터베이스(DataBase, DB)에 저장됨.

### 🛠️ 작동 방식.
- 1️⃣ 클라이언트가 로그인 요청을 함.
- 2️⃣ 서버는 **세션 ID(Session ID)를 생성**하고, 클라이언트에게 전달함.
- 3️⃣ 서버는 해당 **세션 ID(Session ID)를 기반**으로 사용자의 상태 정보를 저장함.
- 4️⃣ 클라이언트는 요청 시마다 **세션 ID(Session ID)를 쿠키나 헤더에 포함해 보냄.**
- 5️⃣ 서버는 **세션 저장소**를 통해 상태를 확인함.

### 🙌 특징.
- **상태 유지.**
    - ↘︎ 서버가 사용자의 세션을 유지하며 상태를 추적함.
- **세션 저장소.**
    - ↘︎ 상태 정보는 서버 메모리, Redis, DB 등에 저장됨.
- **로드 밸런싱 문제.**
    - 사용자가 다른 서버에 접근하면 세션 정보가 없을 수 있음.(Sticky Session 사용 필요.)

### 📝 예시.
- **HttpSession을 사용한 세션 관리.**
- **Redis를 사용한 세션 클러스터링**

### 👍 장점.
- 세션 무효화가 즉시 가능 (로그아웃 시 상태 관리 가능)
- 사용자별 상태를 쉽게 관리할 수 있음

### ⚠️ 단점
- 서버 메모리 사용량 증가.
- 로드 밸런싱 문제 발생(Stick Session 필요)
- 서버 확장 시 세션 정보 동기화 필요

## 3️⃣ 어떤 방식을 선택해야 할까? 🤔
### 1️⃣ Stateless(무상태) 방식 추천 시나리오.
- RESTful API 서비스.
- 마이크로서비스 아키텍처.
- 서버 간 부하 분산(로드 밸런싱)이 필요할 때.
- 높은 확장성이 필요할 때.

### 2️⃣ Stateful(상태 유지) 방식 추천 시나리오.
- 상태를 지속적으로 관리해야 하는 경우.
- 즉각적인 세션 무효화가 필요한 경우.
- 대규모 사용자 세션 관리가 필요할 때.

## 4️⃣ 하이브리드 접근법.
- **Stateless (Access Token) + Stateful (Refresh Token)**
    - ↘︎ Access Token 👉 Stateless로 인증을 수행.
    - ↘︎ Refresh Token 👉 Stateful로 서버가 유효성을 관리.

## 5️⃣ 권장 접근 방식 🙋‍♂️
- 인증: **Stateless (JWT).**
- 세션 관리: **Stateful (Refresh Token 관리).**
- 로그아웃 처리: Refresh Token을 revoke하고 Access Token은 클라이언트 측에서 제거.

## 🚀 결론.
- **Stateless(무상태) 방식.**
    - ↘︎ 확장성과 유연성이 뛰어나며, API 인증에 적합함.
- **Stateful(상태 유지) 방식.**
    - ↘︎ 사용자의 세션 정보를 관리해야 하는 경우 유리함.
- 현대적인 아키텍처에서는 Stateless 방식을 주로 사용하며, Refresh Token을 통해 필요한 부분만 Stateful로 관리하는 **하이브리드 방식**이 널리 사용됨.
