---
layout: post
title: "💾 논리 연산 - 불리언 대수"
date: 2025-09-14 09:00:00 +0900
categories: [study]
tags: [CS, boolean, logic]
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 논리 연산 - 불리언 대수

> "한 권으로 읽는 컴퓨터 구조와 프로그래밍" 중 불리언 대수 파트 요약

---

## 기본 개념

### 불리언 대수란?
1800년대 영국 수학자 조지 불(George Boole)이 만들어낸 **불리언 대수(Boolean algebra)**는 비트에 대해 사용할 수 있는 연산 규칙의 집합입니다.

### 불리언 대수의 특징
- **일반 대수와의 유사성**: 결합 법칙, 교환 법칙, 분배 법칙 적용 가능
- **비트 연산의 체계화**: 0과 1(거짓과 참)에 대한 논리적 연산 규칙 제공
- **컴퓨터 과학의 기초**: 모든 디지털 회로와 프로그래밍 논리의 근간

---

## 기본 불리언 연산자

### 연산자 개요

| 연산자 | 기호 | 의미 | 피연산자 수 |
|--------|------|------|-------------|
| **NOT** | ~ 또는 ! | 논리적 반대 | 1개 |
| **AND** | & 또는 && | 논리곱 | 2개 이상 |
| **OR** | \| 또는 \|\| | 논리합 | 2개 이상 |
| **XOR** | ^ | 배타적 논리합 | 2개 |

### NOT 연산 (논리적 반대)

| 입력 | 출력 |
|------|------|
| 0 (거짓) | 1 (참) |
| 1 (참) | 0 (거짓) |

**예시:**
- NOT(거짓) = 참
- NOT(참) = 거짓

### AND 연산 (논리곱)

| A | B | A AND B |
|---|---|---------|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

**규칙**: 모든 비트가 참일 때만 결과가 참

### OR 연산 (논리합)

| A | B | A OR B |
|---|---|--------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

**규칙**: 어느 한 비트라도 참이면 결과가 참

### XOR 연산 (배타적 논리합)

| A | B | A XOR B |
|---|---|---------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

**규칙**: 두 비트가 다른 값일 때만 결과가 참

---

## 프로그래밍에서의 활용

### Java에서의 불리언 연산

```java
// 논리 연산자 사용 예시
boolean isRaining = true;
boolean isCold = false;
boolean hasUmbrella = true;

// AND 연산
boolean takeUmbrella = isRaining && hasUmbrella;  // true

// OR 연산
boolean wearCoat = isRaining || isCold;  // true

// NOT 연산
boolean goOutside = !isRaining;  // false

// XOR 연산 (비트 단위)
int result = 5 ^ 3;  // 6 (101 XOR 011 = 110)
```

### 조건부 로직에서의 활용

```java
// 복합 조건 검사
public boolean canProcessOrder(Order order) {
    return order.isValid() && 
           order.hasPayment() && 
           !order.isCancelled();
}

// 권한 검사
public boolean hasAccess(User user, Resource resource) {
    return user.isAdmin() || 
           (user.isActive() && resource.isPublic());
}
```

---

## 불리언 대수의 법칙

### 기본 법칙들

| 법칙 | 공식 | 설명 |
|------|------|------|
| **교환 법칙** | A AND B = B AND A | 순서를 바꿔도 결과 동일 |
| **결합 법칙** | (A AND B) AND C = A AND (B AND C) | 그룹화 순서 무관 |
| **분배 법칙** | A AND (B OR C) = (A AND B) OR (A AND C) | 곱셈과 덧셈의 분배와 유사 |

### 특수 법칙들

```
항등 법칙:
A AND 1 = A
A OR 0 = A

영 법칙:
A AND 0 = 0
A OR 1 = 1

보수 법칙:
A AND NOT(A) = 0
A OR NOT(A) = 1
```

---

## 실생활 적용 예시

### 웹 애플리케이션 접근 제어

```java
public class AccessControl {
    public boolean canAccessAdminPanel(User user) {
        return user.isAdmin() && 
               user.isActive() && 
               !user.isLocked();
    }
    
    public boolean canViewContent(User user, Content content) {
        return content.isPublic() || 
               (user.isLoggedIn() && content.isUserContent(user));
    }
}
```

### 데이터 유효성 검사

```java
public boolean isValidEmail(String email) {
    return email != null && 
           email.contains("@") && 
           !email.isEmpty() && 
           email.length() > 5;
}
```

---

## 비트 단위 연산의 활용

### 플래그 관리

```java
// 권한 플래그 정의
public static final int READ = 1;    // 001
public static final int WRITE = 2;   // 010
public static final int EXECUTE = 4; // 100

// 권한 설정
int permissions = READ | WRITE;  // 011

// 권한 확인
boolean canRead = (permissions & READ) != 0;    // true
boolean canWrite = (permissions & WRITE) != 0;  // true
boolean canExecute = (permissions & EXECUTE) != 0; // false

// 권한 제거
permissions = permissions & ~WRITE;  // READ만 남김
```

---

## 핵심 포인트

1. **체계성**: 불리언 대수는 논리 연산에 대한 완전한 수학적 체계
2. **일관성**: 일반 대수의 법칙들이 불리언 대수에도 적용됨
3. **실용성**: 프로그래밍의 조건문과 비트 연산의 기초
4. **확장성**: 기본 연산자들을 조합하여 복잡한 논리 구조 표현 가능
5. **효율성**: 컴퓨터 하드웨어에서 직접 구현되어 매우 빠른 연산 속도

불리언 대수는 디지털 시대의 모든 논리적 사고와 컴퓨터 연산의 근본 원리로, 프로그래밍에서 조건 판단과 데이터 처리의 핵심 도구입니다.
