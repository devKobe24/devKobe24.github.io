---
title: "🔍[Troubleshooting] 🚀 DTO 설계"
tags:
    - Troubleshooting
    - Backend Development
    - Spring Boot
date: "2025-10-03"
thumbnail: "/assets/img/thumbnail/troubleshooting.jpg"
---

# 🚀 DTO 설계!

## 🎯 핵심 이슈

**생성(Create) 시 사용하던 `StudentRequestDto`를 수정(Update) API에서 재사용**하고 있어, API 사용자에게 혼란을 줄 수 있습니다.

---

## ⚠️ 현재 코드의 문제점

### 1. API 의도가 불명확
```java
// StudentRequestDto는 여러 필드를 포함
- name
- admissionYear
- majorName
- doubleMajorName
```

**API 사용자의 혼란**
- "이름만 보내면 되나?"
- "다른 필드들도 채워야 하나?"
- "null로 보내면 어떻게 되지?"

### 2. 불필요한 데이터 전송
클라이언트가 이름만 수정하고 싶어도, 다른 필드들을 JSON에 포함할 수 있습니다.

```json
// 불필요하게 복잡한 요청
{
  "name": "홍길동",
  "admissionYear": 2024,    // 불필요
  "majorName": "컴퓨터공학",   // 불필요
  "doubleMajorName": null   // 불필요
}
```

### 3. 유지보수 리스크
`StudentRequestDto`에 새로운 필수 필드가 추가되면?

```java
public class StudentRequestDto {
    @NotNull  // 새로운 필수 필드 추가
    private String phoneNumber;
    
    // 기존 필드들...
}
```

→ **이름만 수정하는 API가 의도치 않게 영향을 받아 오류 발생!**

---

## ✅ 해결 방안: 목적별 DTO 분리

### 설계 원칙
> **하나의 API는 하나의 명확한 계약(Contract)을 가져야 합니다.**

"학생 이름 수정"이라는 단일 목적에 맞는 전용 DTO를 생성합니다.

---

## 🔧 구현 가이드

### Step 1: 전용 DTO 생성

**`UpdateStudentNameRequestDto.java` (신규 생성)**

```java
package com.kobe.schoolmanagement.dto.request;

import jakarta.validation.constraints.NotBlank;
import lombok.Getter;
import lombok.NoArgsConstructor;

@Getter
@NoArgsConstructor  // JSON 역직렬화를 위한 기본 생성자
public class UpdateStudentNameRequestDto {

    @NotBlank(message = "이름은 비워둘 수 없습니다.")
    private String name;
}
```

**주요 특징**
- ✅ 필요한 필드만 포함 (`name` 단일 필드)
- ✅ 유효성 검증 규칙 명확 (`@NotBlank`)
- ✅ 목적이 명확 (이름 수정 전용)

---

### Step 2: Controller 수정

**Before (현재)**
```java
@PatchMapping("/change/student/name/{studentId}")
public ResponseEntity<StudentResponseDto> changeStudentName(
    @PathVariable String studentId,
    @RequestBody StudentRequestDto requestDto  // 범용 DTO 사용
) {
    return ResponseEntity.ok(studentService.updateStudentName(studentId, requestDto));
}
```

**After (개선)**
```java
import com.kobe.schoolmanagement.dto.request.UpdateStudentNameRequestDto;

@PatchMapping("/change/student/name/{studentId}")
public ResponseEntity<StudentResponseDto> changeStudentName(
    @PathVariable String studentId,
    @RequestBody @Valid UpdateStudentNameRequestDto requestDto  // 전용 DTO + 검증
) {
    return ResponseEntity.ok(studentService.updateStudentName(studentId, requestDto));
}
```

**변경 포인트**
- `StudentRequestDto` → `UpdateStudentNameRequestDto`
- `@Valid` 어노테이션 추가로 유효성 검증 활성화

---

### Step 3: Service 수정

**Before (현재)**
```java
@Transactional
public StudentResponseDto updateStudentName(String studentId, StudentRequestDto requestDto) {
    Student findStudent = studentRepository.findByStudentId(studentId)
        .orElseThrow(() -> new IllegalArgumentException("존재하지 않는 학생입니다."));

    findStudent.updateStudentName(requestDto.getName());

    return StudentResponseDto.fromEntity(findStudent);
}
```

**After (개선)**
```java
import com.kobe.schoolmanagement.dto.request.UpdateStudentNameRequestDto;

@Transactional
public StudentResponseDto updateStudentName(String studentId, UpdateStudentNameRequestDto requestDto) {
    Student findStudent = studentRepository.findByStudentId(studentId)
        .orElseThrow(() -> new IllegalArgumentException("존재하지 않는 학생입니다."));

    findStudent.updateStudentName(requestDto.getName());

    return StudentResponseDto.fromEntity(findStudent);
}
```

**변경 포인트**
- 파라미터 타입만 `UpdateStudentNameRequestDto`로 변경
- 나머지 로직은 동일

---

## 📊 개선 효과 비교

| 항목 | Before | After |
|------|--------|-------|
| **API 명확성** | ❌ 어떤 필드가 필요한지 불명확 | ✅ name만 필요함을 명확히 표현 |
| **데이터 전송** | ❌ 불필요한 필드 포함 가능 | ✅ 필요한 데이터만 전송 |
| **유지보수** | ❌ 다른 API 변경에 영향받음 | ✅ 독립적으로 관리 가능 |
| **유효성 검증** | ⚠️ 암묵적 | ✅ 명시적 (`@NotBlank`) |

---

## 💡 API 요청 예시

### 개선 후 클라이언트 요청
```json
{
  "name": "홍길동"
}
```

**장점**
- 간결하고 명확한 요청 구조
- API 의도가 한눈에 파악됨
- 불필요한 필드 전송 제거

---

## 📝 설계 원칙 정리

### DTO 설계 시 고려사항

1. **단일 책임 원칙**
   - 하나의 DTO는 하나의 명확한 목적을 가져야 함

2. **명시적 계약**
   - API 사용자가 어떤 데이터를 보내야 하는지 명확히 알 수 있어야 함

3. **독립성**
   - 다른 API의 변경이 영향을 주지 않도록 독립적으로 관리

4. **유효성 검증**
   - DTO 레벨에서 명시적으로 검증 규칙 정의

---

## 🎯 결론

**목적에 맞는 전용 DTO를 사용하면:**
- API 계약이 명확해집니다
- 다른 개발자의 혼란을 방지합니다
- 안전하고 유지보수하기 좋은 코드가 됩니다

이는 단순히 코드를 더 쓰는 것이 아니라, **더 나은 설계**를 하는 것입니다. 👍
