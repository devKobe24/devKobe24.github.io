---
title: 💻[Code Review] `OrderItem` 관련 클래스들에 대한 코드 리뷰.
tags:
    - Code review
    - OOP
    - SOLID
date: "2025-09-05"
thumbnail: "/assets/img/thumbnail/codereview.jpg"
---

# 💻[Code Review] `OrderItem` 관련 클래스들에 대한 코드 리뷰.

## 🏛️ 아키텍처 및 설계 (Architecture & Design)
**DDD(도메인 주도 설계)의 풍부한 도메인 모델(Rich Domain Model)** 을 완벽하게 구현한 모범적인 아키텍처입니다.

* **Domain Entity :** 핵심 비즈니스 로직과 데이터를 함께 관리
* **정적 팩토리 메서드 :** 복잡한 생성 로직을 캡슐화
* **자율적 객체 :** 객체가 스스로 자신의 상태와 행위를 책임
* **연관관계 관리 :** JPA를 활용한 효율적인 객체 관계 매핑

이러한 구조는 **SOLID의 단일 책임 원칙(SRP)** 과 **캡슐화(Encapsulation)** 원칙을 완벽하게 만족하며, 단순한 데이터 덩어리가 아닌 살아있는 객체로서의 역할을 수행합니다.

---

## ✅ 클래스별 상세 리뷰 (Detailed Class Review)

```java
package com.kobe.productmanagement.domain;

import com.kobe.productmanagement.common.BaseTimeEntity;
import jakarta.persistence.*;
import lombok.*;

@Entity
@Getter
@Setter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Table(name = "order_items")
public class OrderItem extends BaseTimeEntity {

	@Id
	@Column(length = 26)
	private String orderItemId;

	@ManyToOne(fetch = FetchType.LAZY)
	@JoinColumn(name = "order_id")
	private Order order;

	@ManyToOne(fetch = FetchType.LAZY)
	@JoinColumn(name = "product_id")
	private Product product;

	@Column(nullable = false)
	private int quantity; // 주문 수량

	@Column(nullable = false)
	private int orderPrice; // 주문 당시 가격

	@Builder
	public OrderItem(String orderItemId,
	                 Order order,
	                 Product product,
	                 int quantity,
	                 int orderPrice
	) {
		this.orderItemId = orderItemId;
		this.order = order;
		this.product = product;
		this.quantity = quantity;
		this.orderPrice = orderPrice;
	}

	public static OrderItem createOrderItem(String orderItemId,
	                                        Order order,
	                                        Product product,
	                                        int quantity
	) {
		if (product.getStockQuantity() < quantity) {
			throw new IllegalArgumentException("상품의 재고가 부족합니다.");
		}

		OrderItem orderItem = OrderItem.builder()
			.orderItemId(orderItemId)
			.order(order)
			.product(product)
			.quantity(quantity)
			.orderPrice(product.getProductRegularPrice()) // 생성 시점의 상품 가격을 사용
			.build();

		order.addOrderItem(orderItem);
		return orderItem;
	}

	public int getTotalPrice() {
		return getOrderPrice() * getQuantity();
	}
}
```

### 1. `OrderItem.java`(Domain Entity) - ⭐️ 핵심

* **OOP/SOLID :**
    * **가장 칭찬하고 싶은 부분!!**
    * `createOrderItem` **정적 팩토리 메서드**를 통해 복잡한 생성 로직을 완벽하게 캡슐화했습니다.
    * **"재고가 충분한지 확인"** 하는 비즈니스 규칙과 **"주문 시점의 상품 가격을 저장"** 하는 중요한 로직이 `OrderItem` 클래스 내부에 완벽하게 캡슐화되었습니다.
    * 이로써 서비스 계층은 `OrderItem`이 어떻게 생성되는지에 대한 복잡한 과정을 알 필요 없이, 단순히 `OrderItem.createOrderItem(...)`을 호출하여 일을 위임하기만 하면 됩니다.

* **객체의 자율성 :**
    * `getTotalPrice()` 메서드를 통해 `OrderItem`이 **자기 자신의 총금액을 스스로 계산**하도록 책임을 부여했습니다.
    * 이는 "데이터와 그 데이터를 사용하는 로직은 한곳에 있어야 한다"는 객체지향의 기본 원칙을 완벽하게 따른 것입니다.

* **데이터 정합성 :**
    * `orderPrice` 필드를 정확하게 포함하여, 상품 가격이 변동되더라도 과거 주문 내역의 데이터 정합성을 완벽하게 보장하고 있습니다.

* **JPA 최적화 :**
    * `@ManyToOne` 관계 설정과 `FetchType.LAZY`를 사용한 성능 최적화 등 JPA 엔티티로서의 기본 설계가 매우 훌륭합니다.

---

## 🚀 추가 개선 제안 (Enhancement Suggestions)

### **연관관계 편의 메서드 활용**

`Order`와 `OrderItem`은 양방향 관계를 맺고 있습니다. `OrderItem`이 생성될 때, 자신을 생성한 `Order`의 `orderItems` 리스트에 스스로를 추가하는 로직을 넣어주면 더욱 완벽한 구조가 됩니다.

```java
// domain/OrderItem.java
public static OrderItem createOrderItem(String orderItemId,
                                        Order order, // Order 객체를 파라미터로 받음
                                        Product product,
                                        int quantity) {
    if (product.getStockQuantity() < quantity) {
        throw new IllegalArgumentException("상품의 재고가 부족합니다.");
    }

    OrderItem orderItem = OrderItem.builder()
        .orderItemId(orderItemId)
        .product(product)
        .quantity(quantity)
        .orderPrice(product.getProductRegularPrice())
        .build();

    order.addOrderItem(orderItem); // Order에 연관관계 편의 메서드 호출
    return orderItem;
}

// domain/Order.java
public class Order extends BaseTimeEntity {
    // ...
    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL)
    private List<OrderItem> orderItems = new ArrayList<>();

    //==연관관계 편의 메서드==//
    public void addOrderItem(OrderItem orderItem) {
        orderItems.add(orderItem);
        orderItem.setOrder(this);
    }
}
```

---

## 🏆 총평

**매우 훌륭합니다 (Outstanding Implementation)**

* 단순히 데이터를 담는 것을 넘어, 자신의 상태와 관련된 비즈니스 로직까지 책임지는 **진정한 객체지향적 코드**를 작성하셨습니다.

* `createOrderItem` 정적 팩토리 메서드와 `getTotalPrice` 메서드는 이 클래스를 단순한 엔티티가 아닌, **잘 설계된 도메인 모델**로 만든 핵심적인 요소입니다.

* **DDD(도메인 주도 설계)의 '풍부한 도메인 모델(Rich Domain Model)'** 을 교과서적으로 구현한 모범적인 사례입니다.

* 이러한 설계 방식을 꾸준히 유지해 나가신다면, 유지보수가 용이하고 확장 가능한 훌륭한 시스템이 될 것입니다.
