---
title: "🌐[Network] Load Balancer(로드 밸런서)란 무엇일까요?"
tags:
    - Network
date: "2024-12-29"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] Load Balancer(로드 밸런서)란 무엇일까요?
## 📌 Intro.
- ↘︎ **Load Balancer(로드 밸런서)는 네트워크 트래픽을 여러 서버로 분산시켜 시스템의 성능, 안정성, 가용성을 높이는 기술 또는 장치**입니다.

## 1️⃣ Load Balancer(로드 밸런서)의 핵심 역할.
- **1️⃣ 트래픽 분산:**
    - ↘︎ 여러 서버(노드)에 사용자 요청(트래픽)을 고르게 분산.
    - ↘︎ 특정 서버에 과부하가 걸리지 않도록 관리.
- **2️⃣ 고가용성(High Availability):**
    - ↘︎ 일부 서버가 다운되더라도 다른 서버가 트래픽을 처리하여 서비스 중단 방지.
- **3️⃣ 확장성(Scalability):**
    - ↘︎ 서버를 추가하거나 제거하여 트래픽 처리 능력을 쉽게 확장.
- **4️⃣ 장애 감지 및 복구:**
    - ↘︎ 주기적으로 서버 상태를 확인(Health Check).
    - ↘︎ 문제가 있는 서버로의 트래픽 전달을 중단.

## 2️⃣ Load Balancer(로드 밸런서)의 작동 원리.
- **1️⃣ 클라이언트 요청:**
    - ↘︎ 사용자가 웹사이트나 애플리케이션에 요청을 보냄.
- **2️⃣ 로드 밸런서(Load Balancer)로 전달:**
    - ↘︎ 요청은 먼저 로드 밸런서(Load Balancer)로 전달됨.
- **3️⃣ 서버 선택:**
    - ↘︎ 로드 밸런서(Load Balancer)는 여러 알고리즘 중 하나를 사용하여 최적의 서버를 선택.
- **4️⃣ 요청 전달:**
    - ↘︎ 선택된 서버로 요청을 전달.
- **5️⃣ 응답 반환:**
    - ↘︎ 서버는 요청을 처리한 후, 응답을 로드 밸런서(Load Balancer)를 통해 클라이언트로 반환.

## 3️⃣ 로드 밸런싱 알고리즘(Load Balancing Algorithm)
- **1️⃣ 라운드 로빈(Round Robin)**
    - ↘︎ 요청을 각 서버에 순차적으로 분산.
- **2️⃣ 가중 라운드 로빈(Weighted Round Robin)**
    - ↘︎ 서버에 가중치를 부여해 더 강력한 서버에 더 많은 요청 전달.
- **3️⃣ 최소 연결(Least Connections)**
    - ↘︎ 현재 가장 적은 연결을 가진 서버로 요청 전달.
- **4️⃣ IP 해시(IP Hash)**
    - ↘︎ 클라이언트의 IP 주소를 해시로 변환하여 특정 서버로 항상 연결.
- **5️⃣ 응답 시간 기반(Least Response Time)**
    - ↘︎ 가장 빠르게 응답할 수 있는 서버에 요청 전달.

## 4️⃣ 로드 밸런서(Load Balancer)의 유형.
- **1️⃣ 하드웨어 로드 밸런서(Hardware Load Balancer)**
    - ↘︎ 전용 장비로 트래픽을 분산
        - ↘︎ 예: F5, Citrix NetScaler
- **2️⃣ 소프트웨어 로드 밸런서(Software Load Balancer)**
    - ↘︎ 소프트웨어로 구현된 로드 밸런서(Load Balancer)
        - ↘︎ 예: Nginx, HAProxy
- **3️⃣ 클라우드 로드 밸런서(Cloud Load Balancer)**
    - ↘︎ 클라우드 서비스 제공자가 관리하는 로드 밸런서(Load Balancer)
        - ↘︎ 예: AWS ELB(Elastic Load Balancer), GCP Load Balancer, Azure Load Balancer

## 5️⃣ 로드 밸런서(Load Balancer)의 계층.
- **1️⃣ L4(Layer 4) 로드 밸런서**
    - ↘︎ **OSI 4계층(Transport Layer)에서 작동.**
    - ↘︎ TCP/UDP 프로토콜 기반의 트래픽 분산.
    - ↘︎ 속도가 빠르고 효율적.
- **2️⃣ L7(Layer 7) 로드 밸런서**
    - ↘︎ **OSI 7계층(Application Layer)에서 작동.**
    - ↘︎ HTTP/HTTPS 트래픽을 기반으로 분산.
    - ↘︎ URL, 쿠키, HTTP 헤더 등으로 세부적인 트래픽 제어 가능.

## 6️⃣ 로드 밸런서(Load Balancer)의 장점.
- **1️⃣ 고가용성 및 안정성**
    - ↘︎ 서버 장애 시 자동으로 트래픽을 다른 서버로 전달.
- **2️⃣ 확장성**
    - ↘︎ 수요가 증가할 때 서버를 손쉽게 추가 가능.
- **3️⃣ 보안 강화**
    - ↘︎ DDoS 공격 방어, SSL 암호화 지원.
- **4️⃣ 성능 최적화**
    - ↘︎ 트래픽을 효율적으로 분산하여 서버 과부하 방지.

## 7️⃣ 로드 밸런서(Load Balancer)의 사용 사례.
- **1️⃣ 웹 애플리케이션**
    - ↘︎ 여러 웹 서버로 트래픽을 분산하여 빠른 응답 제공.
- **2️⃣ 데이터베이스 부하 분산**
    - ↘︎ 데이터베이스 읽기/쓰기 요청을 여러 DB 서버로 분산.
- **3️⃣ 클라우드 환경**
    - ↘︎ AWS, Azure, GCP 등에서 클라우드 로드 밸런서(Cloud Load Balancer) 사용.
- **4️⃣ API Gateway**
    - ↘︎ API 요청을 여러 서버로 분산 처리.

## 8️⃣ 로드 밸런서(Load Balancer) 선택 기준.
- **1️⃣ 트래픽 유형**
    - ↘︎ HTTP/HTTPS 트래픽이면 Layer 7 사용.
    - ↘︎ TCP/UDP 트래픽이면 Layer 4 사용.
- **2️⃣ 확장성 요구사항**
    - ↘︎ 수직 확장 또는 수평 확장
- **3️⃣ 보안 요구사항**
    - ↘︎ SSL/TLS 지원 여부
- **4️⃣ 예산과 비용**
    - ↘︎ 하드웨어 vs 소프트웨어 vs 클라우드

## 🚀 결론.
- ↘︎ **로드 밸런서(Load Balancer)는 트래픽 분산, 고가용성, 확장성, 보안을 위한 핵심 요소.**
- ↘︎ **클라우드 기반**에서 운영 중이라면 **AWS ELB, Azure Load Balancer**와 같은 서비스를 고려.
- ↘︎ 애플리케이션의 요구사항에 따라 L4 또는 L7 로드 밸런서(Load Balancer)를 선택.
- **🔑 핵심: "서버의 부담을 나누고, 장애를 최소화하며, 서비스 가용성을 높인다."**
