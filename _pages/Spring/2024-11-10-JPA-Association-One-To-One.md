---
title: 🍃[Spring] JPA 연관관계에 대한 추가적인 기능들에는 무엇이 있을까요? - 1:1 관계
tags:
    - Spring
    - Framework
date: "2024-11-10"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] JPA 연관관계에 대한 추가적인 기능들에는 무엇이 있을까요? - 1:1 관계

## 1️⃣ 1:1 관계.
- 하나의 엔티티가 다른 하나의 엔티티와 1대 1로 매핑되는 관계를 의미합니다.
    - 예를 들어, **User**와 **UserProfile** 엔티티가 1:1 관계에 있다고 가정해 보겠습니다.
        - 각 User는 하나의 UserProfile만을 가지며, 각 UserProfile은 하나의 User에만 연결될 수 있습니다.
- 1:1 관계를 JPA에서 매핑할 때는 보통 `@OneToOne` 어노테이션을 사용합니다.
    - JPA에서는 `@JoinColumn`을 이용해 두 엔티티를 연결하는 외래 키를 지정할 수도 있습니다.

### 👉 예시 코드.
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @OneToOne
    @JoinColumn(name = "user_profile_id") // 외래 키 설정
    private UserProfile userProfile;
    
    // getter, setter 등
}

@Entity
public class UserProfile {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String address;
    private String phoneNumber;
    
    @OneToOne(mappedBy = "userProfile") // 연관관계 주인이 아님을 명시
    private User user;
    
    // getter, setter 등
}
```

### 👉 설명.
- User 엔티티는 UserProfile 엔티티와 1:1 관계로, userProfile 필드를 통해 UserProfile을 참조합니다.
- `@JoinColumn`은 User 엔티티 테이블에 user_profile_id라는 외래 키 컬럼을 생성하여 두 엔티티를 연결합니다.
- USerProfile 엔티티에서는 mappedBy 속성을 사용하여 **양방향 관계**에서의 연관 관계의 주인을 User 엔티티로 설정합니다.

## 2️⃣ 단방향과 양방향.

### 1️⃣ 단방향 1:1 관계.
- 하나의 엔티티에서만 다른 엔티티를 참조합니다.

### 2️⃣ 양방향 1:1 관계.
- 두 엔티티가 서로를 참조할 수 있으며, mappedBy를 사용하여 주인을 명시합니다.

## 3️⃣ 마무리.
- 1:1 관계는 데이터베이스에서의 테이블이 잘 분리되고 성능이 적절히 유지되어야 할 때 유용하게 사용됩니다.
