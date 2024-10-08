---
title: "☕️[Java] 기본형 타입(Primitive Type)."
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-09-12"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] 기본형 타입(Primitive Type).

## 1️⃣ 기본형 타입(Primitive Type)란?
- 기본형 타입(Primitive Type)은 Java에서 가장 단순하고 기본적인 데이터 유형으로, 실제 값을 직접 저장하는 변수 유형입니다.
- 기본형 타입(Primitive Type)은 참조형 타입(Reference Type)과 달리 객체가 아니며, 메모리 상에 직접 데이터를 저장합니다.
- Java에는 8개의 기본형 타입(Primitive Type)이 있습니다.

### 1️⃣ 정수형.
- `byte` : 8비트 정수(-128 ~ 127).
- `short` : 16비트 정수(-32,768 ~ 32,767).
- `int` : 32비트 정수(-2^31 ~ 2^31-1).
- `long` : 64비트 정수(-2^63 ~ 2^63-1).

### 2️⃣ 실수형.
- `float` : 32비트 부동소수점.
- `double` : 64비트 부동소수점.

### 3️⃣ 문자형.
- `char` : 16비트 유니코드 문자(0 ~ 65,535)

### 4️⃣ 논리형.
- `boolean` : 참 또는 거짓(true/false)

## 2️⃣ 기본형 타입의 한계.
- **1. 객체로서의 기능 부족.**
    - 기본형 타입은 객체가 아니기 때문에 메서드나 속성을 가질 수 없습니다.
        - 이로 인해 객체지향적인 프로그래밍에서 사용되는 많은 기능을 사용할 수 없습니다.
- **2. 컬렉션과의 호환성 부족.**
    - 기본형 타입은 Java의 컬렉션 프레임워크(`List`, `Set`, `Map` 등)에 직접적으로 사용할 수 없습니다.
        - 컬렉션은 객체만을 저장할 수 있기 때문에, 기본형을 사용할 경우 래퍼 클래스(예: `Integer`, `Double`, `Boolean`)로 변환해야 합니다.
- **3. 유연성 부족.**
    - 기본형 타입은 값이 불변이며, 추가적인 속성이나 동작을 정의할 수 없습니다.
        - 이를 보완하기 위해 래퍼 클래스와 같은 객체가 필요합니다.

## 3️⃣ 기본형 타입을 사용하는 이유.
- **1. 성능.**
    - 기본형 타입은 참조형 타입보다 메모리 사용량이 적고, 처리 속도가 빠릅니다.
    - 기본형 타입은 실제 값을 직접 메모리에 저장하기 때문에, 메모리 접근이 빠르고, 가비지 컬렉션과 같은 추가적인 처리 없이 직접적으로 값을 사용할 수 있습니다.
- **2. 메모리 효율성.**
    - 기본형 타입은 참조형 타입보다 메모리 사용량이 적습니다.
        - 예를 들어, `int`는 32비트를 사용하지만, `Integer`는 객체를 래핑하기 때문에 추가적인 메모리 오버헤드가 발생합니다.
- **3. 간결함.**
    - 기본형 타입은 간단하고 사용하기 쉽습니다.
    - 복잡한 객체 대신, 단순한 값만 필요할 때 기본형 타입을 사용하는 것이 코드의 가독성을 높이고, 불필요한 복잡성을 줄일 수 있습니다.
- **4. 기본적인 연산 지원.**
    - 기본형 타입은 Java 언어 내에서 산술 연산, 비교 연산 등 기본적인 연산을 직접적으로 지원합니다.
        - 이러한 연산은 참조형 타입보다 훨씬 빠르게 수행됩니다.

## 4️⃣ 기본형 타입의 사용 예.

```java
public class Main {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;
        int sum = a + b; // 기본형 타입 간의 덧셈 연산
        System.out.println("Sum: " + sum); // 출력: Sum: 30
        
        // 기본형 타입을 사용할 경우 메모리 효율성과 성능이 우수
    }
}
```

## 5️⃣ 기본형 타입과 래퍼 클래스.
- Java는 기본형 타입을 객체로 감쌀 수 있는 래퍼 클래스를 제공합니다.
    - 예를 들어, `int`의 래퍼 클래스는 `Integer`입니다.
        - 래퍼 클래스는 기본형 타입을 객체로 변환하여 컬렉션과 같은 객체 기반 API와 호환성을 제공할 수 있습니다.
```java
public class Main {
    public static void main(String[] args) {
        Integer x = 10; // 오토박싱 (autoboxing)
        int y = x; // 오토언박싱 (autounboxing)
        
        // 래퍼 클래스는 메서드를 가질 수 있습니다.
        int max = Integer.max(10, 20);
        System.out.println("Max: " + max); // 출력: Max: 20
    }
}
```

- 기본형 타입은 성능과 메모리 효율성 면에서 장점이 있지만, 객체의 유연성과 컬렉션 호환성 측면에서 한계가 있습니다.
    - 이러한 이유로, Java에서는 상황에 따라 기본형 타입과 래퍼 클래스를 적절히 사용하게 됩니다.
