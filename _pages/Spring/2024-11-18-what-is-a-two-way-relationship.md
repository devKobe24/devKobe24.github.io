---
title: 🍃[Spring] 양방향 관계란 무엇일까요?
tags:
    - Spring
    - Framework
date: "2024-11-18"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 양방향 관계란 무엇일까요?
- **양방향 관계는** 두 엔티티가 서로를 참조할 수 있는 관계를 의미합니다.
    - 즉, 한 엔티티에서 다른 엔티티를 참조할 수 있을 뿐만 아니라, 반대로 다른 엔티티에서도 이를 참조할 수 있습니다.
- 양방향 관계를 사용하면 두 엔티티 간의 데이터 탐색이 양쪽 방향으로 모두 가능해지며, 이는 JPA에서 다음과 같은 관계 어노테이션 조합으로 구현됩니다.
    - `@OneToMany` + `@ManyToOne`
    - `@OneToOne` + `@OneToOne`
    - `@ManyToMany` + `@ManyToMany`

## 1️⃣ 양방향 관계의 특징.

### 1️⃣ 양쪽에서 참조 가능.
- 양쪽 엔티티가 서로를 참조하여 데이터 탐색이 가능합니다.
    - 예를 들어, 부모 엔티티에서 자식 엔티티들을 조회하거나, 자식 엔티티에서 부모 엔티티를 조회할 수 있습니다.

### 2️⃣ 주인(Owner)과 비주인(Inverse) 설정.
- JPA에서 양방향 관계를 설정할 때, 반드시 **주인(Owner)과 비주인(Inverse)을** 명시해야 합니다.
- **주인(Owner) :** 외래 키(Foreign Key)를 실제로 관리하는 쪽.
- **비주인(Inverse) :** 읽기 전용으로 참조만 가능하며, **`mappedBy`** 속성을 통해 주인을 명시합니다.

### 3️⃣ 데이터베이스에서 외래 키는 한쪽에만 존재.
- 데이터베이스의 외래 키(Foreign Key)는 관계의 주인에 해당하는 엔티티의 테이블에만 존재합니다.

## 2️⃣ 양방향 관계의 구현 형태.

### 1️⃣ `@OneToMany` + `@ManyToOne`
- 부모-자식 관계를 구현하며, 부모는 자식의 컬렉션을 가지고 있고, 자식은 부모를 참조합니다.

#### 예제: User와 UserSaveHistory
- User 엔티티(부모)
```java
@Entity
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<UserSaveHistory> saveHistories = new ArrayList<>();
    
    public void addSaveHistory(UserSaveHistory history) {
        saveHistories.add(history);
        history.setUser(this);
    }
    
    public void removeSaveHistory(UserSaveHistory history) {
        saveHistories.remove(history);
        history.setUser(null);
    }
}
```

- UserSaveHistory 엔티티(자식)
```java
@Entity
public class UserSaveHistory {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id")
    private User user;
}
```

### 2️⃣ `@OneToOne` + `@OneToOne`
- 1:1 관계를 구현하며, 양쪽 엔티티가 서로를 참조합니다.

#### 예제: Passport와 Person

- Person 엔티티.
```java
@Entity
public class Person {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @OneToOne(mappedBy = "person", cascade = CascadeType.ALL)
    private Passport passport;
}
```

- Passport 엔티티.
```java
@Entity
public class Passport {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @OneToOne
    @JoinColumn(name = "person_id")
    private Person person;
}
```

### 3️⃣ `@ManyToMany` + `@ManyToMany`
- 다대다 관계를 구현하며, 중간 테이블을 통해 두 엔티티가 연결됩니다.

#### 예제: Student와 Course
- Student 엔티티.
```java
@Entity
public class Student {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToMany
    @JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id")
    )
    private List<Course> courses = new ArrayList<>();
}
```

- Course 엔티티.
```java
@Entity
public class Course {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToMany(mappedBy = "courses")
    private List<Student> students = new ArrayList<>();
}
```

## 3️⃣ 양방향 관계에서 주의점.

### 1️⃣ 주인(Owner)과 비주인(Inverse)
- JPA에서는 양방향 관계를 설정할 때 반드시 **주인**을 명시해야 합니다.
- **주인**은 외래 키(Foreign Key)를 관리하며, 데이터 변경(INSERT, UPDATE)에 **영향을** 미칩니다.
- **비주인**은 `mappedBy` 속성을 사용해 주인을 지정하며, 읽기 전용입니다.

### 2️⃣ 연관 관계 편의 메서드.
- 부모와 자식 간의 관계를 일관되게 유지하려면 편의 메서드를 사용하는 것이 좋습니다.
    - 예를 들어, 부모의 `addChild` 메서드를 호출하면 자식의 부모도 자동으로 설정되도록 구현합니다.
```java
public void addSaveHistory(UserSaveHistory history) {
    saveHistories.add(history);
    history.setUser(this);
}
```

> 🙋‍♂️ 편의 메서드
> 
> **관계를 동기화하기 위한 편의 메서드**란 **양방향 연관 관계에서 두 엔티티 간의 연관 관계를 일관성 있게 유지하기 위해 사용하는 메서드를 의미합니다.**

### 3️⃣ 지연 로딩(Lazy Loading)
- 양방향 관계에서는 `@OneToMany`와 `@ManyToOne` 모두 기본적으로 **지연 로딩(Lazy Loading)을** 사용하여 성능을 최적화합니다.
- 필요시 `fetch = FetchType.EAGER`로 설정할 수 있지만, 이는 데이터 로딩 시 성능에 영향을 줄 수 있습니다.

### 4️⃣ 무한 루프 문제
- 양방향 관계를 JSON으로 직렬화할 때, 부모와 자식이 서로를 참조하며 무한 루프가 발생할 수 있습니다.
    - 해결 방법
        - `@JsonIgnore` : 특정 필드를 직렬화하지 않도록 설정.
        - `@JsonManagedReference`와 `@JsonBackReference`: Jackson 라이브러리에서 관계를 처리하는 어노테이션.

## 4️⃣ 장점과 단점.

### 1️⃣ 장점.

#### 1️⃣ 양쪽 탐색 기능.
- 부모와 자식 모두에서 관계를 탐색할 수 있습니다.

#### 2️⃣ 명확한 관계 표현.
- 객체 모델에서 두 엔티티 간의 관계를 명확하게 표현할 수 있습니다.

### 2️⃣ 단점.

#### 1️⃣ 설계 복잡성 증가.
- 관계를 양쪽에서 관리해야 하므로 코드가 복잡해질 수 있습니다.

#### 2️⃣ 성능 문제.
- 양방향 관계를 사용할 경우 불필요한 데이터 로딩이 발생할 수 있으므로 로딩 전략을 신중히 선택해야 합니다.

## 5️⃣ 결론.
- **양방향 관계**는 두 엔티티 간에 **상호 참조가 필요할 때** 사용되며, 설계가 복잡해질 수 있지만 데이터를 양쪽 방향으로 탐색해야 하는 경우 유용합니다.
- JPA에서는 주인(Owner)을 명확히 설정하고, 편의 메서드를 통해 관계를 관리함으로써 데이터의 일관성을 유지해야 합니다.
- 필요 없는 경우 단방향 관계를 사용하는 것이 더 간단하고 성능상 유리합니다.
