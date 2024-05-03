---
title: "☕️[Java] 타입 비교."
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-05-03"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 타입 비교.
자바 프로그래밍에서 두 변수의 타입이 같은지 비교하는 방법은 몇 가지 상황에 따라 다릅니다.
여기서는 변수가 **기본형(Primitive types)** 인 경우와 **참조형(Reference types)** 인 경우로 나누어 설명하겠습니다.

## 1️⃣ 기본형 변수의 타입 비교.
기본형 변수의 경우, **자바는 정적 타입 언어**이기 때문에 변수의 타입이 코드 작성 시 결정됩니다.
두 변수가 같은 기본형 타입인지 비교하는 일반적인 상황은 적으며, 주로 타입을 확인하거나 변환할 때 컴파일러가 자동으로 처리합니다.
그러나 명시적으로 확인하고 싶다면, 두 변수의 타입을 직접 코드에서 확인할 수 있습니다.
이는 일반적인 코딩 상황보다는 주로 제네릭 코드나 리플렉션을 사용할 때 발생합니다.

## 2️⃣ 참조형 변수의 타입 비교.
참조형 변수의 경우 타입 비교는 다음 두 가지 방법으로 이루어질 수 있습니다.

### 2.1 **`instanceof`** 연산자 사용.
**`instanceof`** 연산자는 특정 객체가 지정된 클래스의 인스턴스이거나 그 서브클래스의 인스턴스인지를 검사합니다.
- 이 방법은 주로 객체의 타입을 확인할 때 사용됩니다.

```java
Object a = "Hello";
Object b = new String("Hello");

if (a instanceof String && b instanceof String) {
    System.out.println("두 객체는 같은 타입입니다.");
}
```

### 2.2 **`getClass()`** 메서드 사용.
객체의 정확한 클래스를 알고 싶을 때 **`getClass()`** 메서드를 사용할 수 있습니다.
이 메서드는 객체의 런타임 클래스를 리턴합니다.
- 두 객체의 클래스가 정확히 같은지 비교할 때 유용합니다.

```java
Object a = new Integer(5);
Object b = new Double(5.0);

if (a.getClass().equals(b.getClass())) {
    System.out.println("두 객체는 같은 클래스입니다.")
} else {
    System.out.println("두 객체는 다른 클래스입니다.")
}
```

## 📝 마무리.
이 방법들은 객체의 타입을 정확히 확인할 수 있으며, 특히 다형석을 활용하는 객체지향 프로그래밍에서 유용하게 사용됩니다.
또한, 테스트 코드나 동적 타입 처리가 필요한 상황에서 자주 사용됩니다.
