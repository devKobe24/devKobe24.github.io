---
title: 🍃[Spring] 애그리게이트(Aggregate)란 무엇일까요?
tags:
    - Spring
    - Framework
date: "2024-11-13"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 애그리게이트(Aggregate)란 무엇일까요?
- **애그리게이트(Aggregate)는** 도메인 주도 설계(Domain-Driven Design, DDD)에서 사용하는 개념으로, **밀접하게 연관된 객체들을 하나의 일관성 있는 변경 단위로 묶어 관리하는 그룹을 의미합니다.**
- 애그리게이트(Aggregate)는 **하나의 도메인 개념을 표현하고, 객체 간의 관계를 캡슐화하여 일관된 상태를 유지할 수 있게 해줍니다.**

> 🙋‍♂️ 캡슐화(Encapsulation)
> 
> 객체 지행 프로그래밍의 중요한 개념 중 하나로, **객체의 데이터와 이를 조작하는 메서드를 하나의 단위로 묶어 외부에서 직접 접근하지 못하도록 숨기는 것을 의미합니다.**
> 캡슐화(Encapsulation)는 객체 내부의 세부 구현을 감추고, 외부에는 필요한 부분만 공개하여 객체의 일관성을 유지하고 코드의 복잡성을 줄이는 데 도움이 됩니다.

## 1️⃣ 애그리게이트(Aggregate)의 구성 요소.

### 1️⃣ 애그리게이트 루트(Aggregate Root)
- 애그리게이트(Aggregate)의 진입점이 되는 객체로, 애그리게이트(Aggregate) 외부에서 접근할 수 있는 유일한 엔티티입니다.
    - 외부에서는 루트 엔티티를 통해서만 애그리게이트(Aggregate) 내부에 접근할 수 있습니다.

### 2️⃣ 내부 엔티티와 값 객체들.
- 애그리게이트 루드(Aggregate Root)와 함께 애그리게이트(Aggregate)를 구성하는 객체들 입니다.
    - 애그리게이트 루트(Aggregate Root)가 이들을 포함하거나 참조하며, 애그리게이트(Aggregate)의 일관성을 책임집니다.

## 2️⃣ 애그리게이트(Aggregate)의 예시: 주문 시스템.
- 주문 시스템에서 **주문(Order)이라는 개념을 에그리게이트(Aggregate)로 정의할 수 있습니다.**
    - Order는 주문 항목들(OrderItem)을 포함하며, 이들을 한 단위로 묶어 주문과 관련된 모든 비즈니스 로직을 처리합니다.
```java
@Entity
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private LocalDateTime orderDate;
    private OrderStatus status;
    
    @OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
    private List<OrderItem> = items = new ArrayList<>();
    
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
    // getter, setter
}
```

## 3️⃣ 설명.
- Order는 **애그리게이트 루트(Aggregate Root)로,** 주문과 관련된 모든 상태와 동작을 책임집니다.
- OrderItem 객체는 Order 애그리게이트의 내부 구성 요소로, 외부에서는 OrderItem에 직접 접근할 수 없습니다.
- Order는 주문 항목을 추가하고 총 가격을 계산하며, 주문 완료 여부를 결정하는 로직을 포함합니다.

## 4️⃣ 애그리게이트(Aggregate)의 특징과 원칙.

### 1️⃣ 일관성 유지.
- 애그리게이트(Aggregate)는 하나의 트랜잭션 내에서 하나의 단위로 처리되어, 애그리게이트(Aggregate) 내부의 상태와 데이터가 항상 일관성을 유지합니다.

### 2️⃣ 단일 진입점.
- 외부에서는 애그리게이트 루트(Aggregate Root)를 통해서만 애그리게이트 내부 객체에 접근할 수 있으며, 루트 엔티티가 내부 객체들에 대한 관리 권한을 가집니다.

### 3️⃣ 단일 식별자.
- 애그리게이트(Aggregate) 전체를 고유하게 식별할 수 있는 식별자는 애그리게이트 루트(Aggregate Root)에만 존재하며, 시스템 전체에서 해당 식별자로 애그리게이트(Aggregate)를 구분합니다.

## 5️⃣ 애그리게이트(Aggregate) 사용의 장점.

### 1️⃣ 응집성.
- 연관된 데이터와 로직이 한 곳에 모여 비즈니스 로직이 명확하게 정의되며, 유지보수가 용이해집니다.

### 2️⃣ 일관성 보장.
- 애그리게이트 단위로 트랜잭션이 처리되므로 데이터 일관성을 보장할 수 있습니다.

### 3️⃣ 캡슐화.
- 애그리게이트 외부에서는 내부 객체에 직접 접근할 수 없고, 애그리게이트 루트를 통해서만 접근이 가능하므로 데이터와 로직이 안전하게 캡슐화됩니다.

## 5️⃣ 애그리게이트의 예제 상황.
- **주문(Order) 애그리게이트 :** 주문 항목(OrderItem)과 결제 정보(PaymentInfor) 등을 포함하여, 하나의 주문 단위로 묶음.
- **사용자(User) 애그리게이트 :** 사용자 정보와 사용자 프로필(Profile), 연락처(Contact) 등을 포함하여 사용자와 관련된 모든 정보를 하나로 묶음.

## 6️⃣ 요약.
- 애그리게이트는 밀접하게 연관된 객체들을 하나의 일관성 있는 변경 단위로 묶어 관리하는 개념입니다.
- 애그리게이트 루트를 통해서만 접근이 가능하도록 캡슐화하여 데이터와 비즈니스 로직의 응집성과 일관성을 유지하며, 객체 지향적인 방식으로 시스템을 모델링할 수 있게 합니다.
