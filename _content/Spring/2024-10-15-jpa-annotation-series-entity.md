---
title: 🍃[Spring] JPA 어노테이션 - `@Entity`
tags:
    - Spring
    - Framework
date: "2024-10-15"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] JPA 어노테이션 - `@Entity`
- `@Entity`는 **JPA(Java Persistence API)에서 데아터베이스 테이블과 매핑되는 자바 클래스를** 정의할 때 사용하는 어노테이션(Annotation)입니다.
    - 이 어노테이션(Annotation)을 클래스에 붙이면 해당 클래스가 JPA(Java Persistence API) 엔티티(Entity)임을 나타내며, JPA가 이를 데이터베이스 테이블과 매핑하여 관리할 수 있게 됩니다.

> 📝 엔티티(Entity)
> 
> 객체 지향 프로그래밍(Object-Oriented Programming, OOP)과 데이터베이스(Database) 설계에서 모두 사용되는 개념으로, 특히 JPA(Java Persistence API)에서 자주 언급됩니다.
> 
> 엔티티(Entity)는 **데이터베이스 테이블과 매핑되는 자바 클래스를 의미하며, 데이터베이스의 각 행(Row)이 자바 클래스의 객체(Instance, 인스턴스)로 대응됩니다.**

## 1️⃣ 주요 특징.

### 1️⃣ 엔티티 클래스(Entity Class)
- `@Entity`가 선언된 클래스는 **엔티티(Entity)라고 하며,** 이는 데이터베이스 테이블에 대응하는 자바 클래스입니다.
- 엔티티 클래스의 인스턴스(Instance, 객체)는 데이터베이스 테이블의 각 행(Row)에 대응됩니다.
- 클래스는 반드시 기본 생성자(default constructor)를 가져야 하고, 기본 키(Primary Key)를 정의해야 합니다.

### 2️⃣ 테이블 매핑(Table Mapping)
- 클래스에 `@Entity` 어노테이션(Annotation)을 붙이면, JPA(Java Persistence API)는 해당 클래스를 데이터베이스의 테이블과 매핑합니다.
- 테이블의 이름은 기본적으로 클래스 이름과 동일하게 매핑되지만, `@Table` 어노테이션(Annotation)을 사용하여 테이블 이름을 명시적으로 지정할 수도 있습니다.

### 3️⃣ 기본 키(Primary Key)
- 엔티티 클래스는 반드시 하나의 필드를 **기본 키(Primary Key)로** 지정해야 하며, 이를 위해 `@Id` 어노테이션(Annotation)을 사용합니다.
- 기본 키(Primary Key)의 생성 전략은 `@GeneratedValue` 어노테이션(Annotation)을 통해 지정할 수 있습니다.

## 2️⃣ 사용 예시.
```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Table;

@Entity // 이 클래스는 JPA 엔티티임을 나타냄.
@Table(name = "users") // 매핑될 데이터베이스 테이블 이름을 지정.
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerarionType.IDENTITY) // 기본 키 생성 전략
    private Long id;
    
    private String name;
    private String email;
    
    // 기본 생성자 (필수)
    public User() {}
    
    // 생성자, getter, setter 등
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

## 3️⃣ 주요 요소.

### 1️⃣ `@Entity`
- 이 어노테이션은 JPA(Java Persistence API)에게 해당 클래스가 데이터베이스의 테이블과 매핑되는 엔티티(Entity)임을 알립니다.

### 2️⃣ `@Table`
- 테이블 이름을 명시적으로 지정하려면 `@Table(name = "테이블이름")`을 사용합니다.
- `@Table`을 사용하지 않으면 기본적으로 클래스 이름이 테이블 이름으로 사용됩니다.

### 3️⃣ `@Id`
- 해당 필드는 엔티티(Entity)의 **기본 키(Primary Key)로** 사용됩니다.
    - 모든 엔티티는 반드시 하나의 기본 키(Primary Key)를 가져야 합니다.

### 4️⃣ `@GeneratedValue`
- 기본 키(Primary Key) 값의 생성 전략을 지정합니다.
    - 예를 즐어, `GenerationType.IDENTITY`는 데이터베이스가 자동으로 기본 키(Primary Key) 값을 증가시키는 전략을 의미합니다.

## 4️⃣ 주의 사항.
- 엔티티 클래스(Entity Class)는 반드시 **기본 생성자(default constructor)가** 있어야 하며, `public` 또는 `protected` 접근 제어자를 가져야 합니다.
- 기본 키(Primary Key)를 `@Id` 어노테이션으로 반드시 지정해야 합니다.

## 5️⃣ 요약.
- `@Entity`는 JPA(Java Persistence API) 엔티티를 선언하는 어노테이션으로, 해당 클래스를 데이터베이스 테이블과 매핑합니다.
- 이를 통해 JPA는 엔티티를 자동으로 관리하고, 데이터베이스와 상호작용할 수 있게 됩니다.
