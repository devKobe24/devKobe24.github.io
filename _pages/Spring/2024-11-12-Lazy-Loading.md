---
title: 🍃[Spring] 지연 로딩(Lazy Loading)은 무엇인가요?
tags:
    - Spring
    - Framework
date: "2024-11-12"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 지연 로딩(Lazy Loading)은 무엇인가요?
- JPA에서 **실제 데이터가 필요한 시점까지 데이터베이스 조회를 지연하는 기법입니다.**
- 엔티티를 처음 조회할 때는 연관된 데이터를 즉시 로드하지 않고, 그 연관된 데이터가 실제로 사용될 때 데이터베이스에서 조회하는 방식입니다.
    - 이 방식은 **불필요한 데이터 조회를 줄여서 성능을 최적화하는 데 유리합니다.**

## 1️⃣ 지연 로딩(Lazy Loading)의 기본 동작.
- 지연 로딩(Lazy Loading)을 설정하면 연관된 엔티티나 컬렉션은 처음에 프록시 객체로 로드됩니다.
    - 프록시는 실제 엔티티를 대신하는 객체로, 데이터베이스 조회가 필요할 때 프록시가 실제 데이터를 조회하여 값을 제공합니다.
        - 따라서 처음부터 연관된 데이터를 모두 로드하는 것이 아니라 **실제 접근 시점에 데이터베이스에서 로드되도록 지연됩니다.**

> 🙋‍♂️ JPA에서의 "프록시(Proxy)"
> 
> **프록시(Proxy)는** JPA에서 **실제 엔티티를 대신하여 생성되는 가짜 객체입니다.**
> 이 프록시는 **연관된 엔티티를 지연 로딩할 때 사용되며, 실제 데이터가 필요한 시점까지 데이터베이스 조회를 지연하는 역할을 합니다.**
> 프록시(Proxy)는 **실제 엔티티와 동일한 인터페이스나 클래스를 상속받아 만들어진 객체로, 실제 데이터가 필요한 시점에 데이터베이스에서 데이터를 로드하여 값을 제공합니다.**

## 2️⃣ 예시: `@OneToMany` 지연 로딩(Lazy Loading) 설정.
- 예를 들어, Team 엔티티가 여러 Member 엔티티와 연관되어 있을 때 `@OneToMany` 관계의 지연 로딩(Lazy Loading)을 설정해볼 수 있습니다.

```java
@Entity
public class Team {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @OneToMany(mappedBy = "team", fetch = FetchType.LAZY) // 지연 로딩 설정
    private List<Member> members = new ArrayList<>();
    
    // getter, setter
}

@Entity
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @ManyToOne
    @JoinColumn(name = "team_id")
    private Team team;
    
    // getter, setter
}
```

## 3️⃣ 설명.
- Team 엔티티의 members 필드에 fetch = FetchType.LAZY 옵션을 설정했습니다.
- Team 엔티티를 조회할 때 members 리스트는 데이터베이스에서 즉시 로드되지 않으며, members 컬렉션에 실제 접근하는 순간 데이터베이스에서 조회가 이루어집니다.

## 4️⃣ Lazy Loading의 장점.

### 1️⃣ 성능 최적화.
- 필요한 데이터만 조회하여 불필요한 쿼리 실행을 방지하므로 성능이 최적화됩니다.

### 2️⃣ 메모리 효율성.
- 필요한 순간까지 메모리에 데이터를 로드하지 않으므로 메모리 사용을 줄일 수 있습니다.

### 3️⃣ 큰 연관 관계 관리.
- 연관된 데이터가 많은 경우, 초기 로딩 시 성능 저하를 방지하고 필요한 시점에 필요한 데이터만 불러올 수 있습니다.

## 5️⃣ Eager Loading과의 비교.
- 반대로 **즉시 로딩(Eager Loading)은** 엔티티를 조회할 때 연관된 모든 데이터를 한 번에 함께 로드하는 방식입니다.
- 즉시 로딩(Eager Loading)은 fetch = FetchType.EAGER 옵션을 통해 설정할 수 있으며, 처음부터 연관된 모든 데이터를 로딩하기 때문에 쿼리가 많이 실행되거나, 많은 데이터를 불필요하게 로드하는 **N+1 문제**가 발생할 수 있습니다.

## 6️⃣ Lazy Loading 사용 시 주의사항.

### 1️⃣ 프록시 초기화 문제.
- 지연 로딩된 프록시는 트랜잭션 범위를 벗어난 후에는 초기화할 수 없기 때문에, 연관 데이터를 접근하려면 트랜잭션 내에서 접근해야 합니다.

### 2️⃣ N+1 문제.
- 지연 로딩을 잘못 사용하면 N+1 문제가 발생할 수 있습니다.
    - 예를 들어, 리스트 형태의 엔티티를 조회할 때 각 엔티티의 연관 데이터에 대해 추가 쿼리가 발생하는 경우입니다.
        - 이를 해결하기 위해 **페치 조인(Fetch Join)을** 사용하여 필요한 데이터를 한 번에 가져오는 것이 좋습니다.

### 3️⃣ JPA 표준 제약.
- `@OneToMany` 및 `@ManyToMany`는 기본적으로 LAZY로 설정되지만, `@ManyToOne` 및 `@OneToOne` 관계는 기본이 EAGER로 설정됩니다.
    - 따라서 필요에 따라 명시적으로 fetch = FetchType.LAZY로 설정해야 합니다.

## 7️⃣ 요약.
- 지연 로딩(Lazy Loading)은 연관된 데이터를 **실제 사용 시점까지 지연하여 조회함으로써 성능을 최적화하는 기법입니다.**
    - 이를 통해 불필요한 데이터 조회를 방지하고 메모리 효율성을 높일 수 있지만, 사용 시 주의할 점을 고려하여 최적의 방식으로 설정해야 합니다.
