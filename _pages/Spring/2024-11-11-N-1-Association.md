---
title: 🍃[Spring] JPA 연관관계에 대한 추가적인 기능들에는 무엇이 있을까요? - N:1관계
tags:
    - Spring
    - Framework
date: "2024-11-11"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] JPA 연관관계에 대한 추가적인 기능들에는 무엇이 있을까요? - N:1관계
- **N:1 관계란** 여러 엔티티가 하나의 엔티티에 **연관될 수 있는 관계를 의미합니다.**
- 데이터베이스에서 **다대일 관계**라고도 부르며, 예를 들어 하나의 **회원(User)은 하나의 팀(Team)에** 소속될 수 있고, **여러 명의 회원이 동일한 팀에 속할 수 있을 때 N:1 관계를 사용하여 매핑합니다.**

## 1️⃣ N:1 관계의 매핑 방식.
- `@ManyToOne` 어노테이션을 사용하여 N:1 관계를 매핑합니다.
    - 이때 **다수(N) 쪽에서 하나(1) 쪽을 참조하여 외래 키(Foreign Key)를 가지게 됩니다.**

## 👉 예시: 회원(User)과 팀(Team) 간의 N:1 관계.
- 회원과 팀의 관계에서, 여러 명의 회원이 하나의 팀에 소속될 수 있으므로 User 엔티티에서 Team 엔티티와의 관계를 N:1로 설정할 수 있습니다.

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @ManyToOne // N:1 관계 설정.
    @JoinColumn(name = "team_id") // 외래 키 컬럼 이름 지정.
    private Team team;
    
    // getter, setter
}

@Entity
public class Team {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @OneToMany(mappedBy = "team") // Team이 User와의 양방향 관계에서 주인이 아님.
    private List<User> users = new ArrayList<>();
    
    // getter, setter
}
```

## 👉 코드 설명.
- User 엔티티의 team 필드는 `@ManyToOne`으로 설정되어 있으며, JoinColumn을 사용하여 외래 키로 `team_id` 컬럼을 지정합니다.
- Team 엔티티는 `@OneToMany(mappedBy = "team")`으로 User 엔티티와 양방향 관계를 맺고 있으며, users 리스트를 통해 자신의 회원들을 참조합니다.
    - 여기서 `mappedBy` 속성을 통해 **연관관계의 주인이 아님을 명시했습니다.**

## 2️⃣ 단방향과 양방향.

### 1️⃣ 단방향 N:1 관계.
- User 엔티티에서만 Team 엔티티를 참조하여 관계를 나타냅니다.
    - 데이터베이스에는 외래 키(Foreign Key)가 User 엔티티에만 존재합니다.

### 2️⃣ 양방향 N:1 관계.
- User와 Team 엔티티가 서로를 참조하며 양방향 관계를 갖습니다.
    - User 엔티티가 연관관계의 주인이므로, Team 엔티티의 users 필드는 읽기 전용입니다.

## 3️⃣ N:1 관계의 예시 상황.
- N:1 관계는 다양한 상황에서 사용됩니다.

### 1️⃣ 학생(Student)과 학교(School)
- 여러 명의 학생이 하나의 학교에 소속될 수 있음.

### 2️⃣ 주문(Order)과 고객(Customer)
- 여러 개의 주문이 하나의 고객에게 속할 수 있음.

## 4️⃣ 주의 사항.
- N:1 관계에서는 다수 쪽이 연관관계의 주인이며 외래 키를 가집니다.
- 필요에 따라 지연 로딩(Lazy Loading)을 설정하여 성능을 최적화하는 것이 좋습니다.
