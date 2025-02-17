---
title: "📚[Backend Development] mappedBy란 무엇일까요?"
tags:
    - Backend Ddevelopment
date: "2025-02-18"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] mappedBy란 무엇일까요?"
## 🍎 Intro.
- mappedBy는 **양방향 연관관계에서 사용되는 속성**으로, **연관 관계의 주인이 아닌(읽기 전용) 쪽에서 사용**합니다.
- 즉, **외래 키(FK)를 관리하지 않는 쪽에서 mappedBy를 사용하여 연관 관계를 매핑**합니다.

## ✅1️⃣ mappedBy의 필요성.
- 양방향 관계에서는 두 개의 엔티티가 서로를 참조하게 되는데, **JPA는 외래 키(FK)를 관리할 "주인"을 하나만 지정해야 합니다.**
    - 이때, **연관 관계의 주인이 아닌 쪽**에서 mappedBy를 사용하여 주인을 명시합니다.

## ✅2️⃣ @OneToOne 양방향 관계에서 mappedBy 사용 예제
### 1️⃣ User 엔티티 (연관 관계의 주인)
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = Generation.IDENTITY)
    private Long id;
    
    private String username;
    
    @OneToOne
    @JoinColumn(name = "profile_id") // FK를 관리하는 주인 (user 테이블에 profile_id FK 생성)
    private UserProfile profile;
    
    // Getter, Setter
}
```

### 2️⃣ UserProfile 엔티티(mappedBy 사용)
```java
@Entity
public class UserProfile {
    @Id
    @GenerationValue(strategy = Generation.IDENTITY)
    private Long id;
    
    private String bio;
    private String website;
    
    @OneToOne(mappedBy = "profile") // User 엔티티의 profile 필드가 관계의 주인
    private User user;
    
    // Getter, Setter
}
```

## ✅3️⃣ mappedBy = "profile"의 의미
- "profile"은 **User 엔티티의 profile 필드명을 가리킵니다.**
- 즉, **이 관계의 주인은 User.profile이며, UserProfile 엔티티는 읽기 전용입니다.**
- 따라서 UserProfile.user 필드는 **외래 키(FK)를 생성하지 않고, 매핑만 수행합니다.**

## ✅4️⃣ 데이터베이스 테이블 구조
- 위 코드를 실행하면 **user 테이블만 profile_id라는 FK 컬럼을 가지며, user_profile 테이블에는 추가 컬럼이 생성되지 않습니다.**

### 📊 user 테이블

|id|username|profile_id (FK)|
| -------- | -------- | -------- |
|1|Alice|101|
|2|Bob|102|

### 📊 user_profile 테이블

|id|bio|website|
| -------- | -------- | -------- |
|101|"Gamer"|"alice.com"|
|102|"Developer"|"bob.dev"|

- 📌 **외래 키는 user.profile_id에만 존재하며, user_profile 테이블에는 FK 컬럼이 없습니다.**

## ✅5️⃣ mappedBy를 사용한 데이터 조회
### ✅ User ➞ UserProfile 조회(가능 ✅)
```java
User user = entityManager.find(User.class, 1L);
UserProfile profile = user.getProfile(); // 정상 작동
```

### ✅ UserProfile ➞ User 조회(가능 ✅)
```java
UserProfile profile = entityManager.find(UserProfile.class, 101L);
User user = profile.getUser(); // mappedBy를 사용했으므로 가능!
```

## 🚀 정리.
- ✔️ **연관 관계의 주인(Owner)이 아닌 쪽에서 mappedBy를 사용해야 한다.**
- ✔️ **"mappedBy = 주인 엔티티 필드명"으로** 설정해야 한다.
- ✔️ **외래 키(FK)는 mappedBy를 사용한 쪽이 아니라 주인이 관리한다.**
- ✔️ **mappedBy는 읽기 전용이므로 @JoinColumn을 사용하지 않는다.**

📌 mappedBy를 사용하면 **불필요한 FK 컬럼 생성 방지 및 데이터베이스 테이블을 깔끔하게 유지할 수 있습니다.**
