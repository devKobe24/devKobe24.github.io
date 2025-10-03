---
title: "🔍[Troubleshooting] 🚀 엔티티 관계 설정"
tags:
    - Troubleshooting
    - Backend Development
    - Spring Boot
date: "2025-10-03"
thumbnail: "/assets/img/thumbnail/troubleshooting.jpg"
---

# 🚀 엔티티 관계 설정!

## 🎯 핵심 결론

**`@ManyToOne` 관계가 올바른 설계입니다.**

`@OneToOne`은 이 상황에 적합하지 않으며, 여러 학생이 하나의 전공에 속할 수 있는 `@ManyToOne` 관계를 사용해야 합니다.

---

## 🔍 관계 타입 비교

### @OneToOne (일대일) - ❌ 부적절

```
학생 ←→ 전공
(1:1 관계)

예시:
[강민성] ←→ [컴퓨터공학과]
   ↓
문제: 컴퓨터공학과에 강민성 한 명만 속할 수 있음
     다른 학생은 컴퓨터공학과를 선택할 수 없음!
```

#### 특징
- 하나의 학생 → 하나의 전공
- 하나의 전공 → 하나의 학생만 가능
- **현실 세계와 맞지 않음**

---

### @ManyToOne (다대일) - ✅ 적절

```
여러 학생 → 하나의 전공
(N:1 관계)

예시:
[강민성] ──┐
[김철수] ──┼→ [컴퓨터공학과]
[이영희] ──┘

각 학생은 하나의 전공에 속함
하나의 전공에 여러 학생이 속할 수 있음
```

#### 특징
- 여러 학생(`Many`) → 하나의 전공(`One`)
- 각 학생은 하나의 전공만 보유
- **현실 세계 비즈니스 로직과 일치**

---

## 📊 관계 비교표

| 구분 | @OneToOne | @ManyToOne |
|------|-----------|------------|
| 관계 | 1:1 | N:1 |
| 전공당 학생 수 | 1명 | 여러 명 |
| 학생당 전공 수 | 1개 | 1개 |
| 현실성 | ❌ 비현실적 | ✅ 현실적 |
| 사용 예시 | 주민등록번호, 여권번호 | 학생-전공, 주문-고객 |

---

## 🔨 올바른 구현 방법

### Step 1: Student 엔티티 수정

기존 `String major` 필드를 `Major` 엔티티 참조로 변경합니다.

#### Before
```java
@Entity
public class Student {
    // ...
    private String major;  // ❌ 단순 문자열
}
```

#### After
```java
package com.kobe.schoolmanagement.domain.entity;

import jakarta.persistence.*;
import lombok.AccessLevel;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;

@Entity
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Student {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String studentId;
    private int admissionYear;

    // ✅ @ManyToOne 관계 설정
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "major_id")  // DB 외래키 컬럼명
    private Major major;

    @Builder
    public Student(String name, String studentId, int admissionYear, Major major) {
        this.name = name;
        this.studentId = studentId;
        this.admissionYear = admissionYear;
        this.major = major;
    }
}
```

#### 💡 핵심 어노테이션 설명

```java
@ManyToOne(fetch = FetchType.LAZY)
// └─ FetchType.LAZY: 필요할 때만 Major 정보를 로딩 (성능 최적화)
//    FetchType.EAGER: 즉시 로딩 (N+1 문제 발생 가능)

@JoinColumn(name = "major_id")
// └─ 외래키 컬럼명을 "major_id"로 지정
//    데이터베이스 테이블에서 major_id 컬럼이 생성됨
```

---

### Step 2: Major 엔티티 (참고)

```java
@Entity
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Major {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String majorNumber;
    private String name;
    
    // 양방향 관계가 필요한 경우 (선택사항)
    // @OneToMany(mappedBy = "major")
    // private List<Student> students = new ArrayList<>();
    
    @Builder
    public Major(String majorNumber, String name) {
        this.majorNumber = majorNumber;
        this.name = name;
    }
}
```

---

### Step 3: DTO를 통한 응답 구성

엔티티를 API 응답으로 직접 노출하는 것은 위험하므로, **DTO 변환 패턴**을 사용합니다.

```java
package com.kobe.schoolmanagement.dto.response;

import com.fasterxml.jackson.annotation.JsonProperty;
import com.kobe.schoolmanagement.domain.entity.Student;
import lombok.Builder;
import lombok.Getter;

@Getter
@Builder
public class StudentResponseDto {
    
    private String name;
    private int admissionYear;

    @JsonProperty("major_info")
    private MajorInfoDto majorInfo;

    /**
     * Student 엔티티를 StudentResponseDto로 변환
     * Major 엔티티는 MajorInfoDto로 변환하여 중첩 구조 생성
     */
    public static StudentResponseDto fromEntity(Student student) {
        return StudentResponseDto.builder()
                .name(student.getName())
                .admissionYear(student.getAdmissionYear())
                // ✅ @ManyToOne으로 설정된 major 필드 활용
                .majorInfo(MajorInfoDto.fromEntity(student.getMajor()))
                .build();
    }
}
```

---

## 🗄️ 데이터베이스 구조

### 생성되는 테이블 구조

#### Student 테이블
```sql
CREATE TABLE student (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255),
    student_id VARCHAR(255),
    admission_year INT,
    major_id BIGINT,  -- ✅ 외래키
    FOREIGN KEY (major_id) REFERENCES major(id)
);
```

#### Major 테이블
```sql
CREATE TABLE major (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    major_number VARCHAR(255),
    name VARCHAR(255)
);
```

### 데이터 예시

**Major 테이블**
| id | major_number | name |
|----|--------------|------|
| 1 | 31513162120518 | Computer |
| 2 | 31513162120519 | Mathematics |

**Student 테이블**
| id | name | student_id | admission_year | major_id |
|----|------|------------|----------------|----------|
| 1 | 강민성 | 1003001 | 2010 | 1 |
| 2 | 김철수 | 1003002 | 2010 | 1 |
| 3 | 이영희 | 1003003 | 2011 | 2 |

→ major_id가 1인 학생이 여러 명 (N:1 관계 구현)

---

## ✅ 예상 결과

### API 응답 예시
```json
{
    "name": "강민성",
    "admissionYear": 2010,
    "major_info": {
        "id": 1,
        "majorNumber": "31513162120518",
        "name": "Computer"
    }
}
```

---

## ⚠️ 주의사항

### 1. 엔티티 직접 노출 금지
```java
// ❌ 나쁜 예시
@GetMapping("/{id}")
public ResponseEntity<Student> getStudent(@PathVariable Long id) {
    return ResponseEntity.ok(studentService.getStudent(id));
}

// ✅ 좋은 예시
@GetMapping("/{id}")
public ResponseEntity<StudentResponseDto> getStudent(@PathVariable Long id) {
    Student student = studentService.getStudent(id);
    return ResponseEntity.ok(StudentResponseDto.fromEntity(student));
}
```

#### 엔티티 직접 노출의 문제점
- **순환 참조**: 양방향 관계 시 무한 루프 발생
- **보안**: 민감한 정보 노출 위험
- **성능**: 불필요한 정보까지 모두 전송
- **유연성**: API 스펙 변경이 엔티티 변경으로 이어짐

---

### 2. FetchType 선택

```java
// ✅ 권장: LAZY (지연 로딩)
@ManyToOne(fetch = FetchType.LAZY)
private Major major;

// 필요한 시점에만 로딩
Student student = studentRepository.findById(1L);
// 이 시점에는 major 정보가 로딩되지 않음 (Proxy 객체)

String majorName = student.getMajor().getName();
// 이 시점에 major 정보를 DB에서 가져옴
```

```java
// ⚠️ 주의: EAGER (즉시 로딩)
@ManyToOne(fetch = FetchType.EAGER)
private Major major;

// N+1 문제 발생 가능
// 학생 100명 조회 시 → Major 조회 쿼리 100번 추가 실행
```

---

## 📌 Best Practices

### 1. 관계 설정 원칙
| 관계 | 사용 시기 | 예시 |
|------|----------|------|
| `@OneToOne` | 정말 1:1 관계일 때만 | 회원-회원상세정보 |
| `@ManyToOne` | N:1 관계 (가장 흔함) | 학생-전공, 주문-고객 |
| `@OneToMany` | 1:N 관계 (양방향 필요 시) | 전공-학생 목록 |
| `@ManyToMany` | M:N 관계 (중간 테이블 필요) | 학생-수강과목 |

### 2. DTO 변환 레이어
```
Controller Layer
    ↓ (DTO)
Service Layer
    ↓ (Entity)
Repository Layer
    ↓ (DB)
```

### 3. 단방향 vs 양방향
```java
// 단방향 (권장)
// Student → Major 참조만 존재
@Entity
public class Student {
    @ManyToOne
    private Major major;
}

// 양방향 (필요시에만)
// Student ↔ Major 양쪽 참조
@Entity
public class Major {
    @OneToMany(mappedBy = "major")
    private List<Student> students;
}
```

**단방향을 기본으로 하고, 정말 필요한 경우에만 양방향 관계를 추가하세요.**
