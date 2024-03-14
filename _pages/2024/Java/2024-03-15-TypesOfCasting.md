---
title: "☕️[Java] 캐스팅의 종류"
tags:
    - Java
    - Programming Language
date: "2024-03-15"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 캐스팅의 종류

자식 타입의 기능을 사용하려면 다음과 같이 다운 캐스팅 결과를 변수에 담아두고 이후에 기능을 사용하면 됩니다.
```java
Child child = (Child) poly
child.childMethod();
```

하지만 다운캐스팅 결과를 변수에 담아두는 과정은 번거롭습니다.
이런 과정 없이 일시적으로 다운캐스팅을 해서 인스턴스에 있는 하위 클래스의 기능을 바로 호출할 수 있습니다.

다음 코드를 봐봅시다.

## 일시적 다운 캐스팅
```java
package poly.basic;

public class CastingMain2 {

  public static void main(String[] args) {
    // 부모 변수가 자식 인스턴스 참조(다형적 참조)
    Parent poly = new Child();

    // 단 자식의 기능은 호출할 수 없습니다.(컴파일 오류 발생)
    // poly.childMethod();

    // 일시적 다운캐스팅 - 해당 메서드를 호출하는 순간만 다운캐스팅
    ((Child) poly).childMethod();
  }
}
```

**실행 결과**
```
Child.childMethod
```

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B5%E1%84%8C%E1%85%A5%E1%86%A8%E1%84%83%E1%85%A1%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%8F%E1%85%A2%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC.png?raw=true">

```java
((Child) poly).childMethod()
```

* `poly`는 `Parent` 타입입니다. 그런데 이 코드를 실행하면 `Parent` 타입을 임시로 `Child`로 변경합니다.
    * 그리고 메서드를 호출할 때 `Child` 타입에서 찾아서 실행합니다.

정확히는 `poly`가 `Child` 타입으로 바뀌는 것은 아닙니다.
```java
((Child) poly).childMethod(); // 다운캐스팅을 통해 부모타입을 자식 타입으로 변환 후 기능 호출
((Child) x001).childMethod(); // 참조값을 읽은 다음 자식 타입으로 다운캐스팅
```

* 참고로 캐스팅을 한다고 해서 `Parent poly`의 타입이 변하는 것은 아닙니다.
    * 해당 참조값을 꺼내고 꺼낸 참조값이 `Child` 타입이 되는 것입니다.
        * 따라서 `poly`의 타입은 `Parent`로 그대로 유지됩니다.

이렇게 일시적 다운캐스팅을 사용하면 별도의 변수 없이 인스턴스의 자식 타입의 기능을 사용할 수 있습니다.

## 업캐스팅

다운캐스팅과 반대로 현재 타입을 부모 타입으로 변경하는 것을 업캐스팅이라고 합니다.

다음 코드를 봅시다.

```java
package poly.basic;

public class CastingMain3 {

  public static void main(String[] args) {
    Child child = new Child();
    Parent parent1 = (Parent) child; // 업캐스팅은 생략 가능, 생략 권장.
    Parent parent2 = child;

    parent1.parentMethod();
    parent2.parentMethod();
  }
}
```

**실행 결과**
```
Parent.parentMethod
Parent.parentMethod
```

다음 코드를 봐봅시다
```java
Parent parent1 = (Parent) child;
```
* `Child` 타입을 `Parent` 타입에 대입해야 합니다.
    * 따라서 타입을 변환하는 캐스팅이 필요합니다.

그런데 부모 타입으로 변환하는 경우에는 다음과 같이 캐스팅 코드인 `(타입)`를 생략할 수 있습니다.
```java
Parent parent2 = child
Parent parent2 = new Child()
```

**"업캐스팅은 생략할 수 있습니다. 참고로 업캐스팅은 매우 자주 사용하기 때문에 생략을 권장합니다."**
* 자바에서 부모는 자식을 담을 수 있습니다. 하지만 그 반대는 안됩니다.(꼭 필요하다면 다운캐스팅을 해야 합니다.)

업캐스팅을 생략해도 되고, 다운캐스팅은 왜 개발자가 직접 명시적으로 캐스팅을 해야할까요?

