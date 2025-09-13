---
title: "📚[Backend Development] 대규모 시스템 아키텍처 핵심 요소"
tags:
    - Backend Ddevelopment
date: "2025-03-12"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 대규모 시스템 아키텍처 핵심 요소"
## ✅1️⃣ 로드 밸런서 (Load Balancer)
### 1️⃣ 로드 밸런서란?
- 로드 밸런서 (Load Balancer)는 클라이언트의 요청을 여러 서버로 분산하여 부하를 줄이고, 장애가 발생한 서버를 감지하여 트래픽을 자동으로 다른 서버로 우회하는 역할을 합니다.

### 2️⃣ 로드 밸런서 종류
- **L4 (Network Layer) 로드 밸런서 :** IP 주소와 포트 기반으로 트래픽을 분산 (예: AWS NLB, HAProxy)
- **L7 (Application Layer) 로드 밸런서 :** HTTP 요청을 분석하여 특정 URL이나 쿠키 정보 기반으로 분산 (예: Nginx, AWS ALB, Traefix)

### 3️⃣ 로드 밸런싱 방식
- **Round Robin :** 서버에 순차적으로 요청을 분배
- **Least Connections :** 현재 접속자가 가장 적은 서버로 분배
- **IP Hashing :** 클라이언트의 IP를 기반으로 특정 서버로 항상 연결 (세션 유지 필요할 때 사용)
- **Weighted Round Robin :** 서버 성능에 따라 가중치를 두고 트래픽을 분배

### 4️⃣ 예제
- **AWS ALB(Application Load Balancer)** 설정 시, 특정 URL 경로에 따라 다른 서버 그룹으로 요청을 보낼 수 있음.
    - `https://example.com/api/*` ➞ API 서버 그룹
    - `https://example.com/images/*` ➞ 이미지 서버 그룹

## ✅2️⃣ 웹 서버(Web Server)와 애플리케이션 서버(Application Server)
### 1️⃣ 웹 서버(Web Server)란?
- 웹 서버는 정적 콘텐츠(HTML, CSS, JavaScript)를 제공하는 역할을 하며, 대표적으로 **Nginxm Apache**가 있습니다.

### 2️⃣ 애플리케이션 서버(Application Server)란?
- 애플리케이션 서버는 비즈니스 로직을 처리하는 서버, 보통 **Spring Boot, Django, Express** 같은 프레임워크를 사용합니다.

### 3️⃣ 웹 서버(Web Server) + 애플리케이션 서버(Application Server) 연동 방식.
- **Reverse Proxy :** 웹 서버(Nginx)가 클라이언트 요청을 받고, 내부 애플리케이션 서버(Spring Boot)로 전달
- **예제**
```
lcation /api/ {
    proxy_pass http://localhost:8080/;
}
````

## ✅3️⃣ 데이터베이스 (Database, DB)
### 1️⃣ 데이터베이스 종류.
#### 📌1️⃣ RDBMS (Relational Database Management System)
- MySQL, PostgreSQL, Oracle
- 데이터 정합성(ACID 보장)이 중요할 때 사용

#### 📌2️⃣ NoSQL
- MongoDB, Cassandra, Redis
- 트래픽이 많고 빠른 읽기/쓰기 성능이 필요한 경우 사용

### 2️⃣ 데이터 분산 전략
- **샤딩(Sharding) :** 데이터를 여러 서버에 나눠서 저장
- **레플리케이션(Replication) :** 데이터를 여러 서버에 복제하여 장애 대비

### 3️⃣ 예제
- **MySQL Master-Slave Replication:**
    - Master DB: 쓰기 작업 처리
    - Slave DB: 읽기 작업 처리

## ✅4️⃣ 캐시 시스템 (Cache System)
### 1️⃣ 캐시(Cache)란?
- 자주 사용하는 데이터를 빠르게 제공하기 위해 메모리에 저장하는 기술

### 2️⃣ 캐시 저장소 종류
#### 📌1️⃣ 메모리 캐시.
- Redis
- Memcached

#### 📌2️⃣ CDN 캐시.
- Cloudflare
- AWS CloudFront

### 3️⃣ 캐시 정책
- **TTL(Time-To-Live) :** 일정 시간이 지나면 캐시 삭제
- **LRU(Least Recently Used) :** 가장 오래 사용되지 않은 데이터부터 삭제

### 4️⃣ 예제
- **Spring Boot + Redis 캐시 적용**
```java
@Cacheable(value = "userCache", key = "#userId")
public User getUserById(Long userId) {
    return userRepository.findById(userId).orElse(null);
}
```

## ✅5️⃣ 메시지 큐 (Message Queue, MQ)
### 1️⃣ 메시지 큐란?
- 비동기 처리를 위해 메시지를 저장하고 전달하는 시스템

### 2️⃣ 메시지 큐 종류.
- **Kafka :** 대량의 데이터 처리 가능, 로그 처리, 실시간 스트리밍 지원
- **RabbitMQ :** 빠른 메시지 큐잉, 트랜잭션 처리 가능
- **AWS SQS :** 관리형 메시지 큐 서비스

### 3️⃣ 예제.
- **Spring Boot + Kafka**
```java
@KafakaListener(topics = "order-topic", groupId = "order-group")
public void consume(String message) {
    System.out.println("Received Message: " + message);
}
```

## ✅6️⃣ CDN (Content Delivery Network)
### 1️⃣ CDN이란?
- 정적 콘텐츠(이미지, 동영상, CSS)를 전 세계 여러 서버에 배포하여 빠르게 제공하는 기술

### 2️⃣ CDN 동작 방식
- 1. 사용자가 웹사이트 접속
- 2. CDN 서버가 가장 가까운 위치에서 콘텐츠 제공
- 3. 원본 서버 부하 감소

### 3️⃣ 예제
- AWS ClouldFront를 사용하여 정적 콘텐츠 캐싱

## ✅7️⃣ 모니터링 & 로깅 시스템
### 1️⃣ 모니터링 시스템
- **Prometheus + Grafana :** 서버 메트릭 수집 및 시각화
- **AWS CloudWatch :** AWS 리소스 모니터링

### 2️⃣ 로깅 시스템
- **ELK Stack (Elasticsearch, Logstash, Kibana) :** 로그 수집 및 분석
- **Fluetd :** 경량 로그 수집기

### 3️⃣ 예제
- **Spring Boot + ELK**
```properties
logging.file.name=logs/app.log
```

## ✅8️⃣ 확장성 (Scalability)
### 1️⃣ 확장 방법
#### 📌1️⃣ 수직 확장 (Scale-Up)
- 서버 성능 업그레이드 (CPU, RAM 추가)
- 한계가 존재함

#### 📌2️⃣ 수평 확장 (Scale-Out)
- 서버 개수를 늘려 부하 분산
- 로드 밸런서를 활용

## ✅9️⃣ 장애 대응 (Fault Tolerance)
### 1️⃣ 장애 대응 기법
- **Failover :** 장애 발생 시 자동으로 대체 서버로 전환
- **Circuit Breaker 패턴 :** 서비스가 일정 횟수 이상 실패하면 자동으로 차단

### 2️⃣ 예제
- **Spring Cloud + Resilience4j Circuit Breaker**
```java
@CircuiteBreaker(name = "backendA", fallbackMethod = "fallback")
public String callService() {
    return restTemplate.getForObject("http://example.com/api", String.class);
}
```
