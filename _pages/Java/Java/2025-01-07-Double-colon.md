---
title: "☕️[Java] `::`(더블 콜론) 문법이란 무엇일까요?"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-12-07"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] `::`(더블 콜론) 문법이란 무엇일까요?
## 📌 Intro.
- ↘︎ **::(더블 콜론)는 메서드 참조(Method Reference)를 나타내는 Java 문법임.**
    - ↘︎ Java 8부터 도입, **람다 표현식을 더 간결하게 사용할 수 있도록 도와줌.**

## ✅1️⃣ 메서드 참조의 기본 개념.
- ↘︎ 람다 표현식으로 작성할 수 있는 코드를 더욱 간단하게 작성할 수 있음.

### 👉 기본 문법 형식.

|형태|예시|설명|
| -------- | -------- | -------- |
|Static 메서드 참조|ClassName::staticMethod|클래스의 static 메서드 참조|
|인스턴스 메서드 참조|instance::method|특정 객체의 인스턴스 메서드 참조|
|임의 객체의 메서드 참조|ClassName::method|클래스의 임의 객체 메서드 참조|
|생성자 참조|ClassName::new|클래스 생성자 참조|

## ✅2️⃣ 예제.
### 1️⃣ Static 메서드 참조.
```java
Function<String, Integer> stringToInt = Integer::parseInt;
// 람다 표현삭으로 하면: (String s) -> Integer.parseInt(s)
```
- ↘︎ Integer::parseInt는 String을 받아 Integer로 변환하는 static 메서드임.

### 2️⃣ 인스턴스 메서드 참조.
```java
String str = "hello";
Supplier<Integer> lengthSupplier = str::length;
// 람다 표현식으로 하면: () -> str.length()
```
- ↘︎ str::length는 str 객체의 length() 메서드를 참조함.

### 3️⃣ 임의 객체의 메서드 참조.
```java
List<String> list = Arrays.asList("a", "b", "c");
list.stream().map(String::toUpperCase).forEach(System.out::println);
// 람다 표현식으로 하면: s -> s.toUpperCase()
```
- ↘︎ String::toUpperCase는 스트림의 각 요소에 대해 toUpperCase() 메서드를 호출함.

### 4️⃣ 생성자 참조.
```java
Supplier<List<String>> listSupplier = ArrayList::new;
// 람다 표현식으로 하면: () -> new ArrayList<>()
```
- ↘︎ ArrayList::new는 새로운 ArrayList 객체를 생성함.

## ✅3️⃣ 코드 분석.
```java
.filter(not(Comment::getDeleted))
.filter(Comment::isRoot)
```

- ↘︎ **1. `Comment::getDeleted`**
    - Comment 클래스의 getDeleted() 메서드를 참조함.
    - Predicate.not를 통해 getDeleted()가 false인 댓글만 필터링함.
- ↘︎ **2. `Comment::isRoot`**
    - Comment 클래스의 isRoot() 메서드를 참조함.
    - 댓글이 루트 댓글인지 확인함.

## 🚀 정리.
- ↘︎ **::(더블 콜론)는 '메서드 참조'를 나타낸다.**
- ↘︎ `ClassName::method`는 클래스 인스턴스 메서드나 `static` 메서드를 참조할 때 사용된다.
- ↘︎ 코드 가독성을 높이고 람다 표현식을 간결하게 작성할 수 있다.

### 🔑 키 포인트.
- ↘︎ **::(더블 콜론)는 '람다 표현식'의 축약형이다.**
- ↘︎ `(x) ➞ x.someMethod()` ➞ `ClassName::someMethod`로 변환 가능하다.
