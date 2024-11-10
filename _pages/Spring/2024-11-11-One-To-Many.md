---
title: 🍃[Spring] JPA 연관관계에 대한 추가적인 기능들에는 무엇이 있을까요? - `@OneToMany`
tags:
    - Spring
    - Framework
date: "2024-11-11"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] JPA 연관관계에 대한 추가적인 기능들에는 무엇이 있을까요? - `@OneToMany`
- `@OneToMany`는 **1:N(일대다) 관계**를 나타내는 어노테이션입니다.
    - 이는 한 개의 엔티티가 여러 개의 엔티티와 관계를 맺는 상황에서 사용됩니다.
        - 예를 들어, 하나의 팀(Team)이 여러 명의 회원(User)과 관계를 맺는 경우 `@OneToMany`를 사용하여 표현할 수 있습니다.

## 1️⃣ `@OneToMany` 어노테이션 기본 개념.
- **일대다 관계**에서 **1(One)** 쪽이 **다수(N)** 쪽을 참조할 때 `@OneToMany` 어노테이션을 사용합니다.
- **보통 다수(N)** 쪽이 외래 키를 가지고 있어 일대다 관계에서 **다수 쪽**이 관계의 **주인**이 됩니다.
- `@OneToMany` 어노테이션은 mappedBy 속성과 함께 사용하여 **주인이 아님을 명시**하고, 관계를 읽기 전용으로 설정합니다.

### 👉 예시: 팀(Team)과 회원(User) 간의 1:N 관계
- 팀(Team) 엔티티와 회원(User) 엔티티가 1:N 관계를 가질 때, Team 엔티티에서 여러 User 엔티티를 참조하도록 설정할 수 있습니다.

```java
@Entity
public class Team {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @OneToMany(mappedBy = "team") // Team이 연관관예의 주인이 아님을 명시
    private List<User> users = new ArrayList<>();
    
    // getter, setter
}

@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @ManyToOne // 다대일 관계에서 User가 연관관계의 주인
    @JoinColumn(name = "team_id") // 외래 키 컬럼 지정
    private Team team;
    
    // getter, setter
}
```

### 👉 코드 설명
- Team 엔티티는 `@OneToMany(mappedBy = "team")`을 사용하여 User와의 관계에서 주인이 아님을 명시합니다.
    - mappedBy 속성의 값 "team"은 User 엔티티의 team 필드를 가리킵니다.
- User 엔티티는 `@ManyToOne`과 `@JoinColumn(name = "team_id")`을 사용하여 왜래 키 team_id 컬럼을 생성하며, 관계의 주인이 됩니다.

## 2️⃣ 단방향과 양방향 `@OneToMany`

### 1️⃣ 단방향 `@OneToMany`
- `@OneToMany`를 사용하여 단방향으로 관계를 설정할 수도 있습니다.
    - 하지만 데이터베이스에 추가적인 조인 테이블이 생성되므로, 보통 성능상의 이유로 많이 사용하지 않습니다.

### 2️⃣ 양방향 `@OneToMany`
- `@OneToMany`와 `@ManyToOne`을 함께 사용하여 양방향으로 설정할 수 있습니다.
    - 이 경우 `@ManyToOne` 쪽이 외래 키를 가지므로 연관관계의 주인이 됩니다.

## 3️⃣ `@OneToMany` 사용 시 주의사항.

### 1️⃣ 지연 로딩(Lazy Loading)
- `@OneToMany`의 기본 로딩 전략은 지연 로딩입니다.
    - 즉, 엔티티를 조회할 때는 즉시 로딩하지 않고, 실제로 접근할 때 데이터를 로드하여 성능을 최적화합니다.

### 2️⃣ Cascade 옵션 사용.
- CascadeType.ALL 이나 CascadeType.REMOVE 등의 옵션을 사용하면, 부모 엔티티를 저장하거나 삭제할 때 자식 엔티티도 함께 저장/삭제됩니다.
    - 주의해서 사용해야 하며, 불필요하게 자식 엔티티가 삭제되지 않도록 합니다.

### 3️⃣ N+1 문제.
- `@OneToMany`는 다수의 자식 엔티티를 가져올 때 N+1 문제가 발생할 수 있으므로, 필요한 경우 **페치 조인(fetch join)을** 사용해 해결해야 합니다.

### 👉 예시 상황.
- **팀(Team)과 회원(User) :** 한 팀에 여러 명의 회원이 소속될 수 있음.
- **주문(Order)과 상품(Item) :** 한 주문에 여러 상품이 포함될 수 있음.

## 4️⃣ 요약.
- `@OneToMany`는 1:N 관계를 표현하며, 보통 mappedBy 속성을 사용하여 연관관계의 주인이 아님을 명시합니다.
    - 이 어노테이션을 통해 부모-자식 관계를 설정하여 편리하게 다수의 엔티티를 관리할 수 있지만, 성능 및 데이터 일관성을 유지하기 위해 로딩 전략과 Cascade 옵션을 주의해서 설정해야 합니다.
