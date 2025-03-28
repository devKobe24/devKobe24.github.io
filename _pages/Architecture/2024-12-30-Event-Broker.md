---
title: 🏗️[Architecture] 이벤트 브로커(Event Broker)란 무엇일까?
tags:
    - Architecture
    - Software Architecture
    - Distributed System
date: "2024-12-30"
thumbnail: "/assets/img/thumbnail/architecture.jpg"
---

# 🏗️[Architecture] 이벤트 브로커(Event Broker)란 무엇일까?
## 📌 Intro.
- ↘︎ **이벤트 브로커(Event Broker)는 시스템이나 애플리케이션 산에 이벤트(Event)를 안전하게 전달하고 관리하는 중개자 역할을 하는 소프트웨어.**
    - ↘︎ 주로 **이벤트 중심 아키텍처(Event-Driven Architecture, EDA)에서 사용되며, 비동기식 통신과 시스템간의 느슨한 결합(Loose Coupling)을 지원함.**

## ✅1️⃣ 이벤트(Event)란?
- ↘︎ **이벤트(Event)는 시스템 내에서 발생한 특정 상황이나 변경 사항을 의미함.**
- ↘︎ 예를 들어, **사용자가 주문을 생성했을 때(Order Created), 상품 재고가 부족할 때(Stock Depleted)** 등이 이벤트(Event)임.

## ✅2️⃣ 이벤트 브로커의 역할.
- **1. 이벤트 수집(Event Ingestion)**
    - ↘︎ 다양한 소스(애플리케이션, 서비스 IoT 장치 등)로 부터 이벤트를 수집함.
- **2. 이벤트 라우팅(Event Routing)**
    - ↘︎ 특정 조건에 따라 적절한 대상(구족자, 서비스)에게 이벤트를 전달함.
- **3. 이벤트 큐잉(Event Queuing)**
    - ↘︎ 이벤트를 임시로 저장하여 이벤트 손실을 방지합니다.
- **4. 비동기 처리(Asynchronous Processing)**
    - ↘︎ 이벤트를 발행한 시스템과 소비하는 시스템이 독립적으로 작동합니다.
- **5. 확장성(Scalability)**
    - ↘︎ 많은 양의 이벤트를 동시에 처리할 수 있도록 지원합니다.
- **6. 내구성(Durability)**
    - ↘︎ 이벤트가 손실되지 않도록 안전하게 저장합니다.

## ✅3️⃣ 이벤트 브로커의 동작 방식.
- **1. 이벤트 발행(Publish)**
    - ↘︎ 이벤트 생산자(Producer)가 이벤트 브로커에 이벤트를 발행함.
- **2. 이벤트 저장(Store)**
    - ↘︎ 이벤트 브로커는 받은 이벤트를 저장하거나 일시적으로 보관함.
- **3. 이벤트 라우팅(Routing)**
    - ↘︎ 저장된 이벤트를 규칙에 따라 특정 구독자(Consumer)에게 전달함.
- **4. 이벤트 소비(Consume)**
    - ↘︎ 이벤트 소비자(Consumer)가 이벤트를 받아 처리함.

## ✅4️⃣ 이벤트 브로커(Event Broker) vs 메시지 브로커(Message Broker)

| 구분           | 이벤트 브로커(Event Broker)            | 메시지 브로커(Message Broker)  |
| -------------- | -------------------------------------- | ------------------------------ |
| 주요 개념      | 이벤트(Event)를 전달                   | 메시지(Message)를 전달         |
| 발생 시점      | 시스템에서 상태 변경이 발생할 때       | 시스템 간 통신이 필요할 때     |
| 모델           | 주로 Pub/Sub (퍼블리시-서브스크라이브) | 주로 P2P (포인트-투-포인트)    |
| 예시           | Apache Kafka, AWS EventBridge          | RabbitMQ, AWS SQS              |
| 주요 사용 사례 | 이벤트 스트림 처리, 실시간 분석        | 비동기 작업 처리, 큐 기반 통신 |

## ✅5️⃣ 이벤트 브로커의 주요 특징.
- **1. Publisher-Subscriber 패턴(Pub/Sub)**
    - ↘︎ 이벤트 발행자는 이벤트를 브로커에 전달하고, 구독자는 관심 있는 이벤트를 수신함.
- **2. 비동기 통신**
    - ↘︎ 이벤트가 브로커를 통해 전달되므로 시스템 간 동기화가 필요하지 않음.
- **3. 느슨한 결합(Loose Coupling)**
    - ↘︎ 생산자와 소비자는 서로 직접 연결되지 않고 브로커를 통해 통신함.
- **4. 이벤트 소싱(Event Sourcing)**
    - ↘︎ 모든 이벤트를 기록하고 필요할 때 재생할 수 있음.
- **5. 확장성 및 신뢰성**
    - ↘︎ 수백만 개의 이벤트를 처리할 수 있고, 이벤트 손실을 방지함.

## ✅6️⃣ 이벤트 브로커의 사용 사례.
- **1. 실시간 데이터 처리.**
    - ↘︎ 실시간 거래, 사용자 활동 로그 처리.
- **2. 마이크로서비스 아키텍처.**
    - ↘︎ 서로 독립된 서비스 간의 이벤트 전달.
- **3. IoT 데이터 스트림.**
    - ↘︎ IoT 장치로부터 대량의 데이터를 수집 및 분석.
- **4. 알림 시스템(Notification System).**
    - ↘︎ 이벤트 발생 시 사용자에게 알림 전송.
- **5. 데이터 파이프라인(Data Pipeline).**
    - ↘︎ 데이터 처리 단계 간 이벤트 전달.

## ✅7️⃣ 대표적인 이벤트 브로커 도구.

| 이벤트 브로커        | 주요 특징                         |
| -------------------- | --------------------------------- |
| Apache Kafka         | 대량의 데이터 스트림, 높은 처리량 |
| AWS EventBridge      | AWS 서비스 간 이벤트 통합         |
| Google Cloud Pub/Sub | 실시간 메시징 및 데이터 스트림    |
| Azure Event Hubs     | 대용량 데이터 수집 및 분석        |
| RabbitMQ             | 이벤트 및 메시지 큐 지원          |

## ✅8️⃣ 이벤트 브로커를 사용할 때의 고려사항.
- **1. 처리량(Throughput):**
    - ↘︎ 얼마나 많은 이벤트를 동시에 처리할 수 있는가?
- **2. 지연 시간(Latency):**
    - ↘︎ 이벤트가 얼마나 빠르게 전달되는가?
- **3. 신뢰성(Reliability):**
    - ↘︎ 이벤트가 손실되지 않는가?
- **4. 확장성(Scalability):**
    - ↘︎ 증가하는 이벤트 수를 처리할 수 있는가?
- **5. 보안(Security):**
    - ↘︎ 이벤트 데이터는 안전하게 보호되는가?

## ✅9️⃣ 이벤트 브로커 도입의 이점.
- **1. 시스템 간 느슨한 결합.**
- **2. 확장성 및 유연성.**
- **3. 고가용성(High Availability)**
- **4. 비동기 이벤트 처리**
- **5, 실시간 데이터 분석 및 처리**

## 🚀 결론
- **이벤트 브로커(Event Broker)는 시스템 간 이벤트를 안전하게 전달하고 라우팅하는 중앙 허브 역할을 함.**
- **Apache Kafka, AWS EventBridge, Google Cloud Pub/Sub** 등이 대표적인 이벤트 브로커.
- 이벤트 브로커를 도입함으로써 **확장성, 유연성, 신뢰성을 확보할 수 있음.**

## 🔑 핵심 요약
- **이벤트 브로커란? :** 이벤트 중심 통신을 중재하는 시스템.
- **주요 특징 :** 비동기 처리, 확장성, 신뢰성.
- **사용 사례 :** 실시간 데이터 처리, 마이크로 서비스 통신.
