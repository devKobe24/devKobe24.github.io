---
title: 🍃[Spring] JPA 어노테이션 - `@Id`
tags:
    - Spring
    - Framework
date: "2024-10-15"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] JPA 어노테이션 - `@Id`
- JPA 어노테이션 중 **`@Id`는 엔티티의 기본 키(Primary Key)를** 지정하는 데 사용되는 어노테이션(Annotation)입니다.
- 기본 키(Primary Key)는 데이터베이스 테이블에서 각 행(Row)을 고유하게 식별하는 데 사용되며, 모든 JPA(Java Persistence API) 엔티티는 반드시 하나의 필드를 기본 키(Primary Key)로 지정해야 합니다.

## 1️⃣ 주요 특징.

### 1️⃣ 기본 키(Primary Key) 지정.
- `@Id` 어노테이션을 통해 엔티티의 필드나 속성을 **기본 키(Primary Key)로** 지정합니다.
- 기본 키(Primary Key)는 데이터베이스 테이블에서 각 행(Row)을 고유하게 식별하는 값으로, 각 엔티티 인스턴스를 고유하게 식별하는 데 사용됩니다.

### 2️⃣ 단일 또는 복합 키.
- `@Id`는 **단일 필드를** 기본 키(Primary Key)로 지정할 때 사용되며, **복합 키(두 개 이상의 필드로 구성된 기본 키(Primary Key))를** 지정할 때는 `@IdClass` 또는 `@EmbeddedId` 어노테이션을 함께 사용할 수 있습니다.

### 3️⃣ 자동 생성.
- 기본 키(Primary Key)는 자동으로 생성될 수 있으며, 이를 위해 `@GeneratedValue` 어노테이션과 함께 사용합니다.
    - `@GeneratedValue`는 기본 키(Primary Key) 생성 전략을 정의합니다.(예: `AUTO`, `IDENTITY`, `SEQUENCE`, `TABLE`).

## 2️⃣ 예시.

### 1️⃣ 단순한 기본 키(Primary Key) 지정.
```java
import javax.persistence.Entity;
import javax.persistence.Id;

@Entity // 엔티티 클래스 선언
public class User {
    
    @Id // 기본 키로 지정
    private Long id; // 이 필드가 기본 키가 됨
    
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

- 위 예시에서 `id` 필드는 `@Id` 어노테이션으로 지정되어 기본 키(Primary Key)가 됩니다.
    - 기본 키(Primary Key)는 고유하게 엔티티를 식별하는 값입니다.

### 2️⃣ 기본 키 자동 생성.
```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;

@Entity
public class Product {
    
    @Id // 기본 키(Primary Key)로 지정.
    @GeneratedValue(strategy = GenerationType.IDENTITY) // 기본 키(Primary Key) 자동 생성/
    private Long productId; // 이 필드가 자동 생성되는 기본 키.
    
    private String name;
    private Double price;
    
    // 기본 생성자 및 Getter and Setter
    public Product () {}
    
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

- 이 예시에서는 `@GeneratedValue(strategy = GenerationType.IDENTITY)`를 사용하여 `productId`가 데이터베이스에서 자동으로 생성됩니다.
    - `IDENTITY` 전략은 데이터베이스가 자동으로 증가하는 값을 기본 키(Primary Key)로 사용하도록 합니다.

## 3️⃣ 기본 키(Primary Key) 생성 전략.
- `@GeneratedValue` 어노테이션과 함께 사용하여 기본 키(Primary Key)가 자동으로 생성되도록 설정할 수 있습니다.
    - 생성 전략은 네 가지가 있습니다.

### 1️⃣ AUTO
- JPA 구현체가 적절한 생성 전략을 자동으로 선택합니다.

### 2️⃣ IDENTITY
- 기본 키(Primary Key) 값을 데이터베이스가 자동으로 증가시켜 생성합니다(주로 MySQL에서 사용)

### 3️⃣ SEQUENCE
- 데이터베이스 시퀀스를 사용하여 기본 키(Primary Key) 값을 생성합니다.(주로 Oracle에서 사용).

> 📝 데이터베이스 시퀀스(Sequence)
> 
> 데이터베이스에서 **고유한 숫자 값을 순차적으로 생성하기 위해 사용되는 객체입니다.**
> 주로 기본 키(Primary Key)나 다른 고유 식별자를 자동으로 생성하는 용도로 사용됩니다.
> 시퀀스(Sequence)는 특정 규칙에 따라 숫자를 생성하며, 자동으로 증가하는 숫자 값을 제공해 데이터베이스의 여러 행(Row)에 고유한 값을 할당할 수 있습니다.

### 4️⃣ TABLE
- 키 값을 저장하기 위한 별도의 테이블을 사용합니다.

## 4️⃣ 요약.
- **`@Id`는** JPA에서 엔티티의 기본 키(Primary Key)를 지정하는 어노테이션으로, 엔티티의 각 인스턴스를 고유하게 식별합니다.
- 기본 키(Primary Key)는 필수적으로 정의해야 하며, 필요에 따라 `@GeneratedValue`를 사용해 자동으로 생성할 수 있습니다.
