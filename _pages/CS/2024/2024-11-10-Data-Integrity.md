---
title: "💾 [CS] 데이터 정합성(Data Integrity)이란 무엇일까요?"
tags:
    - CS
date: "2024-11-10"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 데이터 정합성(Data Integrity)이란 무엇일까요?
- **데이터의 정확성, 일관성, 신뢰성**을 유지하는 것을 의미합니다.
- 데이터 정합성(Data Integrity)이 보장되면 **애플리케이션의 로직과 데이터베이스 간의 데이터가 서로 일관성 있게 유지되며, 데이터가 의도하지 않은 변경 없이 신뢰성 있게 관리됩니다.**
    - 이는 데이터베이스와 관련된 모든 시스템에서 매우 중요한 개념입니다.

## 1️⃣ 데이터 정합성(Data Integrity)이 중요한 이유.
- 데이터 정합성(Data Integrity)은 애플리케이션의 데이터가 정확하고, 오류 없이 유지될 수 있도록 하여, **올바른 데이터 기반으로 시스템이 운영되게 합니다.**
    - 만약 데이터 정합성(Data Integrity)이 깨진다면, 잘못된 데이터로 인해 애플리케이션이 잘못된 동작을 수행할 수 있으며, 이는 사용자에게 혼란을 주고, 시스템의 신뢰성을 떨어뜨릴 수 있습니다.

## 2️⃣ 데이터 정합성(Data Integrity)의 종류.
- 데이터 정합성(Data Integrity)는 크게 **정합성, 참조 정합성, 비즈니스 정합성**으로 나눌 수 있습니다.

### 1️⃣ 엔티티 정합성(Entity Integrity)
- 각 엔티티(테이블)의 **기본 키(Primary Key)는** 유일하고 중복되지 않으며 NULL이 될 수 없음을 보장합니다.
    - 예를 들어, User 테이블의 id 필드가 중복되지 않고 NULL 값이 없는 경우, 이는 엔티티 정합성이 유지된 것입니다.

### 2️⃣ 참조 정합성(Referential Integrity)
- 데이터베이스에서 **외래 키(Foreign Key)를** 통해 연관된 테이블 간의 관계가 일관되게 유지됨을 의미합니다.
    - 예를 들어, Order 테이블이 User 테이블의 외래 키로 user_id를 가지는 경우, 모든 Order가 존재하는 user_id를 가져야 참조 정합성이 유지됩니다.
        - 만약 User 테이블에서 삭제된 사용자의 user_id가 Order 테이블에 남아 있으면 참조 정합성이 깨진 것입니다.

### 3️⃣ 비즈니스 정합성(Business Integrity)
- 비즈니스 로직에 따라 특정 조건들이 일관되게 유지됨을 보장합니다.
    - 즉, **비즈니스 요구 사항에 맞게 데이터가 정확히 반영**되는 것을 의미합니다.
        - 예를 들어, Account 테이블에서 계좌 잔고가 음수로 내려가지 않도록 하는 규칙이 있다면, 이 규칙을 지켜야 비즈니스 정합성이 유지됩니다.

## 3️⃣ 데이터 정합성 보장 방법.
- 데이터 정합성을 유지하기 위해 다음과 같은 기법이 사용됩니다.
    - **1. 데이터베이스 제약 조건 :** 데이터베이스 수준에서 **PRIMARY KEY, FOREIGN KEY, UNIQUE, NOT NULL** 등의 제약 조건을 설정하여 정합성을 보장합니다.
    - **2. 트랜잭션 :** 트랜잭션은 **ACID(Atomicity, Consistency, Isolation, Durability)** 속성을 통해 데이터의 일관성을 유지합니다. 트랜잭션 내의 모든 작업이 성공적으로 완료되거나 모두 실패해야 데이터 정합성이 보장됩니다.
    - **3. 애플리케이션 로직 :** 비즈니스 정합성은 데이터베이스뿐만 아니라 애플리케이션 코드에서도 확인되어야 합니다. 예를 들어, 특정 규칙에 따라 데이터를 삽입하거나 업데이트하기 전에 로직을 통해 검증하는 방식입니다.

## 4️⃣ JPA에서의 데이터 정합성 유지.
- JPA를 사용할 때는 다음과 같은 방식으로 데이터 정합성을 유지할 수 있습니다.
    - **연관관계 주인 설정 :** 연관관계 주인을 올바르게 설정하여, 양방향 관계에서 데이터베이스에 데이터가 일관되게 반영되도록 합니다.
    - **Cascade와 Orphan 객체 처리 :** CascadeType.ALL과 orphanRemoval = true 설정을 통해 부모-자식 관계에서 데이터 정합성을 유지할 수 있습니다.
    - **트랜잭션 관리 :** 데이터의 삽입, 수정, 삭제 작업을 트랙잭션으로 묶어, 작업 도중 에러가 발생해도 정합성이 깨지지 않도록 합니다.

## 5️⃣ 요약.
- 데이터 정합성은 데이터의 정확성과 신뢰성을 유지하기 위해 필수적인 개념입니다.
    - 이를 보장하려면 데이터베이스 제약 조건, 트랜잭션 관리, 애플리케이션 로직을 통한 검증이 필요하며, JPA를 사용하는 경우 연관관계와 트랜잭션을 올바르게 관리해야 합니다.
