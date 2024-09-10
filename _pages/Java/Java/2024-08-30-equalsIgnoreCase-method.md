---
title: "☕️[Java] .equalsIgnoreCase()"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-08-30"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] .equalsIgnoreCase()

## 1️⃣ .equalsIgnoreCase()
- 자바에서 **`".equalsIgnoreCase()"`** 메서드는 문자열(String) 간의 대소문자를 구분하지 않고 두 문자열이 동일한지를 비교하는 메서드입니다.
    - 즉, 두 문자열이 대소문자만 다르고 내용이 동일하다면 **`"true"`** 를 반환하고, 그렇지 않으면 **`"false"`** 를 반환합니다.

## 2️⃣ 예시.
```java
public class Main {
    public static void main(String[] args) {
        String str1 = "Hello";
        String str2 = "hello";
        String str3 = "HELLO";
        String str4 = "World";
        
        // str1 과 str2는 대소문자만 다르고 동일한 내용이므로 true 반환
        System.out.println(str1.equalsIgnoreCase(str2)); // Output: true
        
        // str1 과 str3도 대소문자만 다르고 동일한 내용이므로 true 반환
        System.out.println(str1.equalsIgnoreCase(str3)); // Output: true
        
        // str1과 str4는 내용이 다르므로 false 반환
        System.out.prinln(str1.equalsIgnoreCase(str4)); // Output: false
    }
}
```

## 3️⃣ 주요 포인트.
- **대소문자 무시 :** **`".equalsIgnoreCase()"`** 메서드는 비교할 때 대소문자를 무시합니다.
    - 예를 들어, **`"Hello"`** 와 **`"hello"`** 를 비교하면 **`"true"`** 를 반환합니다.
- **공백 및 특수문자 :** 문자열 내의 공백이나 특수문자까지 비교 대상에 포함됩니다.
    - 즉, **`"Hello "`** 와 **`"hello"`** 는 공백 때문에 **`"false"`** 를 반환합니다.
- **반환 값 :** 이 메서드는 두 문자열이 대소문자만 다르고 동일한 내용을 가지고 있으면 **`"true"`**, 그렇지 않으면 **`"false"`** 를 반환합니다.

이 메서드는 문자열 비교에서 대소문자를 구분할 필요가 없을 때 유용하게 사용됩니다.
예를 들어, 사용자 입력을 비교할 때 유용할 수 있습니다.
