---
title: 🍃[Spring] JPA 연관관계에 대한 추가적인 기능들에는 무엇이 있을까요? - N:M 관계
tags:
    - Spring
    - Framework
date: "2024-11-11"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] JPA 연관관계에 대한 추가적인 기능들에는 무엇이 있을까요? - N:M 관계
- JPA에서 **N:M 관계**란 두 엔티티가 **다대다(Many-to-Many)로** 연결된 관계를 의미합니다.
    - 예를 들어, 학생(Student)과 강의(Course) 관계에서 하나의 학생이 여러 강의를 수강할 수 있고, 동시에 하나의 강의에 여러 학생이 수강할 수 있는 경우에 **N:M 관계**가 성립됩니다.

## 1️⃣ N:M 관계의 매핑 방식.
- JPA에서는 `@ManyToMany` 어노테이션을 사용하여 N:M 관계를 매핑할 수 있습니다.
    - N:M 관계에서는 **연결 테이블(Join Table)이** 필요하며, JPA는 연결 테이블을 자동으로 생성하거나 개발자가 직접 정의할 수 있습니다.

## 2️⃣ N:M 관계의 기본 매핑.
- N:M 관계는 단순히 `@ManyToMany` 어노테이션을 사용하는 방식과, **연결 테이블을 엔티티로 분리하여 사용하는 방식 두 가지가 있습니다.**

### 1️⃣ 기본적인 `@ManyToMany` 매핑
```java
@Entity
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @ManyToMany
    @JoinTable(
        name = "student_course", // 연결 테이블 이름
        joinColumns = @JoinColumn(name = "student_id"), // 현재 엔티티(Student)의 외래 키
        inverseJoinColumns = @JoinColumn(name = "course_id") // 반대 엔티티(Course)의 외래 키
    )
    private List<Course> courses = new ArrayList<>();
}

@Entity
public class Course {
    @Id
    @GeneratedValue(strategy = GeneratioonType.IDENTITY)
    private Long id;
    
    private String title;
    
    @ManyToMany(mappedBy = "course") // 반대쪽 엔티티와의 양방향 관계 설정.
    private List<Student> students = new ArrayList<>();
}
```

### 👉 설명.
- `@ManyToMany` 어노테이션을 사용해 Student와 Course 엔티티 간의 N:M 관계를 매핑합니다.
- `@JoinTable`을 사용하여 연결 테이블(student_course)을 지정하고, 각 엔티티의 외래 키를 joinColumns와 inverseJoinColumns으로 설정합니다.
- Student와 Course 간의 양방향 관계로 설정되었으며, mappedBy를 사용해 반대쪽의 매핑 필드를 지정합니다.

## 3️⃣ 연결 엔티티를 사용한 N:M 매핑.
- `@ManyToMany` 관계는 간단한 경우에만 사용하는 것이 좋습니다.
    - 관계가 복잡하거나 **추가적인 속성이 필요한 경우 연결 테이블을 별도의 엔티티로 분리하여 N:1 및 1:N 관계로 매핑하는 방식이 선호됩니다.**
        - 예를 들어, 학생과 강의 사이에 학점(Grade)과 같은 속성이 필요하다면, 연결 엔티티인 Enrollment를 추가합니다.
```java
@Entity
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @OneToMany(mappedBy = "student")
    private List<Enrollment> enrollments = new ArrayList<>();
    
    // getter, setter
}

@Entity
public class Course {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String title;
    
    @OneToMany(mappedBy = "course")
    private List<Enrollment> enrollments = new ArrayList<>();
    
    // getter, setter
}

@Entity
public class Enrollment {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String grade;
    
    @ManyToOne
    @JoinColumn(name = "student_id")
    private Student student;
    
    @ManyToOne
    @JoinColumn(name = "course_id")
    private Course course;
    
    // getter, setter
}
```

### 👉 설명.
- Enrollment 엔티티를 추가하여 Student와 Course 간의 연결을 표현합니다.
    - 이제 Student와 Course는 Enrollment와 N:1 관계를 가집니다.
- 이 방식은 연결 엔티티에 추가적인 필드(예: grade)를 포함할 수 있어 유연합니다.

## 4️⃣ N:M 관계 사용 시 주의사항.
- 직접적인 `@ManyToMany` 관계는 단순한 관계에서만 사용하며, 복잡한 비즈니스 요구사항이 있는 경우 연결 테이블을 엔티티로 만들어 처리합니다.
- N:M 관계는 조인 테이블을 이용하므로, 데이터의 양이 많아지면 성능 저하를 일으킬 수 있습니다.
    - 지연 로딩(Lazy Loading) 설정을 통해 성능을 최적화합니다.
- 복잡한 관계에서의 Cascade 옵션은 신중하게 설정해야 불필요한 연관 엔티티의 저장/삭제를 방지할 수 있습니다.

## 5️⃣ 요약.
- N:M 관계는 `@ManyToMany` 어노테이션으로 쉽게 매핑할 수 있지만, 추가적인 속성이 필요하거나 관계가 복잡해지면 연결 테이블을 별도 엔티티로 분리하여 처리하는 것이 좋습니다.
    - 연결 엔티티를 분리하면 추가 속성을 포함할 수 있으며, 더 유연한 데이터 모델링이 가능합니다.
