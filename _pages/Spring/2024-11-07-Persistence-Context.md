---
title: 🍃[Spring] 영속성 컨텍스트(Persistence Context)
tags:
    - Spring
    - Framework
date: "2024-11-07"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 영속성 컨텍스트(Persistence Context)
- 영속성 컨텍스트(Persistence Context)는 **엔티티(Entity) 객체의 상태**를 관리하는 환경을 의미하며, **데이터베이스와의 세션** 또는 **영속성 관리 단위**라고도 볼 수 있습니다.
    - 이 개념은 **JPA(Java Persistence API)** 표준에서 정의되며, Spring Data JPA 같은 구현체들이 이를 따릅니다.

## 1️⃣ 영속성 컨텍스트(Persistence Context)의 주요 개념.

### 1️⃣ 엔티티의 생명주기 관리.
- 영속성 컨텍스트(Persistence Context)는 엔티티 객체의 상태를 **생명주기에 따라 관리**합니다.
    - 엔티티는 비영속(new/transient), 영속(persistent), 준영속(detached), 삭제(removed) 상태 중 하나일 수 있습니다.
- 영속성 컨텍스트(Persistence Context)에 의해 관리되는 엔티티는 영속 상태가 됩니다.
    - 영속 상태인 엔티티는 데이터베이스와의 동기화가 자동으로 이루어지며, 변경 사항이 자동으로 추적됩니다.

### 2️⃣ 1차 캐시 역할.
- 영속성 컨텍스트(Persistence Context)는 엔티티를 메모리에 캐시하여 **1타 캐시 역할**을 수행합니다.
    - 즉, 동일한 엔티티에 여러 번 접근할 때, 데이터베이스를 조회하지 않고 메모리에 있는 엔티티를 재사용합니다.
        - 이를 통해 성능을 최적화할 수 있습니다.

### 3️⃣ 변경 감지(Dirty Checking)
- 영속성 컨텍스트(Persistence Context)는 영속 상태의 엔티티 변경 사항을 추적합니다.
    - 트랜잭션이 종료될 때, 변경된 부분을 자동으로 데이터베이스에 반영합니다.
        - 이를 **변경 감지**라고 합니다.

### 4️⃣ 지연 로딩(Lazy Loading)
- 영속성 컨텍스트(Persistence Context)는 필요할 때까지 데이터를 가져오지 않고, 실제로 사용할 때 데이터베이스에서 데이터를 불러오는 **지연 로딩(Lazy Loading)을** 지원합니다.
    - 이는 성능 최적화에 유리합니다.

## 2️⃣ 영속성 컨텍스트(Persistence Context)의 이점.

### 1️⃣ 자동 동기화.
- 영속성 컨텍스트(Persistence Context)에 있는 엔티티는 자동으로 데이터베이스와 동기화되므로, 데이터베이스에 직접 저장하는 코드를 작성하지 않아도 됩니다.

### 2️⃣ 캐시 기능.
- 1차 캐시로 인해 동일한 엔티티에 대한 중복 조회를 방지하여 성능이 개선됩니다.

### 3️⃣ 트랜잭션 관리.
- 트랜잭션 단위로 엔티티 상태가 관리되므로, 안전하게 데이터베이스와의 일관성을 유지할 수 있습니다.

## 3️⃣ 예시.
```java
// 예를 들어, userRepository.findById(id)를 호출하면
// userRepository는 영속성 컨텍스트에 접근하여 엔티티를 반환합니다.
User user = userRepository.findById(id).orElseThrow();

// user의 이름을 수정하면, 이 변경 사항은 영속성 컨텍스트에 반영되고,
// 트랜잭션이 끝날 때 자동으로 데이터베이스에 반영됩니다.
user.setName("New Name");
```
- 위와 같이, 영속성 컨텍스트는 Spring Data JPA와 같은 JPA 구현테가 엔티티를 관리하고, 변경 사항을 데이터베이스에 반영하며, 트랜잭션 내에서 효율적으로 엔티티를 캐싱하고 동기화하도록 돕는 중요한 개념입니다.
