---
title: "💭 [Metacognition] 240616 JAVA의 정석"
tags:
    - Metacognition
    - Java
date: "2024-06-16"
thumbnail: "/assets/img/thumbnail/META.jpg"
---

# 1️⃣ Primitive type(기본형)의 종류와 크기.

Java의 Primitive Type(기본형)은 총 8가지가 있습니다.

## 1️⃣ 각 Type의 크기와 특징.

### 1️⃣ byte

- 크기 : 1 byte(8 bits)
- 범위 : -128 ~ 127

> 📚 NOTE: bit
> 
> binary digit의 약자.
> 이진수(0 또는 1) 하나의 자리수를 의미합니다.
> 
> 즉, 1 bit은 0 또는 1을 표현할 수 있습니다.

### 2️⃣ short

- 크기 : 2 bytes (16 bits)
- 범위 : -32,768 ~ 32,767

### 3️⃣ int

- 크기 : 4 bytes (32 bits)
- 범위 : - 2,147,483,648 ~ 2,147,483,647

### 4️⃣ long

- 크기 : 8 bytes (64 bits)
- 범위 : - 9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807

### 5️⃣ float

- 크기 : 4 bytes (32 bits)
- 범위 : 1.4E-45 ~ 3.4E+38 (단정밀도 부동소수점)

### 6️⃣ double

- 크기 : 8 bytes(64 bits)
- 범위 : 4.9E-324 ~ 1.8E+308 (배정밀도 부동소수점)

### 7️⃣ char

- 크기 : 2 bytes(4 bits)
- 범위 : `'\u0000' ~ '\uffff'` (유니코드 문자)

### 8️⃣ boolean

- 크기 : Java 언어 명세서에 따르면 boolean의 크기는 명시되어 있지 않으며, 일반적으로 1 bit으로 표현됩니다.
- 값: true 또는 false

**이러한 primitive type은 Java에서 기본 데이터 타입으로 사용되며, 각 타입은 고정된 크기를 가지고 있습니다.**
