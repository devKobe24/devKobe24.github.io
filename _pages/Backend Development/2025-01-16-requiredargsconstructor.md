---
title: "📚[Backend Development] 🚀 @RequiredArgsConstructor - Lombok"
tags:
  - Backend Development
  - Server
  - Lombok

date: "2026-01-16"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 🚀 @RequiredArgsConstructor - Lombok

**`@RequiredArgsConstructor`는 클래스에서 반드시 초기화되어야 하는 필드만을 매개변수로 받는 생성자를** 자동으로 만들어 줍니다.
이때 대상이 되는 필드는 `final`로 선언된 필드이거나 `@NonNull`이 붙은 필드입니다.

또한 생성자에 어노테이션을 추가할 수 있도록 하는 `onConstructor` 옵션도 함께 지원합니다.

## 📌 핵심 요약

- final 필드 → 무조건 생성자에 포함
- @NonNull 필드 → 생성자에 포함 + null 체크
```java
// 예시
@RequiredArgsConstructor
public class Monster {

    private final String name;
    private final MonsterStage stage;

    @NonNull
    private MonsterAttribute attribute;

    private int hp;
}
```

**↓ Lombok이 생성하는 생성자**

```java
public Monster(String name,
               MonsterStage stage,
               MonsterAttribute attribute) {
    this.name = name;
    this.stage = stage;
    this.attribute = attribute;
}
```

- onConstructor → 생성자에 어노테이션 추가 기능 (예: @Autowired)
```java
// 예시
@RequiredArgsConstructor(onConstructor_ = @Autowired)
public class MonsterService {
    private final MonsterRepository repository;
}
```

## 🚨 주의할 점 (중요)

**"모든 생성자를 대체한다"는 개념은 아님**
- ❌ 선택 필드가 많을 때는 부족할 수 있음
- ❌ 생성자 내부에서 검증 로직을 넣기 어려움
- ❌ Entity에서 무분별하게 쓰면 도메인 무결성 약화 가능

## ✅ int hp 필드가 필수로 초기화 되지 않아도 되는 이유
```java
// 예시
@RequiredArgsConstructor
public class Monster {

    private final String name;
    private final MonsterStage stage;

    @NonNull
    private MonsterAttribute attribute;

    private int hp;
}
```

이 질문을 이해하면 **Java 객체 생성 원리 + Lombok 설계 철학**이 한 번에 정리됩니다.

결론부터 말하면:
**private int hp;는 "자바 타입 특성상 자동으로 초기화되며, Lombok 입장에서는 '반드시 생성자에서 받아야 할 필수 값'이 아니기 때문입니다."**

단계별로 설명하겠습니다.

### 1️⃣ 자바 객체는 **"자동 초기화(default initialization)"가 있다**

**✔︎ Java의 인스턴스 필드 기본값**

- 타입: int, 기본값: 0
- 타입: long, 기본값: 0L
- 타입: boolean, 기본값: false
- 타입: Object, 기본값: null

### 2️⃣ Lombok의 관점: "필수 초기화란 무엇인가?"

Lombok은 이렇게 정의합니다: 
> **"생성자에서 반드시 값을 받아야만 정상적인 객체 상태가 되는 필드"**

**그래서 Lombok이 "필수"로 보는 기준은 딱 2가지**
1. final 필드
→ 생성자 아니면 초기화 불가 ❌
2. @NonNull 필드
→ null이면 의미 없는 상태 ❌

### 3️⃣ hp는 왜 필수가 아닌가?

**이유 1. 기본값이 존재한다.**
- int → 0
- JVM이 자동 보장

**이유 2. final이 아니다.**
- 생성자 이후에도 값 변경 가능
- "반드시 생성 시점에 정해져야 한다"는 계약이 없음

**이유 3. 도메인 의미상 '초기값'이 유효할 수도 있음**
- 예를 들어 종족(MonsterSpecies) 단계에서는 hp는 "기본 스탯", 나중에 인스턴스 생성 시 세팅.
