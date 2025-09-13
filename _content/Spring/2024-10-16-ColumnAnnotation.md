---
title: 🍃[Spring] JPA 어노테이션 - `@Column`
tags:
    - Spring
    - Framework
date: "2024-10-16"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] JPA 어노테이션 - `@Column`

- **`@Column`은 JPA(Java Persistence API)에서** 사용되는 어노테이션으로, 엔티티(Entity) 클래스의 필드를 데이터베이스의 **테이블 컬럼(Column, 열)에** 매핑할 때 사용됩니다.
    - 즉, 자바 클래스의 필드와 데이터베이스 테이블의 특정 컬럼(Column, 열) 간의 매핑을 정의하는 역할을 합니다.

> 🙋‍♂️[엔티티(Entity)는 무엇일까요?](https://www.devkobe24.com/CS/2024/2024-10-15-what-is-the-entity.html)

## 1️⃣ 주요 특징.

### 1️⃣ 컬럼(Column, 열)과 필드 매핑.
- `@Column` 어노테이션은 엔티티(Entity) 클래스의 필드가 데이터베이스 테이블의 어느 컬럼(Column, 열)과 매핑될지를 명시적으로 지정합니다.
- 매핑되는 컬럼(Column)의 이름, 길이 nullable 여부, 고유 제약조건 등을 설정할 수 있습니다.

### 2️⃣ 옵션 설정.
- `@Column`을 사용하여 컬럼(Column)의 속성(길이, nullable 여부 등) 세밀하게 제어할 수 있습니다.
    - 만약 `@Column`을 생략하면 JPA(Java Persistence API)가 자동으로 필드 이름을 컬럼(Column, 열) 이름으로 사용하고, 기본값으로 처리합니다.

## 2️⃣ 주요 속성.

### 👉 name
- 매핑할 데이터베이스 컬럼(Column, 열)의 이름을 명시적으로 지정합니다.
    - 지정하지 않으면, 필드 이름이 그대로 컬럼(Column, 열) 이름으로 사용됩니다.

### 👉 nullable
- 컬럼(Column, 열)이 `NULL` 값을 허용하는지 여부를 설정합니다.
    - 기본값은 `true`로, `nullable = false`로 설정하면 `NOT NULL` 제약조건이 걸립니다.

### 👉 unique
- 컬럼(Column, 열)에 **고유 제약조건을 설정할 수 있습니다.**
    - 기본값은 `false`이며, `unique = true`로 설정하면 해당 컬럼(Column, 열)에 고유한 값만 저장될 수 있습니다.

### 👉 length
- 문자열 컬럼(Column, 열)의 최대 길이를 설정합니다.
    - 기본값은 255입니다.
    - 주로 `VARCHAR` 타입 컬럼(Column, 열)에서 사용됩니다.

### 👉 precision
- 소수점이 포함된 숫자(예: `BigDecimal`)의 정밀도를 지정합니다.
    - `precision`은 소수점 앞의 전체 자릿수를 의미합니다.

### 👉 scale
- 소수점 이하 자릿수를 설정합니다.
    - `scale`은 소수점 이하의 자릿수를 의미합니다.

## 3️⃣ 예시.

### 1️⃣ 기본적인 사용.
```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Column;

@Entity
public class User {
    
    @Id
    private Long id;
    
    // 매핑할 데이터베이스 컬럼의 이름을 "user_name"으로 지정.
    // NOT NULL 제약 조건을 걸음.
    // 문자열 컬럼의 최대 길이를 100으로 설정.
    @Column(name = "user_name", nullable = false, length = 100) 
    private String name;
    
    // 고유 제약조건을 설정.
    // 고유한 값만 저장될 수 있음.
    @Column(unique = true)
    private String email;
    
    // 기본 생성자, getter, setter
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

- `@Column(name = "user_name", nullable = false, length = 100)`
    - 이 어노테이션은 `name` 필드를 데이터베이스의 `user_name` 컬럼에 매핑합니다.
    - 이 컬럼은 **NOT NULL** 제약조건이 적용되며, 최대 길이는 100자로 제한됩니다.
- `@Column(unique = true)`
    - 이 어노테이션은 `email` 필드에 **고유 제약조건**을 설정하여, `email` 값이 유일하도록 보장합니다.

### 2️⃣ `precision`과 `scale` 사용 예시(소수점 처리)
```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Column;
import javax.persistence.BigDecimal;

@Entity
public class Product {
    
    @Id
    private Long id;
    
    @Column(precision = 10, scale = 2)
    private BigDecimal price;
    
    // 기본 생성자, getter, setter
    public Product() {}
    
    public Long getId() {
        return id;
    }
    
    public void setId(Long id) {
        this.id = id;
    }
    
    public BigDecimal getPrice()  {
        return price;
    }
    
    public void setPrice(BigDecimal price) {
        this.price = price;
    }
}
```

- `@Column(precision = 10, scale = 2)`
    - `price` 필드는 소수점 이하 두 자리를 포함하는 **최대 10자리의 숫자로 저장됩니다.**
        - 예를 들어, 가격이 12345678.99와 같은 값을 가질 수 있습니다.
    - 여기에서 `precision`은 숫자의 전체 자릿수를, `scale`은 소수점 이하 자릿수를 의미합니다.

## 4️⃣ `@Column`의 기본값.
- `@Column` 어노테이션을 생략해도 JPA(Java Persistence API)는 자동으로 이름을 컬럼(Column, 열) 이름으로 사용하고, 기본 속성을 적용합니다.
    - 예를 들어, 아래 코드에서 `name` 필드는 자동으로 `name`이라는 컬럼(Column, 열)과 매핑됩니다.

```java
@Entity
public class User {
    
    @Id
    private Long id;
    
    private String name; // @Column을 생략했지만 필드 이름이 컬럼 이름으로 사용됨
    
    // 기본 생성자, getter, setter
}
```

## 5️⃣ 요약.
- `@Column` 어노테이션은 **엔티티 필드와 데이터베이스 컬럼(Column, 열)** 간의 매핑을 정의하는 역할을 합니다.
- 이 어노테이션을 사용하면 컬럼(Column, 열)의 이름, nullable 여부, 고유 제약조건 등을 세밀하게 설정할 수 있습니다.
- 필수는 아니지만, 명시적으로 데이터베이스 컬럼(Column, 열)의 속성을 지정해야 할 경우 유용하게 사용할 수 있습니다.
- 이를 통해 개발자는 데이터베이스 테이블 구조에 맞추어 엔티티 클래스를 설계하고, 데이터베이스 컬럼(Column, 열)과의 매핑을 정확하게 설정할 수 있습니다.
