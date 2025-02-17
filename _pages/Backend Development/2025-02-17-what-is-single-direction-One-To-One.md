---
title: "📚[Backend Development] 단방향과 @OneToOne이란 무엇일까요?"
tags:
    - Backend Ddevelopment
date: "2025-02-17"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 단방향과 @OneToOne이란 무엇일까요?"
## 🍎 Intro.
- 단방향 **@OneToOne** 관계는 엔티티 간의 1:1 관계를 매핑할 때, 한쪽 엔티티에서만 관계를 관리하는 방식입니다.
    - 즉, **한 엔티티에서만 다른 엔티티를 참조**하고, 반대쪽에서는 이를 알지 못하는 상태입니다.

## ✅1️⃣ 예제 코드
- 예를 들어, User 엔티티와 UserProfile 엔티티가 1:1 관계를 가진다고 가정해봅시다.

### 📝 User 엔티티에서 UserProfile 엔티티를 단방향으로 참조하는 경우:
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String username;
    
    @OneToOne
    @JoinColumn(name = "profile_id") // User 테이블의 profile_id 컬럼이 UserProfile의 id를 참조
    private UserProfile profile;
    
    // Getter, Setter
}
```

### 📝 UserProfile 엔티티:
```java
@Entity
public class UserProfile {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String bio;
    private String website;
    
    // Getter, Setter
}
```

### ✅ 설명:
- 1. @OneToOne을 사용하여 User 엔티티가 UserProfile 엔티티를 참조합니다.
- 2. @JoinColumn(name = "profile_id")를 사용하여 User 테이블에 profile_id 컬럼이 생성됩니다.
    - **User 테이블에 profile_id라는 외래 키(FK) 컬럼을 추가하고,** 이 컬럼이 UserProfile 테이블의 id(PK)를 참조하도록 만듭니다.
        - 즉, User 테이블의 profile_id가 UserProfile 테이블의 id를 참조하는 FK이다.
- 3. 하지만 **UserProfile 엔티티에는 User와의 관계를 알 수 있는 정보가 없습니다. ➞** 이것이 단방향 관계입니다.

### ✅ 살제 데이터베이스 테이블 예시:
- 이 코드를 기반으로 JPA가 생성하는 테이블을 보면 다음과 같이 됩니다.

#### 📊 user 테이블

|id|username|profile_id(FK)|
| --- | -------- | -------- |
|1|Alice|101|
|2|Bob|102|

#### 📊 user_profile 테이블

|id|bio|website|
| -------- | -------- | -------- |
|101|"Gamer"|"alice.com"|
|102|"Developer"|"bob.dev"|

- 📌 즉, **user.profile_id는 user_profile.id를 참조(FK)하는 구조**입니다.
    - 따라서 UserProfile 엔티티에는 profile_id가 따로 필요하지 않습니다.
    - 대신 **기본 키(id)가 User 엔티티의 외래 키(profile_id)로 사용**됩니다.

## ✅2️⃣ 단방향 관계의 특징.
### ✅ 장점.
- 구조가 단순하고 이해하기 쉽다.
- 한쪽에서만 참조하므로 불필요한 연관관계 로딩을 방지할 수 있다.

### ❌ 단점.
- 반대쪽(UserProfile)에서 User를 조회할 방법이 없다.
- UserProfile이 자신을 참조하는 User가 누구인지 알고 싶다면 별도의 쿼리를 작성해야 한다.

## ✅3️⃣ 단방향 관계 조회.
- 사용자가 프로필 정보를 가져오는 코드를 작성하면 다음과 같습니다.

```java
User user = entityManager.find(User.class, 1L);
UserProfile profile = user.getProfile(); // User -> UserProfile 조회 가능.
```
