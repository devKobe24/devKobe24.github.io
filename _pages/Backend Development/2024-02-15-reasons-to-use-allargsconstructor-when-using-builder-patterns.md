---
title: "📚[Backend Development] 빌더 패턴 사용시 @AllArgsConstructor(access = AccessLevel.PRIVATE)을 활용하는 이유"
tags:
    - Backend Ddevelopment
    - Springboot
    - Framework
date: "2025-02-15"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 빌더 패턴 사용시 @AllArgsConstructor(access = AccessLevel.PRIVATE)을 활용하는 이유"
## 🍎 Intro.
- **빌더 패턴을 사용할 때, @AllArgsConstructor(access = AccessLevel.PRIVATE)를 추가하는 이유를 설명하겠습니다.**

## ✅1️⃣ @AllArgsConstructor가 하는 역할.
- @AllArgsConstructor는 **모든 필드를 포함하는 생성자**를 자동으로 생성합니다.
- 하지만 **빌더 패턴을 사용할 경우, 생성자를 직접 호출하지 않고 빌더를 통해 객체를 생성하는 것이 목적**입니다.
- 따라서, **생성자의 접근 제한을 private으로 설정하면, 빌더를 통한 생성만 허용할 수 있습니다.**

## ✅2️⃣ 빌더 패턴 적용 시, @AllArgsConstructor(access = AccessLevel.PRIVATE)가 필요한 이유
### ❌ 잘못된 예제 (빌더 패턴 사용했지만, 생성자도 public)
```java
@AllArgsConstructor // (기본값이 PUBLIC)
@Builder
public class Comment {
    private Long commentId;
    private String content;
    private Long articleId;
    private Long parentCommentId;
    private Long writerId;
    private Boolean deleted;
    private LocalDateTime createdAt;
}
```

### ✅ 문제점:
- @AllArgsConstructor의 기본 접근 제어자가 public이므로, **빌더를 사용하지 않고 생성자를 직접 호출하여 객체를 만들 수 있음.**
- 빌더를 사용하는 목적이 **객체 생성 시 가독성을 높이고 선택적으로 필드를 초기화 할 수 있도록 하기 위함**인데, 생성자가 public이면 **빌더 사용을 강제할 수 없음.**

### 🛠️ 해결 방법(@AllArgsConstructor(access = AccessLevel.PRIVATE))

```java
@AllArgsConstructor(access = AccessLevel.PRIVATE) // 생성자를 PRIVATE으로 설정
@Builder
public class Comment {
    private Long commentId;
    private String content;
    private Long articleId;
    private Long parentCommentId;
    private Long writerId;
    private Boolean deleted;
    private LocalDateTime createdAt;
}
```

### ✅ 이렇게 하면:
- **객체를 직접 생성하는 것을 막고, 빌더를 통한 생성만 가능하도록 제한 가능.**
- **불필요한 생성자 호출을 막고, 가독정이 좋은 빌더 패턴을 강제할 수 있음.**
- **객체의 필드가 많아질수록, 빌더 패턴이 더 유용하게 동작하게 됨.**

## ✅3️⃣ 정리 - 필요한 이유.

|문제점|해결 방법|
| -------- | -------- |
|@AllArgConstructor 기본값이 public이므로, 직접 생성자 호출이 가능함.|@AllArgsConstructor(access = AccessLevel.PRIVATE)를 사용하여 생성자 접근 제한.|
|빌더를 사용해도 생성자를 직접 호출할 수 있어 일관성이 떨어짐.|빌더를 강제하여 가독성 및 유지보수성을 높임.|
|객체 필드가 많아질 경우, 생성자 호출보다 빌더 패턴이 더 유리함.|빌더를 강제하여 더 가독성이 좋은 코드 유지 가능.|

- ✅ **즉, @AllArgsCOnstructor(access = AccessLevel.PRIVATE)를 사용하면, 빌더를 통한 객체 생성을 강제하여 코드 일관성을 유지할 수 있습니다.**
