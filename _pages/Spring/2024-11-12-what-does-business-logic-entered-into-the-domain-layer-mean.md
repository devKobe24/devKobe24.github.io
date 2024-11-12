---
title: 🍃[Spring] '도메인 계층에 비즈니스 로직이 들어갔다'의 의미는 무엇인가요?
tags:
    - Spring
    - Framework
date: "2024-11-12"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] '도메인 계층에 비즈니스 로직이 들어갔다'의 의미는 무엇인가요?
- **애플리케이션의 핵심적인 비즈니스 로직을 도메인 객체(주로 엔티티나 값 객체)에 포함시켰다는 것을 의미합니다.**
    - 도메인 객체에 비즈니스 로직이 들어가면, 객체 지향 설계에서 **객체 스스로 자신의 상태와 행동을 관리하는 방식으로 시스템을 설계할 수 있습니다.**

## 1️⃣ 도메인 계층이란?
- 도메인 계층은 애플리케이션의 **핵심 비즈니스 로직을 표현하는 계층입니다.**
- 애플리케이션의 복잡한 비즈니스 로직과 규칙을 포함하며, 보통 엔티티, 값 객체, 에그리게이트 등의 도메인 객체로 구성됩니다.

> 🙋‍♂️ **에그리게이트(Aggregate)**
> 
> 도메인 주도 설계(Domain-Driven Design, DDD)에서 사용하는 개념으로, **밀접하게 연관된 객체들을 하나의 단위로 묶어 관리하는 그룹**입니다.
> 에그리게이트(Aggregate)는 **하나의 일관성 있는 변경 단위**로 다뤄지며, 객체들의 집합이 단일한 도메인 개념을 표현할 때 사용됩니다.

## 2️⃣ 비즈니스 로직이 도메인 계층에 위치하는 이유.
- 비즈니스 로직을 도메인 계층에 넣는 것은 **객체 지향 원칙에 맞는 설계 방식입니다.**
    - 객체가 자신의 상태를 스스로 관리하고, 필요한 작업도 스스로 수행하는 방식으로 설계하면 **높은 응집력을 유지할 수 있으며, 코드의 재사용성과 유지보수성도 향상됩니다.**
        - 예를 들어, Order라는 주문 엔티티가 있다고 가정할 때, 이 엔티티에 주문 추가, 총액 계산과 같은 비즈니스 로직을 포함시키면, 서비스 계층에서 단순히 데이터를 처리하기보다 Order 객체가 자신의 역할에 맞게 스스로 행동할 수 있게 됩니다.

## 3️⃣ 예제: 비즈니스 로직이 도메인 계층에 포함된 경우.
- 아래 예제에서는 Order 엔티티가 주문과 관련된 비즈니스 로직을 스스로 수행하도록 구현하였습니다.
```java
@Entity
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @OneToMany(mappedBy = "order", cascade = Cascade = CascadeType.ALL, orphanRemoval = true)
    private List<OrderItem> items = new ArrayList<>();
    
    private LocalDateTime orderDate;
    
    public void addItem(OrderItem item) {
        items.add(item);
        item.setOrder(this);
    }
    
    public void removeItem(OrderItem item) {
        item.remove(item);
        item.setOrder(null);
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
        this.orderDate = LocalDateTime.now();
    }
    // getter, setter
}
```

## 4️⃣ 설명.
- **addItem 메서드 :** Order 객체가 스스로 주문 항목을 추가하는 로직을 갖고 있습니다.
- **calculateTotalPrice 메서드 :** 주문 항목들의 총합을 계산하는 비즈니스 로직이 Order 객체에 포함되어 있습니다.
- **completeOrder 메서드 :** 주문을 완료할 때의 비즈니스 규칙(예: 주문 항목이 없으면 오류)을 Order 객체가 스스로 처리합니다.
    - 위와 같이 **도메인 객체에 비즈니스 로직을 포함**하면, 서비스 계층에서는 Order 객체의 상태를 직접 변경하거나 처리하는 대신, Order 객체에 필요한 행동을 요청할 수 있습니다.

## 5️⃣ 도메인 계층에 비즈니스 로직을 넣는 장점.

### 1️⃣ 응집성 강화.
- 데이터와 비즈니스 로직이 도메인 객체 내부에 함꼐 위치하여 서로 밀접하게 관리됩니다.

### 2️⃣ 코드의 재사용성.
- 비즈니스 로직이 도메인 계층에 있으면, 다른 서비스에도 해당 객체의 기능을 재사용할 수 있습니다.

### 3️⃣ 유지보수 용이성.
- 비즈니스 로직이 한 곳에 집중되므로, 코드의 유지보수가 더 쉬워집니다.

### 4️⃣ 풍부한 도메인 모델.
- 객체가 데이터를 단순히 담는 것에 그치지 않고 스스로 의미 있는 역할을 하며, 모델 자체가 더 직관적이고 이해하기 쉬워집니다.

## 5️⃣ 요약.
- "도메인 계층에 비즈니스 로직이 들어갔다"는 의미는 애플리케이션의 비즈니스 로직이 서비스 계층이나 별도의 유틸리티 클래스가 아니라 **도메인 객체 내부에 위치하여 객체가 스스로 비즈니스 규칙을 관리하고 수행한다는 것을 의미합니다.**
    - 이는 응집성과 재사용성을 높이며, 객체 지향적인 설계 원칙에 따라 시스템을 설계하는 방식입니다.
