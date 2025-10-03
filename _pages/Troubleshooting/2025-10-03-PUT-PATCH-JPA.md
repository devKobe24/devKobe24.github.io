---
title: "🔍[Troubleshooting] 🚀 PUT, PATCH, JPA 변경 감지"
tags:
    - Troubleshooting
    - Backend Development
    - Spring Boot
date: "2025-10-03"
thumbnail: "/assets/img/thumbnail/troubleshooting.jpg"
---

# 🚀 PUT, PATCH, JPA 변경 감지!

## ✅ 잘 구현된 부분

### 1. 명확한 계층 구조
- **Controller**: HTTP 요청 처리
- **Service**: 비즈니스 로직 수행
- **Repository**: 데이터 접근
- **Entity**: 자체 상태 관리 (`updateStudentName`)

각 계층의 책임이 명확하게 분리되어 있습니다.

### 2. 적절한 예외 처리
```java
studentRepository.findByStudentId(studentId)
    .orElseThrow(() -> new IllegalArgumentException("존재하지 않는 학생입니다."));
```
존재하지 않는 리소스에 대한 수정을 사전에 방지합니다.

### 3. 직관적인 메서드 명명
`changeStudentName`처럼 기능을 명확하게 표현하는 메서드명을 사용했습니다.

---

## 🔧 개선 제안

### 제안 1: HTTP 메서드 변경 (`PUT` → `PATCH`)

#### 📌 문제점
현재 `@PutMapping`을 사용 중입니다. REST 설계 원칙에서:
- **PUT**: 리소스 전체를 교체 (모든 필드 필요)
- **PATCH**: 리소스 일부만 수정 (변경할 필드만)

현재 구현은 '이름' 필드만 수정하므로 **부분 수정**에 해당합니다.

#### ✨ 개선 코드

**Before (현재)**
```java
@PutMapping("/change/student/name/{studentId}")
public ResponseEntity<StudentResponseDto> changeStudentName(
    @PathVariable String studentId,
    @RequestBody StudentRequestDto requestDto
) {
    return ResponseEntity.ok(studentService.updateStudentName(studentId, requestDto));
}
```

**After (개선)**
```java
import org.springframework.web.bind.annotation.PatchMapping;

@PatchMapping("/change/student/name/{studentId}")
public ResponseEntity<StudentResponseDto> changeStudentName(
    @PathVariable String studentId,
    @RequestBody StudentRequestDto requestDto
) {
    return ResponseEntity.ok(studentService.updateStudentName(studentId, requestDto));
}
```

---

### 제안 2: JPA 변경 감지(Dirty Checking) 활용

#### 📌 문제점
`@Transactional` 메서드 내에서 불필요한 `save()` 호출이 있습니다.

#### 💡 핵심 개념
JPA는 영속성 컨텍스트에서 관리되는 엔티티의 변경을 자동으로 감지합니다.  
트랜잭션이 커밋될 때 변경된 내용이 자동으로 DB에 반영됩니다.

#### ✨ 개선 코드

**Before (현재)**
```java
@Transactional
public StudentResponseDto updateStudentName(String studentId, StudentRequestDto requestDto) {
    Student findStudent = studentRepository.findByStudentId(studentId)
        .orElseThrow(() -> new IllegalArgumentException("존재하지 않는 학생입니다."));

    findStudent.updateStudentName(requestDto.getName());

    // 불필요한 save 호출
    Student savedStudent = studentRepository.save(findStudent);

    return StudentResponseDto.fromEntity(savedStudent);
}
```

**After (개선)**
```java
@Transactional
public StudentResponseDto updateStudentName(String studentId, StudentRequestDto requestDto) {
    // 1. 엔티티 조회 → 영속성 컨텍스트가 관리 시작
    Student findStudent = studentRepository.findByStudentId(studentId)
        .orElseThrow(() -> new IllegalArgumentException("존재하지 않는 학생입니다."));

    // 2. 엔티티 상태 변경
    findStudent.updateStudentName(requestDto.getName());

    // 3. save() 없이도 트랜잭션 커밋 시 자동으로 UPDATE 쿼리 실행
    return StudentResponseDto.fromEntity(findStudent);
}
```

#### 📊 개선 효과
- 코드 간결성 향상
- JPA의 변경 감지 메커니즘 활용
- 불필요한 메서드 호출 제거

---

## 📝 요약

| 항목 | 현재 | 개선 후 |
|------|------|---------|
| HTTP 메서드 | `@PutMapping` | `@PatchMapping` |
| Repository 호출 | `save()` 명시적 호출 | 변경 감지로 자동 저장 |
| RESTful 설계 | 부분적으로 준수 | 완전히 준수 |
