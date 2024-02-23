---
title: "☕️[Java] 클래스와 메서드"
tags:
    - Java
    - Programming Language
date: "2024-02-23"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 클래스와 메서드.

클래스는 데이터인 멤버 변수 뿐 아니라 기능 역할을 하는 메서드도 포함할 수 있습니다.

**먼저 멤버 변수만 존재하는 클래스로 간단한 코드를 작성해봅시다.**

**ValueData**
```java
package oop1;

public class ValueData {
  int value;
}
```

**ValueDataMain**
```java
package oop1;

public class ValueDataMain {

  public static void main(String[] args) {
    ValueData valueData = new ValueData();
    add(valueData);
    add(valueData);
    add(valueData);
    System.out.println("최종 숫자=" + valueData.value);
  }

  static void add(ValueData valueData) {
    valueData.value++;
    System.out.println("숫자 증가 value=" + valueData.value);
  }
}
```

**실행 결과**
```
숫자 증가 value=1
숫자 증가 value=2
숫자 증가 value=3
최종 숫자=3
```
* `ValueData` 라는 인스턴스를 생성하고 외부에서 `ValueData.value`에 접근해 숫자를 하나씩 증가시키는 단순한 코드입니다.
    * 코드를 보면 데이터인 `value`와 `value`의 값을 증가시키는 기능은 `add()` 메서드가 서로 분리되어 있습니다.
* 자바 같은 객체 지향 언어는 클래스 내부에 속성(데이터)과 기능(메서드)을 함께 포함할 수 있습니다.
    * 클래스 내부에 멤버 변수 뿐만 아니라 메서드도 함께 포함할 수 있다는 뜻 입니다.

**"이번에는 숫자를 증가시키는 기능도 클래스에 함께 포함해서 새로운 클래스를 정의해봅시다."**

```java
package oop1;

public class ValueData {
  int value;

  void add() {
    value++;
    System.out.println("숫자 증가 value=" + value);
  }
}
```
* 이 클래스에는 데이터인 `value`와 해당 데이터를 사용하는 기능인 `add()` 메서드를 함께 정의했습니다.

이제 이 클래스가 어떻게 사용되는지 확인해봅시다.

> **참고: 여기서 만드는 add() 메서드에는 static 키워드를 사용하지 않습니다.**
> 메서드는 객체를 생성해야 호출할 수 있습니다. 그런데 `static`이 붙으면 객체를 생성하지 않고도 메서드를 호출할 수 있습니다.
> `static`에 대한 자세한 내용은 추후에 포스팅하겠습니다.

**ValueObjectMain**

```java
package oop1;

public class ValueObjectMain {

  public static void main(String[] args) {
    ValueData valueData = new ValueData();
    valueData.add();
    valueData.add();
    valueData.add();
    System.out.println("최종 숫자=" + valueData.value);
  }
}
```

**실행 결과**
```
숫자 증가 value=1
숫자 증가 value=2
숫자 증가 value=3
최종 숫자=3
```

**인스턴스 생성**
```java
ValueData valueData = new ValueData();
```
* `valueData`라는 객체를 생성했습니다. 이 객체는 멤버 변수 뿐만 아니라 내분에 기능을 수행하는 `add()` 메서드도 함께 존재합니다.

**인스턴스의 메서드 호출**
* 인스턴스의 메서드를 호출하는 방법은 멤버 변수를 사용하는 방법과 동일합니다.
    * `.(dot)`을 찍어서 객체 접근한 다음에 원하는 메서드를 호출하면 됩니다.
```java
valueData.add(); // 1
x002.add(); // 2 x002 ValueData 인스턴스에 있는 add() 메서드를 호출합니다.
```
* **3:** `add()` 메서드를 호출하면 메서드 내부에서 `value++`을 호출하게 됩니다.
    * 이때 `value`에 접근해야 하는데, 기본으로 본인 인스턴스에 있는 멤버 변수에 접근합니다.
        * 본인 인스턴스가 `x002` 참조값을 사용하므로 자기 자신인 `x002.value`에 접근하게 됩니다.
* **4:** `++` 연산으로 `value`의 값을 하나 증가시킵니다.

**정리**
* 클래스는 속성(데이터, 멤버 변수)과 기능(메서드)을 정의할 수 있습니다.
* 객체는 자신의 메서드를 통해 자신의 멤버 변수에 접근할 수 있습니다.
    * 객체의 메서드 내부에서 접근하는 멤버 변수는 객체 자신의 멤버 변수입니다.
