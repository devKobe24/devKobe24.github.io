---
title: "🔍[Troubleshooting] 🚀 @NoArgsConstructor 사용: DTO vs Entity"
tags:
    - Troubleshooting
    - Backend Development
    - Spring Boot
date: "2025-10-08"
thumbnail: "/assets/img/thumbnail/troubleshooting.jpg"
---

# 🚀 @NoArgsConstructor 사용: DTO vs Entity

## 🔑 핵심 차이: 생성자의 접근 제어자

결론부터 말씀드리면, 두 어노테이션의 유일한 차이는 생성되는 **기본 생성자(no-argument constructor)의 접근 제어자(Access Modifier)** 입니다.

### `@NoArgsConstructor`
**`public`** 기본 생성자를 만듭니다.

```java
public class MyDto {
    // @NoArgsConstructor가 생성한 코드
    public MyDto() {
    }
}
```

### `@NoArgsConstructor(access = AccessLevel.PROTECTED)`
**`protected`** 기본 생성자를 만듭니다.

```java
public class MyEntity {
    // @NoArgsConstructor(access = AccessLevel.PROTECTED)가 생성한 코드
    protected MyEntity() {
    }
}
```

---

## 🤔 왜 이 차이가 중요한가요?

이 접근 제어자의 차이는 **"누가 이 클래스를 직접 생성할 수 있는가?"** 를 결정하며, 이는 **JPA Entity**와 **DTO**에서 각각 다른 의미를 가집니다.

---

## 📥 Request DTO에서: `@NoArgsConstructor` (public)

### 왜 public 기본 생성자가 필요한가요?

Request DTO는 주로 외부(클라이언트)에서 온 JSON 데이터를 자바 객체로 변환(Deserialization)할 때 사용됩니다.

Spring Boot가 클라이언트로부터 받은 **JSON 데이터를 DTO 객체로 변환하는 과정** 때문입니다.

### JSON → DTO 변환 과정

1. **객체 생성 단계**
   - 클라이언트가 API를 호출하면, Spring은 내부적으로 **Jackson** 라이브러리를 사용해 HTTP Body의 JSON 문자열을 읽습니다
   - Jackson은 이 JSON 데이터를 담을 자바 객체(Request DTO)를 먼저 생성해야 합니다

2. **`public` 기본 생성자 호출**
   - Jackson은 가장 간단하고 표준적인 방법인 **`public` 기본 생성자(`new YourRequestDto()`)** 를 호출하여 일단 텅 빈 DTO 객체를 만듭니다

3. **필드 값 주입**
   - 그 후에 JSON의 각 필드(`"chapterName": "선사시대"`)를 분석하여, 생성된 DTO 객체의 해당 필드에 값을 세터(setter)나 리플렉션(reflection)을 통해 주입합니다

> ⚠️ 만약 `public` 기본 생성자가 없다면 Jackson은 1단계에서 객체를 생성하는 것부터 실패하게 되어 오류가 발생합니다.

### 코드 예시

```java
package com.kobe.koreahistory.dto.request;

import lombok.Getter;
import lombok.NoArgsConstructor;

/**
 * 챕터 생성 요청 DTO
 */
@Getter
@NoArgsConstructor  // public 기본 생성자를 만들어줌
public class ChapterCreateRequestDto {
    private int chapterNumber;
    private String chapterTitle;
    // ... 필드들
}
```

### 핵심 포인트

**Request DTO에는 `@NoArgsConstructor`를 사용하여 `public` 기본 생성자를 열어두는 것이 일반적이고 올바른 방법입니다.**

---

## 🗄️ JPA Entity에서: `@NoArgsConstructor(access = AccessLevel.PROTECTED)`

### 왜 protected를 사용하나요?

**JPA Entity**는 데이터베이스 테이블과 직접 매핑되는 핵심 도메인 객체입니다. 이 객체는 함부로 생성되어서는 안 되며, 항상 일관된 상태를 유지해야 합니다.

### JPA가 기본 생성자를 필요로 하는 이유

- **JPA 명세**: JPA는 DB에서 데이터를 조회하여 Entity 객체를 만들 때, 내부적으로 리플렉션(reflection)을 통해 기본 생성자를 사용합니다
- 따라서 기본 생성자는 반드시 필요합니다

### 왜 public이 아닌 protected인가?

만약 기본 생성자를 `public`으로 열어두면, 개발자가 서비스 로직 등에서 아무 생각 없이 `new Chapter()`와 같이 비어있는(상태가 불완전한) Entity 객체를 생성할 수 있습니다. 

이는 데이터 무결성을 해치고 버그를 유발하는 주요 원인이 됩니다.

### protected의 안전장치 역할

생성자를 **`protected`** 로 만들면, **외부 패키지에서 `new Chapter()`를 호출하는 것을 막을 수 있습니다.** 

이는 개발자에게 "이 객체는 빌더(`@Builder`)나 정적 팩토리 메서드처럼 정해진 방식으로만 생성해야 해!"라는 명확한 메시지를 전달하는 **안전장치** 역할을 합니다.

> 💡 JPA는 리플렉션을 사용하므로 `protected`여도 문제없이 객체를 생성할 수 있습니다.

### 코드 예시

```java
package com.kobe.koreahistory.entity;

import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.AccessLevel;

import javax.persistence.*;

/**
 * 챕터 엔티티
 */
@Entity
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)  // protected 기본 생성자
public class Chapter {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private int chapterNumber;
    private String chapterTitle;
    
    @Builder
    public Chapter(int chapterNumber, String chapterTitle) {
        this.chapterNumber = chapterNumber;
        this.chapterTitle = chapterTitle;
    }
}
```

---

## 📊 최종 정리

|  | **`@NoArgsConstructor`<br>(public)** | **`@NoArgsConstructor`<br>`(access = AccessLevel.PROTECTED)`** |
|:---|:---|:---|
| **생성자** | `public` | `protected` |
| **주요 용도** | **Request DTO**<br>(JSON 역직렬화용) | **JPA Entity**<br>(객체 생성의 안정성 확보용) |
| **의도** | "이 클래스는 외부에서<br>자유롭게 생성될 수 있습니다" | "이 클래스는 정해진 방식 외에는<br>함부로 생성하지 마세요" |
| **비유** | 문을 활짝 열어두기 🚪 | 문을 살짝만 열어두기 🔒 |

---

## 💡 핵심 원칙

### Request DTO
외부의 JSON 데이터를 받아야 하므로, 누구나 객체를 생성할 수 있도록 문을 활짝 열어두어야 합니다.

👉 **`@NoArgsConstructor` (public)**

### JPA Entity
핵심 도메인 객체이므로, 정해진 규칙(빌더 패턴 등)으로만 생성하도록 강제하고 싶습니다.

👉 **`@NoArgsConstructor(access = AccessLevel.PROTECTED)`**

> 이 원칙을 지키면 더 안정적이고 의도가 명확한 코드를 작성할 수 있습니다.
