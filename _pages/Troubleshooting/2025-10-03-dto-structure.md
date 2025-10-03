---
title: "🔍[Troubleshooting] 🚀 DTO 중첩 설계"
tags:
    - Troubleshooting
    - Backend Development
    - Spring Boot
date: "2025-10-03"
thumbnail: "/assets/img/thumbnail/troubleshooting.jpg"
---

# 🚀 DTO 중첩 설계!

## 🎯 목표

학생 정보 응답 시 **전공 정보를 중첩 객체로 반환**하도록 DTO 구조를 개선합니다.

### 변경 목표 JSON 구조
```json
{
    "name": "Minseong Kang",
    "admissionYear": 2010,
    "major_info": {
        "id": 1,
        "majorNumber": "31513162120518",
        "name": "Computer"
    }
}
```

---

## 📊 구조 설계

### 기존 구조 vs 개선 구조

| 구분 | 기존 | 개선 |
|------|------|------|
| 응답 필드 | `id`, `studentId`, `name`, `major` | `name`, `admissionYear`, `major_info` |
| 전공 표현 | String 타입 | 중첩 객체 (MajorInfoDto) |
| 정보 깊이 | 단일 레벨 | 2레벨 중첩 구조 |

---

## 🔨 구현 단계

### Step 1: 전공 정보 DTO 생성

새로운 `MajorInfoDto` 클래스를 생성하여 전공 상세 정보를 담습니다.

```java
package com.kobe.schoolmanagement.dto.response;

import com.kobe.schoolmanagement.domain.entity.Major;
import lombok.Builder;
import lombok.Getter;

@Getter
@Builder
public class MajorInfoDto {

    private Long id;
    private String majorNumber;
    private String name;

    /**
     * Major 엔티티를 MajorInfoDto로 변환
     * 
     * @param major 변환할 Major 엔티티
     * @return MajorInfoDto 객체
     */
    public static MajorInfoDto fromEntity(Major major) {
        return MajorInfoDto.builder()
                .id(major.getId())
                .majorNumber(major.getMajorNumber())
                .name(major.getName())
                .build();
    }
}
```

#### 💡 핵심 포인트
- **정적 팩토리 메서드 패턴** 사용 (`fromEntity`)
- 엔티티를 DTO로 변환하는 책임을 DTO에 위임
- 불변 객체 설계 (Getter만 제공)

---

### Step 2: 학생 응답 DTO 수정

기존 `StudentResponseDto`를 수정하여 중첩 구조를 반영합니다.

#### Before
```java
@Getter
@Builder
public class StudentResponseDto {
    private Long id;
    private String studentId;
    private String name;
    private String major;  // ❌ 단순 문자열
    
    // ...
}
```

#### After
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

    @JsonProperty("major_info")  // ✅ JSON 키를 스네이크 케이스로 변환
    private MajorInfoDto majorInfo;

    /**
     * Student 엔티티를 StudentResponseDto로 변환
     * 
     * @param student 변환할 Student 엔티티
     * @return StudentResponseDto 객체
     */
    public static StudentResponseDto fromEntity(Student student) {
        return StudentResponseDto.builder()
                .name(student.getName())
                .admissionYear(student.getAdmissionYear())
                .majorInfo(MajorInfoDto.fromEntity(student.getMajor()))  // ✅ 중첩 변환
                .build();
    }
}
```

#### 💡 주요 변경사항
1. **불필요한 필드 제거**: `id`, `studentId` 삭제
2. **필드 타입 변경**: `String major` → `MajorInfoDto majorInfo`
3. **JSON 필드명 매핑**: `@JsonProperty`로 스네이크 케이스 적용
4. **중첩 변환 로직**: `fromEntity` 메서드 체인 활용

---

## 🔄 데이터 변환 흐름

```
Student Entity
    ├── name: "Minseong Kang"
    ├── admissionYear: 2010
    └── major: Major Entity
             ├── id: 1
             ├── majorNumber: "31513162120518"
             └── name: "Computer"
        ↓
StudentResponseDto.fromEntity(student)
        ↓
    MajorInfoDto.fromEntity(student.getMajor())
        ↓
StudentResponseDto
    ├── name: "Minseong Kang"
    ├── admissionYear: 2010
    └── majorInfo: MajorInfoDto
             ├── id: 1
             ├── majorNumber: "31513162120518"
             └── name: "Computer"
        ↓
JSON Response (자동 직렬화)
```

---

## ✅ 예상 결과

### API 응답 예시
```json
{
    "name": "Minseong Kang",
    "admissionYear": 2010,
    "major_info": {
        "id": 1,
        "majorNumber": "31513162120518",
        "name": "Computer"
    }
}
```

---

## 📌 설계 원칙

### 1. **계층 분리**
- Entity ↔️ DTO 변환 로직을 DTO에 캡슐화
- Controller는 변환 로직을 알 필요 없음

### 2. **명확한 네이밍**
| 항목 | Java 코드 | JSON 키 |
|------|-----------|---------|
| 전공 정보 | `majorInfo` (카멜 케이스) | `major_info` (스네이크 케이스) |

### 3. **재사용성**
- `MajorInfoDto`는 다른 응답 DTO에서도 재사용 가능
- 전공 정보 표현 방식이 일관됨

### 4. **확장성**
```java
// 향후 추가 정보가 필요할 때
@JsonProperty("major_info")
private MajorInfoDto majorInfo;

@JsonProperty("course_info")  // ✅ 동일한 패턴으로 확장
private CourseInfoDto courseInfo;
```

---

## 🚀 적용 방법

### Controller에서 사용
```java
@PostMapping
public ResponseEntity<StudentResponseDto> createStudent(
    @RequestBody StudentRequestDto request
) {
    Student student = studentService.createStudent(request);
    
    // fromEntity 메서드로 간단히 변환
    return ResponseEntity.ok(
        StudentResponseDto.fromEntity(student)
    );
}
```

### 추가 작업 불필요
- Service 계층 수정 필요 없음
- Repository 계층 수정 필요 없음
- DTO 변환 로직만 수정하면 끝!
