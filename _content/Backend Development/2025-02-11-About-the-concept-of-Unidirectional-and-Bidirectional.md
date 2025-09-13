---
title: "📚[Backend Development] 단방향과 양방향의 개념에 대하여."
tags:
    - Backend Ddevelopment
    - JPA
date: "2025-02-11"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 단방향과 양방향의 개념에 대하여."
## 🍎 Intro.
- JPA에서 엔티티 간의 관계를 설정할 때 **단방향**과 **양방향** 관계를 정의할 수 있습니다.
- 이는 데이터베이스의 **외래 키(Foreign Key)** 관계를 객체 지향적으로 매핑하는 방식에 따라 달라집니다.

## ✅1️⃣ 단방향 관계 (Unidirectional)
- 단방향 관계는 **한쪽 엔티티만 다른 엔티티를 참조**하는 방식입니다.

### 📝 예제: 단방향 @OneToOne 관계.

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Unidirectional_@OneToOne_Example.png?raw=true">

```java
@Entity
public class Passport {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String passportNumber;
}

@Entity
public class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    
    @OneToOne
    @JoinColumn(name = "passport_id") // 외래 키를 Person 테이블이 가짐
    private Passport passport;
}
```

### 📌 특징
- Person 엔티티에서만 Passport를 참조할 수 있습니다.
- Passport 엔티티에서는 Person을 전혀 모릅니다.
- 테이블 구조에서는 Person 테이블에 passport_id라는 외개 키가 존재합니다.
- 데이터 조회 시 Person을 가져올 때 Passport도 함께 조회할 수 있습니다.

## ✅2️⃣ 양방향 관계 (Bidirectional)
- 양방향 관계는 **두 엔티티가 서로를 참조**하는 방식입니다.

## 📝 예제: 양방향 @OneToOne 관계.

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Bidirectional_@OneToOne_Example.png?raw=true">

```java
@Entity
public class Passport {
    @Id
    @GeneratedValue(startegy = GenerationType.IDENTITY)
    private Long id;
    private String passportNumber;
    
    @OneToOne(mappedBy = "passport") // Person 엔티티의 passport 필드와 연결
    private Person person;
}

@Entity
public class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    
    @OneToOne
    @JoinColumn(name = "passport_id") // 실제 외래 키를 소유
    private Passport passport;
}
```

### 📌 특징
- Person이 Passport를 참조하고, Passport도 Person을 참조합니다.
- Person이 **외래 키를 소유(@JoinColumn(name = "passport_id))"**
- Passport에서 mappedBy = "passport"를 사용하여 **반대편에서 매핑을 담당함.**
- 두 엔티티가 서로 참조하기 때문에 **양방향 탐색 가능**(예: passport.getPerson())

## ✅3️⃣ 단방향 VS 양방향 비교.

|구분|단방향 관계|양방향 관계|
| -------- | -------- | -------- |
|참조 방향|한쪽 엔티티만 다른 엔티티를 참조|두 엔티티가 서로 참조|
|테이블 구조|한쪽 테이블에만 외래 키 존재|테이블 구조는 동일하나 객체에서 상호 참조|
|조회 방향|한쪽에서만 조회 가능|양쪽에서 조회 가능|
|사용 예|단순한 연관 관계|상관 관계가 필요한 경우|

## ✅4️⃣ 언제 단방향/양방향을 선택해야 할까?
### 1️⃣ 단방향이 더 적합한 경우.
- **반대 방향에서 참조할 필요가 없는 경우**
    - 예: Order ➞ Payment (주문은 결제를 참조하지만, 결제는 주문을 참조할 필요 없음)
- **성능을 최적화하고 불필요한 데이터 로딩을 방지하고 싶은 경우**
    - 양방향 관계를 만들면 불필요한 연관 객체까지 로딩될 수 있음.
    - FetchType.LAZY를 설정하더라도 관리 부담이 커질 수 있음.


### 2️⃣ 양방향이 더 적합한 경우.
- **양쪽에서 참조할 필요가 있는 경우**
    - 예: Member ↔ Team (회원이 팀을 참조하고, 팀도 회원 목록을 관리해야 함)
- **반대 엔티티를 쉽게 조회해야 하는 경우**
    - 예를 들어 Passport에서 Person을 조회하는 기능이 자주 필요하다면 양방향이 유리함.
    - OneToMany, ManyToOne 관계에서는 성능 고려 후 양방향 설정을 할 수도 있음.

## ✅5️⃣ 정리
- @OneToOne, @OneToMany, @ManyToOne, @ManyToMany 관계는 **단방향과 양방향이 모두 가능**합니다.
- 단방향은 한쪽에서만 참조, 양방향은 서로 참조합니다.
- **양방향 관계에서는 mappedBy를 사용하여 연관 관계의 주인을 지정**해야 합니다.
- 불필요한 양방향 관계를 피하고, 필요한 경우에만 적용하여 성능과 유지보수성을 고려해야 합니다.

👉 일반적으로 **단반향을 기본으로 하고, 필요할 때만 양방향을 추가하는 것이 좋습니다.**
