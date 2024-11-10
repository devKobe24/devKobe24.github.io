---
title: 🍃[Spring] JPA 연관관계에 대한 추가적인 기능들에는 무엇이 있을까요? - 연관관계 주인 효과
tags:
    - Spring
    - Framework
date: "2024-11-10"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] JPA 연관관계에 대한 추가적인 기능들에는 무엇이 있을까요? - 연관관계 주인 효과

## 1️⃣ 연관관계 주인 효과.
- JPA에서 연관관계의 주인으로 설정된 엔티티만 **데이터베이스의 외래 키를 반영할 수 있는 효과**를 말합니다.
- JPA에서는 **연관관계의 주인(Owner)과 주인이 아닌 엔티티(Non-owner)를** 구분하여, 데이터베이스에 외래 키를 저장하거나 업데이트할 때 주인으로 설정된 엔티티만 실제 SQL에 반영되도록 합니다.

## 2️⃣ 연관관계 주인 효과의 동작 방식.
- 양방향 연관관계에서 두 엔티티가 서로를 참조할 때, 어느 한쪽을 "연관관계의 주인"으로 설정하고, 그 주인에서만 **외래 키 값을 저장**하거나 **변경할 수 있습니다.**
    - 주인이 아닌 쪽에서는 mappedBy 속성을 통해 주인이 아님을 명시하고, 이는 **읽기 전용**으로 취급됩니다.
        - 이를 통해 JPA는 중복된 외래 키 저장을 방지하고, 실제 외래 키 관리를 일관되게 처리합니다.

### 👉 예시: 1:1 관계에서의 연관관계 주인 효과.
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @OneToOne
    @JoinColumn(name = "user_profile_id") // 외래 키 관리
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
    
    @OneToOne(mappedBy = "userProfile") // 주인이 아님을 명시
    private User user;
    
    // getter, setter 등
}
```

- 위 코드에서 User 엔티티가 UserProfile 엔티티와 연관관계에서 **주인**입니다.
    - userProfile 필드에 대한 설정만 데이터베이스에 반영됩니다.

### 👉 연관관계 주인 효과의 예시 상황.
```java
User user = new User();
UserProfile profile = new UserProfile();

user.setUserProfile(profile);
profile.setUser(user);

entityManager.persist(user);
entityManager.persist(profile);
```
- 위와 같이 user의 userProfile을 설정하고 profile의 user를 설정해도, 실제 데이터베이스에는 User 엔티티의 userProfile에 설정된 값만 **외래 키로 반영**됩니다.
    - UserProfile 엔티티에서 user 필드의 변경은 데이터베이스에 반영되지 않습니다.
        - 이를 통해 불필요한 쿼리가 발생하는 것을 방지하고, 데이터 무결성을 유지할 수 있습니다.

## 3️⃣ 요약.
- **연관관계 주인만** 데이터베이스에 외래 키 정보를 반영할 수 있습니다.
- **주인이 아닌 엔티티**에서 설정한 연관관계는 데이터베이스에 반영되지 않으며, 읽기 전용으로 사용됩니다.
- 이를 통해 중복된 외래 키 저장을 방지하고, 데이터 무결성을 유지할 수 있습니다.

## 4️⃣ 연관관계 주인.
- **외래 키(Foreign Key)를 관리하는 엔티티**를 말합니다.
    - 이는 **데이터베이스에 연관관계와 관련된 변경사항을 반영할 때 어떤 엔티티가 외래 키의 수정, 삽입, 삭제 작업을 수행하는지를 결정합니다.**

## 5️⃣ 연관관계 주인의 역할.
- 연관관계에서 주인으로 지정된 엔티티는 **데이터베이스의 외래 키 컬럼을 관리하며,** 주인이 아닌 쪽에서는 연관관계를 읽기 전용으로 사용합니다.
    - 예를 들어, **양방향 연관관계**에서 `@OneToOne`, `@OneToMany`, `@ManyToMany`, `@ManyToOne`, `@ManyToMany` 중 주인이 되는 쪽에서만 외래 키의 값이 변경됩니다.

## 6️⃣ 연관관계 주인 설정의 중요성.
- JPA에서 연관관계의 주인을 설정하는 이유는 양방향 연관관계에서 **두 엔티티 모두 연관관계를 맺고 있는 경우, 어느 쪽이 실제로 데이터베이스에 반영할지를 명확히 하기 위함입니다.**
    - 주인이 아닌 엔티티에서 연관관계를 설정하더라도 데이터베이스에는 반영되지 않고, 주인에서 설정된 내용만 반영됩니다.

### 👉 예제: 1:1 관계에서의 연관관계 주인 설정.
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @OneToOne
    @JoinColumn(name = "user_profile_id") // 외래 키 관리
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

- 위 코드에서 User 엔티티가 연관관계의 주인이며, UserProfile은 mappedBy 속성을 통해 **연관관계 주인이 아님을 명시하고 있습니다.**
    - 이 경우 외래 키는 User 엔티티의 user_profile_id 컬럼에 저장됩니다.

## 7️⃣ 요약.
- **연관관계 주인 :** 데이터베이스에 외래 키를 관리하는 엔티티. `@JoinColumn`을 통해 설정됨.
- **주인이 아닌 엔티티 :** mappedBy 속성을 통해 설정되며, 읽기 전용으로 작동.
- 주인만이 외래 키 수정, 삭제, 삽입을 관리하고 데이터베이스에 반영함.
