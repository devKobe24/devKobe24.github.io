---
title: 🍃[Spring] JPA 어노테이션 - `@GeneratedValue`
tags:
    - Spring
    - Framework
date: "2024-10-16"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] JPA 어노테이션 - `@GeneratedValue`
- **`@GeneratedValue`는 JPA(Java Persistence API)에서 기본 키(Primary Key) 값을 자동으로 생성**할 때 사용하는 어노테이션입니다.
- 이 어노테이션은 엔티티(Entity)의 기본 키(Primary Key) 필드에 값을 자동으로 할당하는 방식을 정의하며, 데이터베이스에서 기본 키(Primary Key)가 생성되는 방법을 설정할 수 있습니다.

## 1️⃣ 주요 특징.

### 1️⃣ 자동으로 기본 키 값 생성.
- `@GeneratedValue`는 기본 키(Primary Key)에 수동으로 값을 할당하지 않고, 데이터베이스나 JPA(Java Persistence API) 구현체가 기본 키(Primary Key) 값을 자동으로 생성하도록 합니다.

### 2️⃣ 전략(GenerationType) 설정.
- `@GenerationType`는 `strategy` 속성을 사용하여, 기본 키 값(Primary Key)을 생성하는 방식을 설정할 수 있습니다.
- JPA(Java Persistence API)는 네 가지 **생성 전략**을 제공합니다.
    - **`AUTO`**
    - **`IDENTITY`**
    - **`SEQUENCE`**
    - **`TABLE`**

## 2️⃣ 생성 전략(GenerationType)

### 1️⃣ `GenerationType.AUTO`
- 기본 키(Primary Key) 생성 전략을 JPA(Java Persistence API) 구현체(예: Hibernate)가 자동으로 선택하도록 합니다.
- 데이터베이스에 맞는 최적의 방법을 JPA(Java Persistence API)가 결정합니다.
    - 일부 데이터베이스에서는 SEQUENCE 방식, 다른 데이터베이스에서는 IDENTITY 방식 등을 사용할 수 있습니다.

### 2️⃣ `GenerationType.IDENTITY`
- 기본 키 값이 **자동 증가하는 컬럼(Column,열)을** 사용하는 방식입니다.
- 데이터베이스가 직접 기본 키(Primary Key) 값을 생성합니다.
    - 예를 들어, MySQL에서는 `AUTO_INCREMENT`, SQL Server에서는 `IDENTITY` 컬럼(Column, 열)을 사용하여 값을 자동으로 증가시킵니다.
- `IDENTITY` 전략은 데이터베이스에 의존적이며, 즉각적으로 값이 생성됩니다(데이터가 삽입되기 전에 미리 알 수 없음).

### 3️⃣ GenerationType.SEQUENCE
- **시퀀스 객체를** 사용하여 기본 키 값을 생성합니다.
- 데이터베이스의 테이블을 통한 고유한 ID 값을 관리하며, 이 방식은 데이터베이스 독립적인 방식이지만 성능이 떨어질 수 있습니다.

## 3️⃣ `@GeneratedValue` 사용 예시.

### 1️⃣ AUTO 전략.
```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;

@Entity
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO) // 기본 키 생성 전략을 JPA가 자동으로 선택.
    private Long id;
    
    private String name;
    private String email;
    
    // 기본 생성자 및 Getter, Setter
    public User() {}
    
    public Long getId() {
        return id;
    }
    
    public void setId(Long id) {
        this.id = id;
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public String getEmail() {
        return email;
    }
    
    public void setEmail(String email) {
        this.email = email;
    }
}
```

### 2️⃣ IDENTITY 전략.
```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;

@Entity
public class Product {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // 데이터베이스가 자동 증가하는 방식으로 기본 키(Primary Key) 생성
    private Long productId;
    
    private String name;
    private Double price;
    
    // 기본 생성자 및 Getter, Setter
    public Product() {}
    
    public Long getProductId() {
        return productId;
    }
    
    public void setProductId(Long productId) {
        this.productId = productId;
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public Double getPrice() {
        return price;
    }
    
    public void setPrice(Double price) {
        this.price = price;
    }
}
```

### 3️⃣ SEQUENCE 전략.
```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.SequenceGenerator;

@Entity
public class Order {
    
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "order_seq") // 시퀀스
    @SequenceGenerator(name = "order_seq", sequenceName = "order_sequence", allocationSize = 1)
    private Long orderId;
    
    private String product;
    private int quantity;
    
    // 기본 생성자 및 Getter, Setter
    public Order() {}
    
    public Long getOrderId() {
        return orderId;
    }
    
    public void setOrderId(Long orderId) {
        this.orderId = orderId;
    }
    
    public String getProduct() {
        return product;
    }
    
    public void setProduct(String product) {
        this.product = product;
    }
    
    public int getQuantity() {
        return quantity;
    }
    
    public void setQuantity(int quantity) {
        this.quantity = quantity;
    }
}
```

- 위 코드에서는 `@SequenceGenerator`를 사용하여 시퀀스에 대한 세부 설정을 지정합니다.
    - `sequenceName`은 데이터베이스에서 사용할 시퀀스의 이름을 정의하고, `allocationSize`는 시퀀스 값을 미리 할당하는 크기를 지정합니다.

## 4️⃣ 요약.
- **`@GeneratedValue`는** JPA(Java Persistence API)에서 **기본 키(Primary Key)를 자동으로 생성할 때 사용하는 어노테이션입니다.**
- 생성 전략(`GenerationType`)에는 `AUTO`, `IDENTITY`, `SEQUENCE`, `TABLE`이 있으며, 각 전략은 기본 키(Primary Key)를 생성하는 방식에 따라 다르게 동작합니다.
- `AUTO`는 JPA(Java Persistence API)가 자동으로 전략을 선택하고, `IDENTITY`는 데이터베이스의 자동 증가 기능을 사용하며, `SEQUENCE`는 시퀀스 객체를 통해 기본 키(Primary Key)를 생성하고, `TABLE`은 별도의 테이블을 통해 고유 값을 관리합니다.
- 이를 통해 JPA는 기본 키 값을 쉽게 생성하고 관리할 수 있어, 개발자는 기본 키 생성에 대해 신경 쓰지 않아도 됩니다.
