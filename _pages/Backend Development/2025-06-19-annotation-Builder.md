---
title: "📚[Backend Development] @Builder 애너테이션"
tags:
    - Backend Ddevelopment
    - Database
    - Server
    - Build System
date: "2025-06-19"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 📚[Backend Development] @Builder 애너테이션
`@Builder` 애너테이션은 **Lombok** 라이브러리가 제공하는 기능으로, Java에서 **Builder 패턴**을 쉽게 구현할 수 있도록 도와줍니다.

## ✅ 1. 빌더 패턴이란?
**Builder 패턴은 복잡한 객체를 단계적으로 생성할 수 있게 하는 생성 디자인 패턴입니다.** 
- 생성자나 setter 대신 사용
- 선택적 필드, 불변성, 가독성 향상
- 특히 파라미터가 많은 객체를 생성할 때 유용함

예:
```java
LoginUser user = LoginUser.builder()
    .email("user@example.com")
    .nickname("user")
    .age(25)
    .build();
```

## ✅ 2. Lombok의 `@Builder`로 빌더 패턴 적용 예제.
### 🧩 엔티티 예시
```java
@Getter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class LoginUser {
    private String email;
    private String nickname;
    private int age;
}
```

### 🧩 사용법
```java
LoginUser user = LoginUser.builder()
    .email("user@example.com")
    .nickname("Kobe")
    .age(28)
    .build();
```
- 순서 상관없이 필드 명시 가능
- 가독성 뛰어남
- null-safe 하게 생성 가능

## ✅ 3. `@Builder`의 특징 및 내부 동작
- 내부적으로 **정적 빌더 클래스(LoginUserBuilder)를 생성**
- 각 필드는 withXxx() 메서드 형태로 설정 가능
- build() 메서드가 최종 객체를 반환

**빌더 메서드 예시 (Lombok이 내부 생성하는 구조):**
```java
public class LoginUser {
    public static class LoginUserBuilder {
        private String email;
        private String nickname;
        private int age;
        
        public LoginUserBuilder email(String email) {
            this.email = email;
            return this;
        }
        
        public LoginUser build() {
            return new LoginUser(email, nickname, age);
        }
    }
}
```

## ✅ 4. `@Builder`의 사용 주의사항.

| 상황 | 주의점 |
| -------- | -------- |
| `@Builder` + JPA Entity | 생성자에 `@Builder`를 붙이는 방식 추천(기본 생성자 필요) |
| `@Builder` + Default 값 설정 | 필드 초기화 시 `@Builder.Default`를 함께 사용해야 적용됨 |
| Setter 없음 | 불변 객체처럼 사용할 수 있어 안정성 ↑ |

## ✅ 5. `@Builder.Default` 예시.
```java
@Builder
public class LoginUser {
    private String email;
    
    @Builder.Default
    private String role = "USER"; // 기본값 설정
}
```

## ✅ 요약

| 장점 | 설명 |
| -------- | -------- |
| 가독성 | 필드명 기반으로 객체 생성 가능 |
| 유지보수 | 필드 순서 신경 쓸 필요 없음 |
| 안전성 | setter 없이 불변 객체처럼 사용 가능 |
| 유연성 | 선택적 필드 조합으로 생성 가능 |
