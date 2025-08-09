---
title: "📚[Backend Development] CRUD 각 기능별 Request/Response Body 원칙과 패턴 완벽 이해하기"
tags:
    - Backend Ddevelopment
    - HTTP
    - RESTful API
    - API
    - Architecture
    - CRUD
date: "2025-08-09"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 🌏 CRUD 각 기능별 Request/Response Body 원칙과 패턴 완벽 이해하기

> **왜 이 문서를 작성했나? 🤔**
> API 명세서 작성 과정에서 CRUD의 Request / Response Body 예시를 구현하면서, Request / Response Body에 대한
> 작성 원칙과 패턴을 제대로 알지 못하여 정리한 학습 노트입니다.

## 🎯 핵심 목표.

**CRUD의 Request / Response Body 작성 원칙과 방법을 완벽히 이해하자!**

## 🪵🦶 목차

1. CRUD의 Request / Response Body 작성 원칙.
2. CRUD의 Request / Response Body 작성 방법.

## 📦 CRUD의 Request / Response Body 작성 원칙

API 명세서의 Body는 **'최소한의 정보로 명확하게 소통한다'** 는 대원칙을 따릅니다.
각 HTTP Method의 역할에 따라 필요한 정보를 주고받도록 설계합니다.

- **C (Create - `POST`):**
    - **Request :** 
        - **새로운 리소스를 만드는 데 필요한 모든 정보**를 담습니다. 
        - 단, 서버가 자동으로 생성하는 값(ID, 생성일시 등)은 제외합니다.
    - **Response :** 
        - 생성된 리소스의 **완전한 상태**를 반환하여, 클라이언트가 ID나 서버 생성 값을 다시 요청할 필요가 없게 합니다.

- **R (Read - `GET`):**
    - **Request :** 
        - **Body를 사용하지 않습니다.** 
        - 모든 조건은 URL 경로(Path Variable)나 쿼리 파라미터(Query Parameter)로 전달합니다.
    - **Response :** 
        - 조회된 리소스의 정보를 담습니다.
        - **단일 리소스는 객체(`{}`), 목록은 배열(`[]`)** 로 반환합니다.

- **U (Update - `PATCH` / `PUT`):**
    - **Request :**
        - **변경하려는 데이터만 담습니다.**
        - `PATCH`는 변경할 필드만, `PUT`는 리소스 전체를 보냅니다.
    - **Response :**
        - 수정이 완료된 리소스의 **완전한 상태**를 반환하여, 클라이언트가 수정 결과를 명확히 알 수 있게 합니다.

- **D (Delete - `DELETE`):**
    - **Request :**
        - **Body를 사용하지 않습니다.**
        - 삭제할 대상은 URL 경로로 식별합니다.
    - **Response :**
        - 성공적으로 삭제되었음을 알리기 위해 **Body를 비워두는 것이 원칙입니다.** (상태 코드 `204 No Content` 사용)

## 📦 CRUD의 Request / Response Body 작성 방법

위 원칙에 따라 예시인 '상품 관리 API'의 각 Body를 어떻게 작성하는지 보여드리겠습니다.

**C: 상품 등록 (`POST /products`)**
- **Request Body:**
    - **목적:** 
        - 신규 상품 정보와 초기 재고 정보를 전달합니다.
    - **내용:** 
        - `productName`, `productSaleCost`, `initialStockCount`, `expirationDate` 등 사용자가 직접 입력해야 하는 모든 필드를 포함합니다.
        - `productId`는 포함하지 않습니다.

```JSON
{
    "productName": "신선한 목장 우유 1L",
    "productSaleCost": 2500,
    "initialStockCount": 5,
    "expirationDate": "2025-08-18"
}
```

- **Response Body:**
    - **목적:**
        - 서버에서 생성된 `productId`, `createdAt`과 계산된 `margin` 등을 포함한 완전한 상품 정보를 클라이언트에게 알려줍니다.
    - **내용:**
        - `productId`를 포함한 모든 필드와 계산된 값을 담습니다.

```JSON
{
    "productId": "8801234567890",
    "productName": "신선한 목장 우유 1L",
    "totalStockCount": 8,
    "createdAt": "2025-08-09",
    "margin": 25.5,
    ...
}
```

**R: 상품 검색 (`GET /products?name=...`)**
- **Request Body:**
    - 사용하지 않습니다.
- **Response Body:**
    - **목적:**
        - 검색 조건에 맞는 상품 목록을 전달합니다.
    - **내용:**
        - 각 상품의 요약 정보를 담은 객체등의 배열(`[]`) 형태입니다.
        - 결과가 없으면 빈 배열(`[]`)을 반환합니다.

```JSON
[
    { "productId": "8801234567890", "productName": "신선한 목장 우유 1L", ...},
    { "productId": "8801234145725", "productName": "진라면 순한맛 1ea", ... },
]
```

**U: 상품 수정 (`PATCH /products/{productId}`)**
- **Request Body:**
    - **목적:**
        - 변경할 정보만 전달하여 효율성을 높입니다.
    - **내용:**
        - `productSaleCost`, `productDiscountRate` 등 변경이 필요한 필드만 포함합니다.

```JSON
{
    "productSaleCost": 3200
    "productDiscountRate": 20
}
```

- **Response Body:**
    - **목적:**
        - 수정이 반영된 최종 결과를 클라이언트가 확인할 수 있게합니다.
    - **내용:**
        - 수정된 필드뿐만 아니라, 그로 인해 함께 변경된 계산값(`margin` 등)까지 포함한 상품의 전체 정보를 담습니다.

```JSON
{
    "productId": "8801234567890",
    "productSaleCost": 3200,
    "productDiscountRate": 20,
    "margin": 31.2 // 재계산된 값
    ...
}
```

**D: 상품 삭제 (`DELETE /products/{productId}`)**
- **Request Body:**
    - 사용하지 않습니다.
- **Response Body:**
    - **비어 있습니다.**
    - 삭제 성공 여부는 HTTP 상태 코드(`204 No Content`)로 전달합니다.
