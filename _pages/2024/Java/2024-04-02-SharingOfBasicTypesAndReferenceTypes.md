---
title: "☕️[Java] 기본형과 참조형의 공유"
tags:
    - Java
    - Programming Language
date: "2024-04-02"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 기본형과 참조형의 공유.

자바의 데이터 타입을 가장 크게 보면 기본형(Primitive Type)과 참조형(Reference Type)으로 나눌 수 있습니다.
- **기본형(Primitive Type):** 하나의 값을 여러 변수에서 절대로 공유하지 않습니다.
- **참조형(Reference Type):** 하나의 객체를 참조값을 통해 여러 변수에서 공유할 수 있습니다.

하나의 값을 공유하거나 또는 공유하지 않는다는 것이 무슨 뜻인지 예제를 통해 알아봅시다.

## 기본형 예제
기본형은 하나의 값을 여러 변수에서 절대로 공유하지 않습니다.

다음 예를 봅시다.

```java
package lang.immutable.address;

public class PrimitiveMain {

  public static void main(String[] args) {
    // 기본형은 절대로 같은 값을 공유하지 않는다.
    int a = 10;
    int b = a; // a -> b, 값 복사 후 대입
    
    System.out.println("a = " + a);
    System.out.println("b = " + b);
    

    b = 20;
    System.out.println("20 -> b");
    System.out.println("a = " + a);
    System.out.println("b = " + b);
  }
}
```

**실행 결과**
```java
a = 10
b = 10
20 -> b
a = 10
b = 20
```

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%E1%84%92%E1%85%A7%E1%86%BC%E1%84%80%E1%85%AA%E1%84%8E%E1%85%A1%E1%86%B7%E1%84%8C%E1%85%A9%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%B4%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8B%E1%85%B2-1.png?raw=true">

- 기본형 변수 `a`와 `b`는 절대로 하나의 값을 공유하지 않습니다.
- `b = a`라고 하면 **자바는 항상 값을 복사해서 대입** 합니다. 이 경우 `a`에 있는 값 `10`을 복사해서 `b`에 전달합니다.
- 결과적으로 `a`와 `b`는 둘 다 `10`이라는 똑같은 숫자의 값을 가집니다.
    - 하지만 `a`가 가지는 `10`과 `b`가 가지는 `10`은 복사된 완전히 다른 `10`입니다.
        - 메모리 상에서도 `a`에 속하는 `10`과 `b`에 속하는 `10`이 각각 별도로 존재합니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%E1%84%92%E1%85%A7%E1%86%BC%E1%84%80%E1%85%AA%E1%84%8E%E1%85%A1%E1%86%B7%E1%84%8C%E1%85%A9%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%B4%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8B%E1%85%B2-2.png?raw=true">

- `b = 20`이라고 하면 `b`의 값만 `20`으로 변경됩니다.
- `a`의 값은 `10`으로 그대로 유지됩니다.
- 기본형 변수는 하나의 값을 절대로 공유하지 않습니다.
    - 따라서 값을 변경해도 변수 하나의 값만 변경됩니다.
        - 여기서는 변수 `b`의 값만 `20`으로 변경되었습니다.

너무 당연한 이야기 입니다.

그렇다면 이번에는 참조형 예제를 봐봅시다.

## 참조형 예제

```java
package lang.immutable.address;

public class RefMain1_1 {

  public static void main(String[] args) {
    // 참조형 변수는 하나의 인스턴스를 공유할 수 있습니다.
    Address a = new Address("서울");
    Address b = a;

    System.out.println("a = " + a);
    System.out.println("b = " + b);

    b.setValue("부산"); // b의 값을 부산으로 변경해야함.
    System.out.println("부산 -> b");
    System.out.println("a = " + a); // 사이드 이펙트 발생
    System.out.println("b = " + b);
  }
}
```
- 처음에는 `a`, `b` 둘다 서울이라는 주소를 가져야 한다고 가정합니다.
    - 따라서 `Address b = a` 코드를 작성했고, 변수 `a`, `b` 둘다 서울이라는 주소를 가집니다.
        - 이후에 `b`의 주소를 부산으로 변경합니다.
            - 그런데 실행 결과를 보면 `b`뿐만 아니라 `a`의 주소도 함께 부산으로 변경되어 버립니다.

**실행 결과**
```
a = Address{value='서울'}
b = Address{value='서울'}
부산 -> b
a = Address{value='부산'}
b = Address{value='부산'}
```

순서대로 코드를 분석해봅시다.
```java
Address a = new Address("서울");
Address b = a;
```

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%E1%84%92%E1%85%A7%E1%86%BC%E1%84%80%E1%85%AA%E1%84%8E%E1%85%A1%E1%86%B7%E1%84%8C%E1%85%A9%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%B4%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8B%E1%85%B2-3.png?raw=true">

- 참조형 변수들은 같은 참조값을 통해 같은 인스턴스를 참조할 수 있습니다.
- `b = a`라고 하면 `a`에 있는 참조값 `x001`을 복사해서 `b`에 전달합니다.
    - 자바에서 모든 값 대입은 변수가 가지고 있는 값을 복사해서 전달합니다.
        - 변수가 `int` 같은 숫자값을 가지고 있으면 숫자값을 복사해서 전달하고, 참조값을 가지고 있으면 참조값을 복사해서 전달합니다.
- 참조값을 복사해서 전달하므로 결과적으로 `a`, `b`는 같은 `x001` 인스턴스를 참조합니다.
- 기본형 변수는 절대로 같은 값을 공유하지 않습니다.
    - 예) `a = 10`, `b = 10`과 같이 같은 모양의 숫자 `10`이라는 값을 가질 수는 있지만 같은 값을 공유하는 것은 아닙니다
        - 서로 다른 숫자 `10`이 두 개 있는 것입니다.
- 참조형 변수는 참조값을 통해 같은 객체(인스턴스)를 공유할 수 있습니다.

여기서 `b`의 주소만 부산으로 변경했는데, `a`의 주소도 함께 부산으로 변경되어 버린 이유는 무엇일까요?
- 메모리 구조를 보면 바로 답이 나오겠지만, 개발을 하다 보면 누구나 이런 실수할 수 있을 것 같다는 생각도 함께 들 것입니다.
