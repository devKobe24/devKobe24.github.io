---
title: 🍃[Spring] 연관관계에서의 "응집성"이란 무엇일까요?
tags:
    - Spring
    - Framework
date: "2024-11-13"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 연관관계에서의 "응집성"이란 무엇일까요?
- 연관관계에서의 **응집성**은 **밀접하게 관련된 데이터와 비즈니스 로직이 하나의 단위로 모여 서로 강하게 결합**되어 있음을 의미합니다.
- JPA 연관관계를 통해 응집성을 높이면, 서로 관련 있는 엔티티들이 하나의 일관된 비즈니스 로직을 수행하고 상태를 관리할 수 있습니다.
    - 이를 통해 코드의 **재사용성과 유지보수성**이 향상됩니다.

## 1️⃣ 연관관계에서 응집성이 높은 설계의 예시.
- 응집성이 높은 설계는 주로 **애그리게이트(Aggregate)** 개념과 밀접한 관련이 있습니다.
    - 예를 들어, Order와 OrderITem이 연관된 엔티티라고 가정하면, Order 엔티티가 주문 항목 OrderItem들을 포함하고, 주문의 총 가격 계산, 항목 추가, 주문 완료와 같은 비즈니스 로직을 책임집니다.
        - 이러한 방식으로 연관관계가 응집성 있게 구성되면 관련 데이터와 로직이 **하나의 단위로 캡슐화**되며, **일관된 상태와 로직**을 유지할 수 있습니다.

```java
@Entity
public class Order {
    @Id
    @GeneratedValue(stratege = GenerationType.IDENTITY)
    private Long id;
    
    private LocalDateTime orderDate;
    private OrderStatus status;
    
    @OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
    private List<OrderItem> items = new ArrayList<>();
    
    // 비즈니스 로직을 통해 연관 관계의 응집성 유지
    public void addItem(OrderItem item) {
        items.add(item);
        item.setOrder(this);
    }
    
    public BigDecimal calculateTotalPrice() {
        return items.stream()
                    .map(OrderItem::getTotalPrice)
                    .reduce(BigDecimal.ZERO, BigDecimal::add);
    }
    
    public void completeOrder() {
        if (items.isEmpty()) {
            throw new IllegalStateException("주문 항목이 비어 있습니다.");
        }
        this.status = OrderStatus.COMPLETED;
    }
}
```

## 2️⃣ 예제의 응집성 설명.
- Order와 OrderItem의 밀접한 관계를 반영하여, Order 엔티티는 OrderItem들을 포함하고 관리합니다.
- Order 엔티티 내부에서 addItem, calculateTotalPrice, completeOrder 등 주문과 관련된 주요 비즈니스 로직을 처리함으로써 관련된 데이터와 로직이 한 곳에 모여 있습니다.
    - 이를 통해 **Order 객체가 주문과 관련된 상태와 행동을 일관되게 관리할 수 있으며, Order 객체 외부에서는 이 로직을 직접적으로 접근할 필요가 없습니다.**

## 3️⃣ 응집성 높은 설계의 장점.

### 1️⃣ 데이터의 일관성 보장.
- 연관된 엔티티들이 하나의 단위로 결합되어 있기 때문에, 잘못된 상태나 값이 개별적으로 변경되는 상황을 방지할 수 있습니다.

### 2️⃣ 비즈니스 로직의 응집성.
- 관련된 데이터와 로직이 하나의 단위에 묶여있어 비즈니스 로직이 더 명확하게 정의되며, 코드가 직관적이고 이해하기 쉬워집니다.

### 3️⃣ 유지보수성 향상.
- 관련 로직이 서로 모여 있으므로 수정 사항이 발생할 때, 해당 단위 내에서만 변경하면 되므로 유지보수가 쉬워집니다.

## 3️⃣ 요약.
- 연관관계에서의 응집성은 **관련된 데이터와 비즈니스 로직이 하나의 단위에 밀접하게 결합되어 있는 것을 의미합니다.**
    - 이를 통해 객체가 자신의 상태와 행동을 일관되게 관리할 수 있으며, 데이터 일관성과 유지보수성이 높아집니다.
- JPA에서의 응집성 높은 연관관계는 비즈니스 로직을 효율적으로 관리하고 시스템의 안정성을 향상시키는 데 도움이 됩니다.
