---
title: "☕️[Java] String 클래스의 앞뒤 공백 제거 방법."
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-09-08"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] String 클래스의 앞 뒤 공백 제거 방법.

## 1️⃣ `trim()`
- Java에서 `String` 클래스의 앞뒤 공백을 제거하려면 `trim()` 메서드를 사용할 수 있습니다.
    - 이 메서드는 문자열의 시작과 끝에 있는 공백을 제거한 새로운 문자열을 반환합니다.

### 예시
```java
String originalString = "   Hello, World!   ";
String trimmedString = originalString.trim();

System.out.println("Original:'" + originalString + "'");
System.out.println("Trimmed: '" + trimmedString + "'");
```

**출력 결과**
```bash
Original: '   Hello, World!   '
Trimmed: 'Hello, World!'
```

- 이렇게 `trim()` 메서드를 사용하면 문자열의 앞뒤 공백이 모두 제거된 결과를 얻을 수 있습니다.

## 2️⃣ `strip()`
- `trim()` 과 유사하게 동작하지만, `trim()`은 UTF-16 코드 포인트 중 공백으로 간주되는 문자를 기준으로 하며, `strip()`은 모든 공백 문자를 제거합니다.
- `strip()`은 모든 공백 문자를 제거합니다.
- `strip()`은 Java 11부터 사용할 수 있습니다.

### 예시
```java
String strippedString = originalString.strip();
System.out.println("Stripped: '" + strippedString + "'");
```

- `strip()` 도 앞뒤의 공백을 제거하지만, 보다 광범위한 공백 문자를 처리합니다.
