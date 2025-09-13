---
title: "📚[Backend Development] DTO(Data Transfer Object) 구현 4대 원칙"
tags:
    - Backend Ddevelopment
    - DTO
    - Layer
    - Architecture
date: "2025-08-07"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 📚[Backend Development] DTO(Data Transfer Object) 구현 4대 원칙

DTO(Data Transfer Object)는 **계층 간 데이터 전송을 위해 사용하는 객체**입니다.

DTO 구현 4대 원칙에 대해 알아보기 전에 DTO의 핵심 개념과 DTO를 사용하는 이유에 대해 간단하게 알아본 후 본격적으로 DTO 구현 4대 원칙에 대해 알아보도록 하겠습니다.

---

# 📝 목차

- DTO
    - 1. DTO 핵심 개념.
    - 2. DTO를 사용하는 이유.
- DTO 구현 4대 원칙
    - 1. 단순한 데이터 컨테이너여야 합니다.(Be a Simple Data Container)
    - 2. 불변(Immutable) 객체로 만드세요(Be Immutable).
    - 3. 엔티티와 철저히 분리하세요(Be Decoupled from the Entity)
    - 4. 목적에 따라 분리해서 만드세요(Be Purpost-Specific)

---

# 📦 DTO

## ✅ 1. DTO 핵심 개념.

DTO는 시스템의 내부 로직과 외부 인터페이스를 분리하기 위한 '데이터 상자' 또는 '데이터 운반용 객체'입니다.
복잡한 비즈니스 로직 없이, 오직 데이터를 담아 전달하는 용도로만 사용됩니다.

## ✅ 2. DTO를 사용하는 이유.

가장 중요한 이유는 **내부 데이터 모델(`Entity`)과 외부 API 계약(`DTO`)을 분리**하기 위함입니다.

- **관심사 분리** 
    - 데이터베이스와 직접 연결된 `Entity`를 외부에 그대로 노출하면 보안에 취약하고, 내부 구조가 변경될 때마다 API 명세 전체가 영향을 받게 됩니다. DTO는 API 명세에 필요한 데이터만 골라 담아 이 문제를 해결합니다.

- **유연성 및 안정성**
    - 데이터베이스 `Entity`의 구조가 변경되더라도 DTO를 사용하는 한 API 명세는 그대로 유지할 수 있어, 시스템이 훨씬 유연하고 안정적이게 됩니다.

- **보안**
    - `Entity`에 포함된 비밀번호와 같은 민감함 정보나, 외부에 불필요한 내부 데이터가 클라이언트에게 노출되는 것을 방지합니다.

---

# 📦 DTO 구현 4대 원칙

## ✅ 1. 단순한 데이터 컨테이너여야 합니다 (Be a Simple Data Container)

DTO의 유일한 역한은 **"데이터를 담아 계층(Layer) 간에 전달하는 것입니다."**
가격 계산이나 유효성 검증과 같은 **"비즈니스 로직(Business Logic)을 절대 포함해서는 안 됩니다."**
이러한 로직은 서비스 계층(Service Layer)의 책임입니다.

## ✅ 2. 불변(Immutable) 객체로 만드세요. (Be Immutable)

`setter`를 제공하지 않고, "생성자나 빌더(`@Builder`)를 통해 **생성 시점에만** 값을 할당하세요."
데이터가 여러곳으로 전달되는 동안 값이 변경될 위험을 원칙적으로 차단하여 시스템의 안정성을 크게 높입니다.

## ✅ 3. 엔티티와 철저히 분리하세요 (Be Decoupled from the Entity)

"DTO는 API의 외부 명세(계약)를, 엔티티는 내부 데이터베이스 구조를 나타냅니다."
DTO가 엔티티를 직접 참조하거나 의존해서는 안 됩니다.
이 원칙은 내부 구조가 변경되더라도 외부 API에 영향을 주지 않는 유연한 시스템을 만듭니다.

## ✅ 4. 목적에 따라 분리해서 만드세요 (Be Purpost-Specific)

하나의 거대한 DTO를 여러 곳에서 사용하는 대신, 각 API의 목적에 맞는 별개의 DTO를 만드세요.
예를 들어, '상품 생성 요청'에는 `ProductCreateRequestDto`를,'상품 목록 조회 응담'에는 `ProductListResponseDto`를 사용하는 것이 좋습니다.
이는 DTO를 명확하고 간결하게 유지시켜줍니다.
