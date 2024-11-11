---
title: 🍃[Spring] JPA의 어노테이션 - `@JoinColumn`
tags:
    - Spring
    - Framework
date: "2024-11-11"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] JPA의 어노테이션 - `@JoinColumn`
- `@JoinColumn`은 JPA에서 **두 엔티티 간의 연관관계를 매핑할 때 외래 키(Foreign Key) 컬럼을 설정**하기 위해 사용하는 어노테이션입니다.
- `@JoinColumn`은 관계가 설정되는 테이블에 외래 키(Foreign Key)를 정의하고, 해당 외래 키(Foreign Key) 컬럼이 **연관된 엔티티를 참조**하게 합니다.

## 1️⃣ `@JoinColumn`의 주요 속성.
- **name**
    - 외래 키(Foreign Key) 컬럼의 이름을 지정합니다.
        - 지정하지 않으면 기본적으로 **참조하는 엔티티의 필드명 또는 프로퍼티명 + _id 형식으로 이름이 설정됩니다.**
- **referencedColumnName**
    - 외래 키(Foreign Key)가 참조할 대상 엔티티의 컬럼명을 지정합니다.
        - 기본적으로 참조 엔티티의 기본 키가 사용됩니다.
- **nullable**
    - 외래 키(Foreign Key) 컬럼이 NULL을 허용할지 여부를 지정합니다.
        - 기본값은 true이며, false로 설정하면 반드시 값이 있어야 합니다.
- **unique**
    - 외래 키(Foreign Key) 컬럼이 유일한 값인지 설정합니다.
        - 기본값은 false입니다.
- **insertable, updatable**
    - 외래 키(Foreign Key) 컬럼의 값이 삽입/업데이트 가능한지 설정합니다.

## 2️⃣ `@JoinColumn` 예시.
- 팀(Team)과 회원(User)간의 **다대일(N:1)** 관계에서 User 엔티티의 team 필드를 통해 Team 엔티티와의 관계를 설정하며, `@JoinColumn`을 사용해 외래 키(Foreign Key)를 매핑할 수 있습니다.
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @ManyToOne
    @JoinColumn(name = "team_id") // 외래 키 컬럼 설정
    private Team team;
    
    // getter, setter
}

@Entity
public class Team {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    // getter, setter
}
```

### 👉 설명.
- User 엔티티에서 team 필드는 `@ManyToOne` 관계를 통해 Team 엔티티와 연결됩니다.
- `@JoinColumn(name = "team_id")`를 사용하여 User 테이블에 team_id라는 외래 키(Foreign Key) 컬럼이 생성되도록 지정했습니다.
    - 이 컬럼은 Team 엔티티의 기본 키 id를 참조하게 됩니다.

## 3️⃣ `@JoinColumn`을 사용한 양방향 관계 예시.
- `@JoinColumn`은 **양방향 관계**에서도 자주 사용됩니다.
    - 예를 들어, User와 Team 엔티티가 서로를 참조하는 양방향 관계로 설정할 수 있습니다.
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @ManyToOne
    @JoinColumn(name = "team_id")
    private Team team;
}

@Entity
public class Team {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @OneToMany(mappedBy = "team") // 연관관계 주인이 아님을 명시
    private List<User> users = new ArrayList<>();
    
    // getter, setter
}
```

### 👉 설명.
- User 엔티티는 team_id 컬럼을 통해 Team 엔티티와 연결되며, 이 외래 키(Foreign Key)가 관계의 주인이 됩니다.
- Team 엔티티는 users 필드를 통해 User 엔티티를 참조하며, mappedBy 속성을 통해 연관관계 주인이 아님을 명시했습니다.

## 4️⃣ 요약.
- `@JoinColumn`은 두 엔티티 간의 관계에서 외래 키를 설정하기 위해 사용되며, 외래 키 컬럼의 이름과 속성을 지정할 수 있습니다.
- 단방향, 양방향 관계에서 모두 사용되며, 명확한 외래 키 설정을 통해 데이터베이스 테이블 간의 관계를 정의합니다.
- name. nullable, unique 등 다양한 속성을 설정해 데이터베이스 컬럼의 제약 조건을 조정할 수 있습니다.
