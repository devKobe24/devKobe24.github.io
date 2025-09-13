---
title: 💻[Code Review] `OrderItem` 생성자와 정적 팩토리 메서드에 대한 리뷰.
tags:
    - Code review
    - OOP
    - SOLID
date: "2025-09-05"
thumbnail: "/assets/img/thumbnail/codereview.jpg"
---

# 💻[Code Review] `OrderItem` 생성자와 정적 팩토리 메서드에 대한 리뷰.

## 🤔 OrderItem 생성자와 정적 팩토리 메서드에 대한 질문

### 📋 현재 코드 상황

#### 1. `@Builder` 어노테이션을 활용한 생성자
`OrderItem` 생성 시에는 내부의 **모든 필드**가 포함되어 있습니다.

```java
@Builder
public OrderItem(String orderItemId,
                 Order order,
                 Product product,
                 int quantity,
                 int orderPrice) {
    this.orderItemId = orderItemId;
    this.order = order;
    this.product = product;
    this.quantity = quantity;
    this.orderPrice = orderPrice;
}
```

#### 2. `createOrderItem` 정적 팩토리 메서드
하지만 정적 팩토리 메서드에서는 **`orderPrice` 파라미터가 누락**되어 있습니다.

```java
public static OrderItem createOrderItem(String orderItemId,
                                        Order order,     // [1] Order 객체를 파라미터로 받음
                                        Product product,
                                        int quantity) {  // [2] orderPrice는 내부에서 가져오므로 파라미터 제거
    
    // 재고 확인 로직
    if (product.getStockQuantity() < quantity) {
        throw new IllegalArgumentException("상품의 재고가 부족합니다.");
    }

    // OrderItem 생성
    OrderItem orderItem = OrderItem.builder()
        .orderItemId(orderItemId)
        .product(product)
        .quantity(quantity)
        .orderPrice(product.getProductRegularPrice())  // 🔍 여기서 내부적으로 설정
        .build();

    order.addOrderItem(orderItem);  // [3] Order에 연관관계 편의 메서드 호출
    return orderItem;
}
```

---

## ❓ 질문

**왜 `createOrderItem` 정적 메서드는 `orderPrice` 필드가 파라미터에서 빠져 있나요?**

- 생성자에서는 `orderPrice`를 외부에서 받고 있는데
- 정적 팩토리 메서드에서는 `product.getProductRegularPrice()`로 내부에서 자동으로 설정하고 있습니다

이렇게 설계한 이유나 의도가 무엇인지 궁금합니다.

---

## 💡 OrderItem 생성자와 정적 팩토리 메서드에 대한 답변

객체지향 설계의 핵심을 꿰뚫는 아주 좋은 질문입니다.

두 메서드의 파라미터가 다른 이유는 각자의 **역할과 책임이 다르기 때문**입니다. `createOrderItem` 정적 팩토리 메서드에서 `orderPrice`가 빠진 것은 의도된, 매우 중요한 설계입니다.

---

### 🎯 핵심: "누가 가격을 결정할 책임이 있는가?"

이 설계의 핵심 질문은 이것입니다:

> **"주문 항목(`OrderItem`)이 생성될 때, 그 주문 가격(`orderPrice`)은 누가 정해야 하는가?"**

**정답**: 외부(서비스 계층/Service Layer)가 아니라, **`OrderItem` 객체 스스로가 주문 시점의 `Product` 가격을 보고 결정해야 한다**

`createOrderItem` 메서드는 바로 이 책임을 수행하기 위해 `orderPrice`를 파라미터로 받지 않는 것입니다.

---

### 🔍 두 메서드의 역할 비교

#### 1. `@Builder`가 적용된 생성자

**역할**: 단순한 객체 생성 도구

```java
@Builder
public OrderItem(String orderItemId,
                 Order order,
                 Product product,
                 int quantity,
                 int orderPrice) {  // 🔴 외부에서 값을 받아야 함
    this.orderItemId = orderItemId;
    this.order = order;
    this.product = product;
    this.quantity = quantity;
    this.orderPrice = orderPrice;  // 🔴 그대로 할당만 함
}
```

**특징**:
- Lombok이 만들어주는 빌더는 **객체를 조립하는 역할**만 수행
- 어떠한 비즈니스 규칙도 포함되어 있지 않음
- 서비스 계층에서 `orderPrice`를 포함한 모든 값을 직접 계산해서 제공해야 함
- ⚠️ **위험**: 실수로 잘못된 가격을 넣을 수 있음

#### 2. `createOrderItem` 정적 팩토리 메서드

**역할**: 비즈니스 규칙이 포함된 '스마트' 생성자

```java
public static OrderItem createOrderItem(String orderItemId,
                                        Order order,
                                        Product product,
                                        int quantity) {  // 🟢 orderPrice 파라미터 없음
    
    // 🟢 비즈니스 규칙 1: 재고 확인
    if (product.getStockQuantity() < quantity) {
        throw new IllegalArgumentException("상품의 재고가 부족합니다.");
    }

    // 🟢 비즈니스 규칙 2: 주문 시점 가격 자동 설정
    OrderItem orderItem = OrderItem.builder()
        .orderItemId(orderItemId)
        .product(product)
        .quantity(quantity)
        .orderPrice(product.getProductRegularPrice())  // 🟢 내부에서 자동으로 설정
        .build();

    order.addOrderItem(orderItem);
    return orderItem;
}
```

**특징**:
- **재고 확인**: `product.getStockQuantity() < quantity` 로직으로 재고 검사
- **가격 결정**: 스스로 `Product`의 현재 가격을 조회하여 `orderPrice` 설정
- **안전성**: 서비스 계층의 실수 가능성을 원천적으로 차단
- **비즈니스 규칙 보장**: "주문 가격은 주문 시점의 상품 가격을 따른다"는 규칙을 객체 스스로가 보장

---

### 📊 실제 사용 예시 비교

#### ❌ Builder 직접 사용 시 (위험한 방법)
```java
// 서비스 계층에서 실수할 수 있는 예시
OrderItem orderItem = OrderItem.builder()
    .orderItemId("ORDER_ITEM_001")
    .product(product)
    .quantity(2)
    .orderPrice(5000)  // 🔴 실제 상품 가격이 10000원인데 잘못 입력!
    .build();
```

#### ✅ 정적 팩토리 메서드 사용 시 (안전한 방법)
```java
// 안전하고 올바른 방법
OrderItem orderItem = OrderItem.createOrderItem(
    "ORDER_ITEM_001",
    order,
    product,  // 가격은 product에서 자동으로 가져옴
    2
);
// 🟢 항상 정확한 상품 가격이 자동으로 설정됨
```

---

### 🏗️ 결론: 책임의 위임

#### `@Builder` = 조립 설명서 📋
- 단순히 부품(`orderItemId`, `product`, `quantity`, `orderPrice` 등)을 조립하는 역할
- 어떤 값을 넣을지는 외부에서 모두 결정해야 함

#### `createOrderItem` = 자동화된 생산 라인 🏭
- 재고 확인부터 가격 책정까지 모든 과정을 책임
- 최소한의 재료(`product`, `quantity`)만 넣어주면 완벽한 제품(`OrderItem`) 생산
- 복잡하고 중요한 로직을 객체 내부로 캡슐화

---

### ✨ 객체지향 설계의 핵심

이처럼 **객체 생성과 관련된 복잡하고 중요한 로직을 객체 내부로 숨기고(캡슐화)**, 외부에는 간단한 인터페이스(`createOrderItem`)만 제공하는 것이 바로 **객체지향 설계의 핵심**입니다.

이 설계를 통해 코드는:
- 🛡️ **더 안전**하고 (잘못된 데이터 입력 방지)
- 🔍 **더 명확**하며 (비즈니스 규칙이 명시적)
- 🔧 **더 유지보수하기 좋아집니다** (변경 시 영향 범위 최소화)
