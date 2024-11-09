---
title: 🍃[Spring] JPA에서 "연관관계의 주인" 이란 무엇일까요?
tags:
    - Spring
    - Framework
date: "2024-11-09"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] JPA에서 "연관관계의 주인" 이란 무엇일까요?
- 두 엔티티 사이에 양방향 관계가 있을 때, **외래 키(Foreign Key)를** 관리하는 주체가 되는 엔티티를 말합니다.
- 연관관계의 주인이 된 엔티티만이 **데이터베이스에서 외래 키를 수정할 수 있으며, 이로 인해 관계의 방향을 정의하고, 관리의 책임을 분명히 합니다.**

## 1️⃣ 연관관계의 주인이 필요한 이유.
- 객체의 관계에서는 양방향 관계를 쉽게 만들 수 있지만, 관계형 데이터베이스에서는 **외래 키가 하나의 테이블에서만 존재**하며, 한쪽에서만 외래 키를 업데이트할 수 있습니다.
- JPA는 이러한 관계형 데이터베이스의 제약을 반영하여, **어느 쪽이 관계를 관리할 것인지를 지정하도록 합니다.**

## 2️⃣ 연관관계 주인의 설정 방법.
- 연관관계의 주인은 `@ManyToOne` 또는 `@JoinToColumn` **어노테이션이 있는 엔티티**가 됩니다.
- 주인이 아닌 엔티티는 mappedBy **속성을 사용하여 연관관계의 주인을 지정**하며, 데이터베이스에 영향을 미치지 않고 읽기 전용 필드로만 사용됩니다.

## 3️⃣ 예시.
- 회원과 팀 사이에 다대일(`@ManyToOne`) 관계가 있는 경우, **회원 엔티티가 연관관계의 주인이** 되어 팀에 대한 외래 키를 가지게 됩니다.

```java
@Entity
public class Memeber {
    
    @Id
    @GenereatedValue
    private Long id;
    
    @ManyToOne // 연관관계의 주인
    @JoinColumn(name = "team_id") // 외래 키를 매핑
    private Team team;
    
    // getters and setters
}
```

```java
@Entity
public class Team {
    
    @Id
    @GeneratedValue
    private Long id;
    
    @OneToManay(mappedBy = "team") // 주인이 아님을 명시
    private List<Member> members = new ArrayList<>();
    
    // getters and setters
}
```
- 여기서 Memeber 엔티티가 **연관관계의 주인**이므로 `team_id` 외래 키를 직접 관리합니다.
    - 반면, Team 엔티티는 members 필드에 `mappedBy`를 사용하여 관계를 참조만 하고, 데이터베이스에 영향을 미치지 않습니다.

## 4️⃣ 연관관계 주인의 역할과 사용.
- **주인 엔티티에서만 외래 키 값을 설정 및 변경할 수 있습니다.**
- **주인이 아닌 엔티티에서 관계를 변경해도 데이터베이스에는 반영되지 않기 때문에, 관계를 변경할 때는 주인 엔티티에서 변경해야 합니다.**

## 5️⃣ 연관관계 주인을 잘못 설정했을 때 발생할 문제.
- 연관관계의 주인을 잘못 설정하면, 의도한 대로 데이터베이스에 관계가 반영되지 않거나 일관되지 않은 상태가 발생할 수 있습니다.
