---
title: "🔍[Troubleshooting] 🚀 Delete API"
tags:
    - Troubleshooting
    - Backend Development
    - Spring Boot
date: "2025-10-03"
thumbnail: "/assets/img/thumbnail/troubleshooting.jpg"
---

# 🚀 Delete API!

## 👍 잘 구현된 부분

### 1. 명확한 계층 분리
- Controller는 HTTP 요청 처리에 집중
- Service는 비즈니스 로직 담당
- Repository는 데이터 접근 계층으로 역할 분리
- 각 계층의 책임이 명확하게 구분되어 유지보수가 용이합니다

### 2. RESTful URI 설계
```java
@DeleteMapping("/delete/student/{studentId}")
```
- 리소스(student) 중심의 URI 설계
- 적절한 HTTP 메서드(DELETE) 사용
- RESTful 원칙을 잘 준수하고 있습니다

### 3. 트랜잭션 관리
```java
@Transactional
public void deleteStudent(String studentId) { ... }
```
- `@Transactional` 어노테이션을 통한 원자성 보장
- 삭제 작업의 안정성 확보

---

## 🤔 개선이 필요한 부분

### 1. 위험한 이중 삭제 로직

#### 📌 현재 코드의 문제점

```java
@Transactional
public void deleteStudent(String studentId) {
    // 1단계: deleteByStudentId()에서 이미 DELETE 쿼리 실행
    Student deletedStudent = studentRepository.deleteByStudentId(studentId);
    
    // 2단계: 이미 삭제된 엔티티를 다시 삭제 시도 (불필요하고 위험!)
    studentRepository.delete(deletedStudent);
}
```

**왜 문제인가?**
- `deleteBy...` 메서드는 내부적으로 DELETE 쿼리를 실행합니다
- 이미 삭제된 엔티티를 다시 삭제하려 시도하면 예외가 발생할 수 있습니다
- 불필요한 중복 로직으로 혼란을 야기합니다

#### ✅ 개선된 코드

```java
@Transactional
public void deleteStudent(String studentId) {
    // 1단계: 엔티티 존재 여부 확인
    Student student = studentRepository.findByStudentId(studentId)
        .orElseThrow(() -> new IllegalArgumentException("존재하지 않는 학생입니다."));
    
    // 2단계: 조회된 엔티티 삭제
    studentRepository.delete(student);
}
```

**개선 효과**
- 명확한 삭제 프로세스: 조회 → 검증 → 삭제
- 존재하지 않는 학생 ID 요청 시 명시적인 예외 처리
- 코드의 의도가 명확하여 유지보수성 향상

---

### 2. 부적절한 HTTP 응답 상태 코드

#### 📌 현재 코드의 문제점

```java
@DeleteMapping("/delete/student/{studentId}")
public void deleteStudent(@PathVariable String studentId) {
    studentService.deleteStudent(studentId);
    // 기본적으로 200 OK 반환
}
```

**왜 문제인가?**
- 삭제 성공 시 반환할 본문이 없는데 `200 OK`를 반환
- REST API 표준에 따르면 본문이 없을 때는 `204 No Content`가 적절

#### ✅ 개선된 코드

```java
@DeleteMapping("/delete/student/{studentId}")
public ResponseEntity<Void> deleteStudent(@PathVariable String studentId) {
    studentService.deleteStudent(studentId);
    return ResponseEntity.noContent().build(); // 204 No Content
}
```

**개선 효과**
- REST API 표준을 정확히 준수
- 클라이언트에게 명확한 의미 전달 (삭제 완료, 반환 본문 없음)
- API 문서화 시 더 명확한 스펙 제공

---

## 📝 요약

| 구분 | 내용 | 중요도 |
|------|------|--------|
| ✅ 유지 | 계층 분리, RESTful 설계, 트랜잭션 관리 | - |
| ⚠️ 수정 필수 | 이중 삭제 로직 제거 | 🔴 높음 |
| 💡 권장 | HTTP 204 상태 코드 사용 | 🟡 중간 |

### 최종 권장 사항
1. **Service 계층**: 조회 후 삭제 패턴으로 변경하여 안정성 확보
2. **Controller 계층**: `ResponseEntity<Void>`로 명시적인 204 응답 반환
3. 이를 통해 더 안전하고 표준을 준수하는 API를 제공할 수 있습니다
