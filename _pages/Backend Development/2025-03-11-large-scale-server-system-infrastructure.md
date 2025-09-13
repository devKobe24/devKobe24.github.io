---
title: "📚[Backend Development] 대규모 시스템 서버 인프라"
tags:
    - Backend Ddevelopment
date: "2025-03-11"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 대규모 시스템 서버 인프라"
## ✅1️⃣ 대규모 시스템 서버 인프라란?
- 대규모 시스템 서버 인프라는 **"많은 사용자가 동시에 접속해도 원활하게 동작할 수 있도록 설계된 서버 환경"을** 의미합니다.
- 대표적인 예
    - 1. **클라우드 서비스(AWS, GCP, Azure)**
    - 2. **대형 웹사이트(네이버, 토스, 카카오톡)**
    - 3. **온라인 게임 서버(베틀그라운드, LOL)**
    - etc.
- 이러한 시스템에서는 **트래픽 처리, 확장성(Scalability), 가용성(Availability), 복원력(Resilience)등**이 매우 중요합니다.

## ✅2️⃣ 대규모 시스템 아키텍처의 핵심 요소.
### 1️⃣ 로드 밸런서 (Load Balancer)
- 서버에 들어오는 트래픽을 여러 서버로 분산하는 역할을 합니다.
- 대표적인 로드 밸런서
    - 1. **Nginx**
    - 2. **HAProxy**
    - 3. **AWS ELB**
    - etc.
- 예시: 사용자가 많아지면 한 대의 서버만으로 감당하기 어려우므로 여러 대의 서버에 요청을 분배합니다.

### 2️⃣ 웹 서버 (Web Server)와 애플리케이션 서버 (Application Server)
- 웹 서버(Nginx, Apache) ➞ 정적 파일(HTML, CSS, JS) 제공.
- 애플리케이션 서버(Spring Boot, Django, Express) ➞ 비즈니스 로직 수행.
- 예시: 클라이언트가 로그인하면, 애플리케이션 서버에서 DB를 조회해 사용자 정보를 제공합니다.

### 3️⃣ 데이터베이스 (Database, DB)
- 데이터를 저장하고 관리하는 시스템.
- **RDBMS (MySQL, PostgreSQL, Oracle) ➞** 강력한 데이터 무결성 보장.
- **NoSQL (MongoDB, Redis, Cassandra) ➞** 대량의 데이터 처리에 적합.
- **샤딩(Sharding), 레플리케이션(Replication)을** 활용해 성능과 안정성을 높임.

### 4️⃣ 캐시 시스템 (Cache System)
- 자주 조회되는 데이터를 빠르게 제공하기 위해 사용됩니다.
- 대표적인 캐시 기술
    - 1. **Redis**
    - 2. **Memcached**
- 예시: 인기 게시슬을 캐시에 저장해 DB 부하를 줄입니다.

### 5️⃣ 메시지 큐(Message Queue, MQ)
- 비동기 처리를 위해 사용됩니다.
- 대표적인 메시지 큐 시스템
    - 1. **Kafka**
    - 2. **RabbitMQ**
    - 3. **AWS SQS**
- 예시: 유저가 글을 작성하면, MQ에 메시지를 보내고 나중에 비동기로 처리함.

### 6️⃣ CDN (Content Delivery Network)
- 정적 콘텐츠 (이미지, 동영상)를 전 세계 여러 서버에 배포하여 빠르게 제공하는 기술.
- 대표적인 CDN 서비스
    - 1. **Clouldflare**
    - 2. **AWS CloudFront**
- 예시: 해외 사용자가 한국 서버에서 이미지 다운로드 시, 가까운 CDN 서버에서 제공해 속도를 높입니다.

### 7️⃣ 모니터링 & 로깅 시스템
- 서버 상태를 지속적으로 체크하고, 장애 발생 시 빠르게 대응할 수 있도록 합니다.
- 대표적인 모니터링 도구
    - 1. **Prometheus**
    - 2. **Grafana**
    - 3. **AWS CloudWatch**
- 대표적인 로깅 도구
    - 1. **ELK Stack (Elasticsearch, Logstach, Kibana)**
    - 2. **Fluentd**
- 예시: CPU 사용량이 80%를 초과하면 경고 알림을 발생시켜 장애를 예방합니다.

## ✅3️⃣ 대규모 시스템 설계의 핵심 개념
### ✅ 확장성 (Scalability)
- 시스템이 사용자 증가에 따라 성능 저하 없이 확장할 수 있는 능력
- **수평 확장(Scale-Out) :** 서버를 여러 대 추가
    - 예: AWS EC2 Auto Scaling
- **수직 확장(Scale-Up) :** 서버의 성능을 높이는 방법
    - 예: CPU, RAM 업그레이드

### ✅ 가용성 (Availability)
- 시스템이 지속적으로 동작할 수 있는 능력 (99.99% Uptime 보장)
- 예시: 장애 발생 시, 다른 서버가 대신 처리하도록 **Failover** 설계

### ✅ 복원력 (Resilience)
- 시스템이 장애 발생 후 빠르게 복구하는 능력
- 예시: AWS RDS Multi-AZ 구성 ➞ 장애 발생 시 자동으로 대체 DB로 전환

### ✅ 일관성 (Consistency) vs 가용성 (Availability) vs 분할 내성 (Partition Tolerance)
- **CAP 정리 :** 분산 시스템에서는 **일관성 (Consistency), 가용성 (Availability), 분할 내성 (Partition Tolerance)** 중 두 가지만 선택 가능
- 예시: **은행 시스템 ➞ 일관성 (Consistency) 우선**
- 예시: **SNS 서비스 ➞ 가용성 (Availability) 우선**

### ✅ 마이크로서비스 아키텍처 (MSA, Microservice Architecture)
- 하나의 거대한 서비스(모놀리식)를 작은 서비스 여러 개로 분리하는 방식
- 장점
    - 확장성 증가
    - 독립 배포 기능
    - 장애가 전체 시스템에 영향을 덜 줌
- 예시
    - 사용자 인증, 결제, 주문, 리뷰 서비스를 각각 독립적인 서비스로 나눔

## ✅4️⃣ 대규모 시스템 설계 사례
### 💎 쇼핑몰 (이커머스) 시스템
- 웹 서버 (Nginx)
- 애플리케이션 서버 (Spring Boot)
- DB (MySQL + Redis 캐시)
- 메시지 큐 (Kafka) ➞ 주문 처리
- CDN ➞ 이미지 및 정적 리소스 제공

### 💎 게임 서버 (배틀그라운드, LOL)
- 게임 서버 (C++, Java)
- 매칭 서버 ➞ 유저 매칭
- 랭킹 시스템 (Redis)
- 로그 수집 및 분석 (Elasticsearch)

### 💎 금융 서비스 (토스, 카카오뱅크)
- 강력한 보안 및 트랜잭션 무결성
- **CQRS 패턴 적용 (읽기와 쓰기 분리)**
- **이중화된 DB 및 Failover 시스템 구축**

## ✅5️⃣ 대규모 시스템 설계 시 고려할 점
- 📌 **트래픽 급증에 대비한 Auto Scaling 설계**
- 📌 **DB 부하를 줄이기 위한 캐시 적용 (Redis, Memcached)**
- 📌 **장애 발생 대비를 위한 로드 밸런서 및 이중화 (Failover 구성)**
- 📌 **비동기 처리를 위한 메시지 큐 도입 (Kafka, RabbitMQ)**
- 📌 **빠른 장애 탐지를 위한 모니터링 시스템 구축**

## 💡 결론
- 대규모 시스템 서버 인프라는 **트래픽 분산, 확장성, 장애 대응**이 **핵심**입니다.
    - 백엔드 개발자로서 **로드 밸런싱, DB 성능 최적화, 메시지 큐, 캐시 시스템** 같은 요소를 깊이 이해하는 게 중요합니다.
