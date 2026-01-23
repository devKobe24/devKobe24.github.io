---
title: "📚[Backend Development] 🚀 @NoArgsConstructor - Lombok"
tags:
  - Backend Development
  - Server
  - Lombok

date: "2026-01-23"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 🎯 @NoArgsConstructor 완벽 이해하기

> "왜 Entity에 빈 생성자를 넣으라고 하는 거죠?" 🤔
> 실무에서 가장 많이 받는 질문 중 하나입니다. 지금부터 속 시원히 알려드릴게요!

---

## 🔍 @NoArgsConstructor가 뭔가요?

**한 줄 요약:** 파라미터 없는 기본 생성자를 자동으로 만들어주는 Lombok 어노테이션

```java
@NoArgsConstructor
public class Member {
    private String name;
}
```

**⬇️ Lombok이 컴파일 시 이렇게 변환**

```java
public class Member {
    private String name;
    
    public Member() {  // 👈 이게 자동으로 생성됨!
    }
}
```

---

## 💡 왜 필요한가요? (실전 시나리오)

### 1️⃣ **JPA Entity - 가장 중요! ⭐⭐⭐**

**상황:** 첫 프로젝트에서 Entity 만들고 실행했더니...

```
❌ org.hibernate.InstantiationException: 
No default constructor for entity: Member
```

**이유:** JPA(Hibernate)가 DB에서 데이터를 가져와 객체로 만들 때  
👉 **리플렉션으로 기본 생성자를 호출**하기 때문!

**해결:**

```java
@Entity
@NoArgsConstructor(access = AccessLevel.PROTECTED)  // 👈 이것!
@Getter
public class Member {
    @Id @GeneratedValue
    private Long id;
    private String name;
}
```

**🤔 왜 PROTECTED?**
- ✅ JPA는 내부에서 접근 가능
- ✅ 외부에서 `new Member()` 무분별한 생성 방지
- ✅ 도메인 무결성 보호

---

### 2️⃣ **JSON ↔ 객체 변환 (Spring API)**

**상황:** API 요청을 받는데 자꾸 에러가...

```java
@PostMapping("/members")
public void save(@RequestBody MemberDto dto) {
    // dto가 null이거나 필드가 비어있음 😱
}
```

**Jackson의 동작 방식:**
```
1. 기본 생성자로 빈 객체 생성 ⬅️ 여기서 필요!
2. JSON 데이터를 setter/reflection으로 주입
```

**해결:**

```java
@NoArgsConstructor  // 👈 필수!
@Getter @Setter
public class MemberDto {
    private String name;
    private int age;
}
```

---

### 3️⃣ **프레임워크가 객체를 대신 생성하는 경우**

다음 기술들은 모두 내부적으로 기본 생성자를 사용해요:

- 🍃 Spring Framework (Bean 생성)
- 🔥 Hibernate (Proxy 생성)
- 📊 MyBatis (ResultMap)
- 🔍 QueryDSL (Q클래스)
- 🧪 테스트 프레임워크 (Mock 객체)

**핵심 원리:**  
> "우리가 `new` 하지 않고, 프레임워크가 대신 생성한다"

---

## ⚠️ 주의! 이럴 땐 사용하지 마세요

### ❌ 불변 객체 / 필수 필드가 있는 도메인 객체

```java
// 나쁜 예 ❌
@NoArgsConstructor
public class Order {
    private final Long memberId;  // final인데 초기화 안 됨!
    private final OrderStatus status;
}
```

**문제점:**  
- 불완전한 객체가 만들어질 수 있음
- 비즈니스 규칙이 깨짐

**올바른 방법 ✅**

```java
@RequiredArgsConstructor  // final 필드만 생성자에 포함
public class Order {
    private final Long memberId;
    private final OrderStatus status;
}
```

---

## 🏆 실무 Best Practice

### 패턴 1: JPA Entity (가장 많이 쓰이는 방식)

```java
@Entity
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)  // ⭐
public class Member {

    @Id @GeneratedValue
    private Long id;
    
    private String name;
    private String email;

    // 정적 팩토리 메서드 or 빌더 패턴 권장
    @Builder
    public Member(String name, String email) {
        this.name = name;
        this.email = email;
    }
}
```

**✨ 이 패턴의 장점:**
- ✅ JPA 요구사항 충족
- ✅ 무분별한 `new Member()` 방지
- ✅ 생성 로직을 빌더나 팩토리로 제어
- ✅ 도메인 무결성 유지

---

### 패턴 2: DTO (API 요청/응답)

```java
@NoArgsConstructor  // Jackson을 위해 필요
@AllArgsConstructor  // 테스트 코드 작성 편의
@Getter @Setter
public class MemberRequestDto {
    private String name;
    private String email;
}
```

---

## 🎯 한 줄로 정리하면?

**@NoArgsConstructor는 "프레임워크가 객체를 생성해야 할 때" 필요하다!**

---

## 📝 체크리스트로 기억하기

| 상황 | @NoArgsConstructor 필요? | 접근 제어자 |
|------|------------------------|-----------|
| 🗄️ JPA Entity | ✅ 필수 | `PROTECTED` |
| 📮 API DTO (요청) | ✅ 필수 | `PUBLIC` |
| 📤 API DTO (응답) | ⚠️ 상황에 따라 | `PUBLIC` |
| 🎯 도메인 객체 | ❌ 지양 | - |
| 🧱 불변 객체 | ❌ 불가 | - |

---

## 💬 마지막 팁

```java
// 두 개를 함께 쓰는 경우도 많아요!
@NoArgsConstructor(access = AccessLevel.PROTECTED)  // JPA용
@AllArgsConstructor(access = AccessLevel.PRIVATE)   // Builder용
@Builder
@Entity
public class Member {
    // ...
}
```
