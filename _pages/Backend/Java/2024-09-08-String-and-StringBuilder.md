---
title: "☕️[Java] Strig 클래스를 StringBuilder 클래스로 변환하는 방법."
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-09-08"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] Strig 클래스를 StringBuilder 클래스로 변환하는 방법.
- Java에서 `String` 객체를 `StringBuilder` 객체로 변환하는 방법은 매우 간단합니다.
- `StringBuilder` 클래스는 `String` 과 달리, 문자열을 수정할 수 있으며, 주로 문자열을 빈번하게 조작해야 할 때 사용됩니다.

## 1️⃣ 변환 방법.
```java
String str = "Hello World!";
StringBuilder sb = new StringBuilder(str);
```

- 여기서 `StringBuilder` 생성자는 `String` 객체를 매개변수로 받아 해당 문자열을 포함하는 `StringBuilder` 객체를 생성합니다.

## 예시
```java
String str = "Hello, World!";
StringBuilder sb = new StringBuilder(str);

// StringBuilder로 추가적인 작업 수행
sb.append(" This is Java.");
System.out.println(sb.toString());
```

**출력 결과**
```bash
Hello, World! This is Java.
```

- 이 예제에서는 `String` 객체 `str을` `StringBuilder` 객체로 변환한 후, `append()` 메서드를 사용하여 문자열을 추가했습니다.
    - 최종적으로 `StringBuilder` 객체를 문자열로 변환하여 출력할 때는 `toString()` 메서드를 사용합니다.

이렇게 `StringBuilder`를 사용하면 문자열을 수정하는 작업이 보다 효율적으로 수행됩니다.
