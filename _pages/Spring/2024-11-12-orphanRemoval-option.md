---
title: 🍃[Spring] JPA에서의 orphanRemoval 옵션
tags:
    - Spring
    - Framework
date: "2024-11-12"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] JPA에서의 orphanRemoval 옵션.
- orphanRemoval 옵션은 JPA에서 부모 엔티티와의 연관관계가 끊어진 **Orphan Object(고아 객체)를 자동으로 삭제하는 기능을 제공합니다.**
- 부모 엔티티에서 자식 엔티티와의 관계를 제거할 때, 데이터베이스에서도 자동으로 해당 자식 엔티티가 삭제되도록 하는 옵션입니다.

## 1️⃣ orphanRemoval 사용 예시.
- 부모 엔티티 Order와 자식 엔티티 OrderItem 간의 **1:N 관계**에서 orphanRemoval = true를 설정하면, Order와의 관계가 끊긴 OrderItem 객체는 데이터베이스에서 자동으로 삭제됩니다.
```java
@Entity
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String customerName;
    
    @OneToMany(mappedBy = "order", cascade = Cascade = CascadeType.ALL, orphanRemoval = true)
    private List<OrderItem> orderItems = new ArrayList<>();
    
    // 연관관계 편의 메서드
    public void addOrderItem(OrderItem item) {
        orderItems.add(item);
        item.setOrder(this);
    }
    
    public void removeOrderItem(OrderItem item) {
        orderItems.remove(item);
        item.setOrder(null); // 부모와의 관계를 끊음
    }
    
    // getter, setter
}

@Entity
public class OrderItem {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String productName;
    private int quantity;
    
    @ManyToOne
    @JoinColumn(name = "order_id")
    private Order order;
    
    // getter, setter
}
```

## 2️⃣ orphanRemoval 작동 방식
- Order 엔티티에서 orderItems 필드에 orphanRemoval = true를 설정했습니다.
- removeOrderItem() 메서드를 통해 orderItems 리스트에서 OrderItem 객체를 제거하고, OrderItem의 order 필드를 null로 설정해 부모와의 관계를 끊습니다.
- 이때 orphanRemoval = true로 설정되어 있기 때문에, JPA는 관계가 끊어진 OrderItem 객체를 **자동으로 데이터베이스에서 삭제합니다.**

## 3️⃣ 예제 코드 실행.
```java
Order order = new Order();
order.setCustomerName("Alice");

OrderItem item1 = new OrderItem();
item1.setProductName("Laptop");
item1.setQuantity(1);

OrderItem item2 = new OrderItem();
item2.setProductName("Mouse");
item2.setQuantity(2);

order.addOrderItem(item1);
order.addOrderItem(item2);

entityManager.persist(order); // Order와 OrderItem들이 함께 저장됨

order.removeOrderItem(item1); // Order와의 관계를 끊음
entityManager.merge(order); // item1은 데이터베이스에서 자동으로 삭제됨
```

## 4️⃣ orphanRemoval과 CascadeType.REMOVE의 차이점.
- CascadeType.REMOVE
    - 부모 엔티티가 삭제될 때 연관된 자식 엔티티를 삭제합니다.
- orphanRemoval = true
    - 부모와의 관계가 끊긴 Orphan Object(고아 객체)를 자동으로 삭제합니다.
        - 부모 엔티티가 삭제되지 않더라도 관계가 끊긴 자식 엔티티만 개별적으로 삭제됩니다.

## 5️⃣ orphanRemoval 사용 시 주의 사항.
- **1:N 관계**에서 자식 엔티티의 생명주기가 부모 엔티티에 종속될 때 사용합니다.
- 양방향 관계에서 orphanRemoval을 설정할 경우, 순환 참조나 예기치 않은 삭제가 발생하지 않도록 **양쪽 필드를 모두 관리해야 합니다.**
- 잘못 사용하면 의도치 않은 데이터 삭제가 발생할 수 있으므로, 부모 엔티티와 자식 엔티티의 관계가 확실히 종속적인 경우에만 사용해야 합니다.

## 6️⃣ 요약.
- orphanRemoval 옵션은 부모와의 연관관계가 끊긴 자식 엔티티(Orphan Object, 고아 객체)를 자동으로 삭제하는 기능입니다.
- CascadeType.REMOVE와는 다르게 부모 엔티티가 삭제되지 않아도 관계가 끊긴 자식 엔티티만 삭제할 수 있습니다.
