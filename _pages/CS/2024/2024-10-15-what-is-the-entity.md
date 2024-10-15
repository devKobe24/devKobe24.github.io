---
title: "💾 [CS] 엔티티(Entity)는 무엇일까요?"
tags:
    - CS
date: "2024-10-15"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 엔티티(Entity)는 무엇일까요?
- **엔티티(Entity)는** 객체 지향 프로그래밍(Object-Oriented Programming, OOP)과 데이터베이스 설계에서 모두 사용되는 개념으로, 특히 **JPA(Java Persistence API)에서** 자주 언급됩니다.
- 엔티티(Entity)는 **데이터베이스 테이블과 매핑되는 자바 클래스를 의미하며,** 데이터베이스의 각 **행(Row)이** 자바 클래스의 인스턴스(Instance, 객체)로 대응됩니다.

## 1️⃣ 엔티티의 정의.

### 1️⃣ 데이터베이스의 행(Record).
- 데이터베이스에서는 테이블이 여러 행(Record)을 가집니다.
    - 엔티티(Entity)는 그 테이블의 각 행(Record)을 자바 객체(Intance)로 변환됩니다.
- 예를 들어, `User` 테이블이 있다면, 테이블의 각 레코드는 `User` 엔티티 객체로 변환됩니다.

### 2️⃣ JPA에서의 엔티티.
- JPA(Java Persistence API)에서는 엔티티가 데이터베이스 테이블과 매핑되며, 이를 위해 클래스에 `@Entity` 어노테이션(Annotation)을 붙입니다.
- 엔티티(Entity) 클래스의 인스턴스(Instance, 객체)는 데이터베이스에서 하나의 행(Row, Record)을 나타내며, 그 필드는 테이블의 각 열(Column)에 매핑됩니다.

### 3️⃣ 객체 지향적 데이터 모델링.
- 엔티티(Entity)는 데이터베이스의 레코드(Record)를 단순히 자바 객체로 변환하는 것뿐만 아니라, 객체 지향 프로그래밍에(Object-Oriented Programming, OOP)에서의 **상태(속성)와 행동(메서드)을** 가질 수 있습니다.
    - 즐, 데이터와 그 데이터를 처리하는 메서드가 함께 정의됩니다.

## 2️⃣ 엔티티의 특징.

### 👉 클래스와 데이터베이스 테이블 매핑.
- 엔티티 클래스는 보통 하나의 데이터베이스 테이블과 매핑됩니다.

### 👉 필드와 열(Column) 매핑.
- 엔티티 클래스의 필드는 테이블의 열(Column)과 매핑됩니다.

### 👉 기본 키(Primary Key).
- 엔티티는 반드시 하나의 필드를 기본 키(Primary Key)로 지정해야 합니다.
    - 이 필드는 테이블에서 각 행(Row)을 고유하게 식별하는 데 사용됩니다.

### 👉 상태 관리.
- 엔티티는 JPA(Java Persistence API)가 관리하며, 엔티티의 상태(생성, 수정, 삭제)를 자동으로 데이터베이스에 반영할 수 있습니다.

## 3️⃣ 엔티티 클래스의 예.

```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;

@Entity // 이 클래스는 데이터베이스 테이블과 매핑되는 엔티티임을 나타냅니다.
public class User {
    
    @Id // 기본 키(Primary Key)를 지정.
    @GeneratedValue(strategy = Generation.IDENTITY) // 기본 키(Primary Key) 자동 생성 전략 설정.
    private Long id;
    
    private String name;
    private String email;
    
    // 기본 생성자.
    public User() {}
    
    // 생성자, getter 및 setter
    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }
    
    // Getter and Setter
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

## 4️⃣ 엔티티의 주요 요소.

### 1️⃣ `@Entity`
- 이 어노테이션은 클래스가 데이터베이스 테이블과 매핑된다는 것을 의미합니다.

### 2️⃣ `@Id`
- 엔티티 클래스의 필드 중 하나는 반드시 기본 키(Primary Key)로 지정되어야 하며, `@Id` 어노테이션을 사용합니다.

### 3️⃣ `@GeneratedValue`
- 기본 키(Primary Key)가 자동으로 생성되도록 설정할 수 있습니다.
    - `GenerationType.IDENTITY`는 데이터베이스가 자동으로 키를 증가시키도록 하는 전략입니다.

## 5️⃣ 엔티티의 장점.

### 👉 객체 지향 프로그래밍과 데이터베이스의 통합.
- 엔티티를 사용하면 데이터베이스의 테이블과 자바 객체를 일관된 방식으로 다룰 수 있어, 코드의 가독성과 유지보수성을 향상시킬 수 있습니다.

### 👉 자동화된 데이터베이스 작업.
- JPA와 같은 프레임워크는 엔티티의 상태를 추적하여, 데이터베이스에서 발생하는 작업(삽입, 갱신, 삭제)을 자동으로 처리해줍니다.

### 👉 데이터베이스 독립성.
- 엔티티를 사용하면 특정 데이터베이스에 종속되지 않고 다양한 데이터베이스에서 동일한 코드를 사용할 수 있습니다.

## 6️⃣ 요약.
- **엔티티(Entity)는** 데이터베이스 테이블과 매핑되는 자바 클래스이며, JPA(Java Persistence API)를 통해 객체 지향적인 방식으로 데이터베이스와 상호작용할 수 있게 해줍니다.
- 엔티티 클래스는 데이터베이스의 테이블을 자바 객체(Instance)로 표현하고, 테이블의 각 행(Row)을 엔티티 객체로 변환하여 데이터베이스 작업을 쉽게 처리할 수 있도록 도와줍니다.
