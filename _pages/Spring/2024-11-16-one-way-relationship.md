---
title: 🍃[Spring] 단방향 관계란 무엇일까요?
tags:
    - Spring
    - Framework
date: "2024-11-16"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 단방향 관계란 무엇일까요?
- **단방향 관계**는 **두 엔티티 간의 연관관계가 한쪽 방향으로만 설정된 관계**를 의미합니다.
    - 즉, 한 엔티티는 다른 엔티티를 참조할 수 있지만, 반대 방향으로는 참조가 불가능합니다.
- 단방향 관계에서는 관계의 방향이 한쪽으로만 설정되기 때문에 **한 엔티티만 다른 엔티티를 탐색 가능**합니다.
- 데이터베이스 관점에서는 외래 키(Foregin Key)가 한쪽 테이블에만 존재하며, 반대쪽 관계 정보를 알 수 없습니다.

## 1️⃣ 특징.

### 1️⃣ 단방향 참조.
- 한쪽 엔티티에서만 다른 엔티티를 참조합니다.
- 반대 방향의 탐색은 불가능합니다.

### 2️⃣ 설계가 간단.
- 관계 방향이 단순하므로 유지보수가 쉽습니다.
- 필요 이상으로 복잡한 관계를 만들지 않아도 됩니다.

### 3️⃣ 외래 키(Foregin Key)
- 외래 키(Foregin Key)는 관계를 참조하는 엔티티(참조하는 쪽)에만 존재합니다.

## 2️⃣ 단방향 관계의 종류.

### 1️⃣ `@ManyToOne`
- **자식 엔티티(N)가** 부모 엔티티(1)를 참조하는 단방향 관계.
- 외래 키(Foreign Key)는 자식 테이블에 존재.

#### 👉 예제
- **Order와 Customer 관계 :** 여러 주문(Order)이 하나의 고객(Customer)을 참조.
```java
@Entity
public class Order {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne
    @JoinColumn(name = "customer_id") // 외래 키
    private Customer customer;
    
    // 기타 필드 및 메서드
}
```
- 데이터베이스에서 Order 테이블은 customer_id 컬럼을 외래 키(Foreign Key)로 가집니다.

### 2️⃣ `@OneToMany`
- **부모 엔티티(1)가** 자식 엔티티(N)의 컬렉션을 참조하는 단방향 관계.
- 외래 키(Foreign Key)는 자식 테이블에 존재하며, 부모 테이블에는 컬렉션만 관리.

#### 👉 예제
- **Customer와 Order 관계 :** 한 고객(Customer)이 여러 주문(Order)을 가짐.
```java
@Entity
public class Customer {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @OneToMany
    @JoinColumn(name = "customer_id") // 외래 키를 자식 테이블에 설정
    private List<Order> orders = new ArrayList<>();
    
    // 기타 필드 및 메서드.
}
```
- 데이터베이스에서는 외래 키가 여전히 Order 테이블에 존재.

### 3️⃣ `@OneToOne`
- 한 엔티티가 다른 엔티티와 1:1로 매핑되는 단방향 관계.
- 외래 키는 참조하는 쪽에만 존재.

#### 👉 예제.
- **Passport와 Person 관계 :** 한 사람(Person)이 하나의 여권(Passport)을 가짐.
```java
@Entity
public class Passport {
    
    @Id
    @GeneratedValue(stratege = GenerationType.IDENTITY)
    private Long id;
    
    @OneToOne
    @JoinColumn(name = "person_id") // 외래 키
    private Person person;
    
    // 기타 필드 및 메서드
}
```

### 4️⃣ `@ManyToMany`
- 두 엔티티가 서로 다대다 관계를 가지는 단방향 관계.
- 중간 테이블을 통해 연결되며, 한쪽에서만 다른 쪽을 참조.

#### 👉 예제.
- **Student와 Course 관계 :** 한 학생(Student)이 여러 수업(Course)을 듣는 경우.
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
        inverseJoinColumns = @JoinColumn(name = "courser_id")
    )
    private List<Course> courses = new ArrayList<>();
    
    // 기타 필드 및 메서드
}
```

## 3️⃣ 단방향 관계의 장단점.

### 1️⃣ 장점.

#### 1️⃣ 설계 및 유지보수가 간단.
- 관계가 한쪽 방향으로만 설정되므로 복잡도가 줄어듭니다.

#### 2️⃣ 성능 최적화.
- 양방향 관계보다 데이터 로딩이나 관리 비용이 낮습니다.
- 불필요한 연관 데이터 로딩을 방지할 수 있습니다.

#### 3️⃣ 의존성 감소.
- 두 엔티티 간의 의존성이 낮아져 설계가 유연합니다.

### 2️⃣ 단점.

#### 1️⃣ 탐색 방향 제한.
- 관계의 방향이 한쪽으로만 설정되므로, 반대 방향으로 데이터를 탐색하려면 별도의 쿼리가 필요합니다.
    - 예: Order에서 Customer는 참조 가능하지만, Customer에서 Order는 참조 불가능.

#### 2️⃣ 추가 요구사항에 대한 확장성 제한.
- 단방향 관계만으로 충분하지 않은 경우, 추가적으로 양방향 관계를 설정해야 할 수 있습니다.

## 4️⃣ 단방향 관계 사용 시점.

### 1️⃣ 데이터 탐색 방향이 한쪽으로만 필요한 경우.
- 예: 자식 -> 부모(주문에서 고객을 참조)

### 2️⃣ 양방향 관계가 불필요한 경우.
- 반대 방향 탐색이 요구되지 않으며, 간단한 설계를 원할 때.

### 3️⃣ 성능 최적화가 중요한 경우,
- 불필요한 연관 데이터 로딩이나 복잡한 관계를 줄이고 싶은 경우.

## 5️⃣ 단방향 관계와 양방향 관계 비교.

| 특징         | 단방향 관계                         | 양방향 관계                             |
| ------------ | ----------------------------------- | --------------------------------------- |
| 참조 방향    | 한쪽에서만 참조 가능                | 양쪽에서 참조 가능                      |
| 설계 복잡성  | 간단                                | 복잡                                    |
| 사용 사례    | 단순한 관계에서 사용                | 부모와 자식 모두 탐색이 필요한 경우     |
| 외래 키 관리 | 외래 키는 관계 설정된 쪽에서만 관리 | 외래 키는 관계 주인이 관리              |
| 성능         | 낮은 연산 비용                      | 추가 쿼리 및 연관 데이터 로딩 비용 증가 |

## 6️⃣ 결론.
- **단방향 관계**는 설계가 간단하고 유지보수가 쉬우며, 탐색 방향이 한쪽으로만 필요한 경우에 사용됩니다.
- JPA에서는 `@ManyToOne`, `@OneToMany`, `@OneToOne`, `@ManyToMany` 어노테이션을 활용하여 단방향 관계를 설정할 수 있습니다.
- 양방향 관계보다 관리가 용이하므로, 필요한 경우에만 양방향 관계를 선택하는 것이 좋습니다.
