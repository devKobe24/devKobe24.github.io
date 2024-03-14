---
title: "☕️[Java] 다운캐스팅과 주의점"
tags:
    - Java
    - Programming Language
date: "2024-03-15"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 다운캐스팅과 주의점.

다운캐스팅은 잘못하면 심각한 런타임 오류가 발생할 수 있습니다.
다음 코드를 통해 다운캐스팅에서 발생할 수 있는 문제를 확인해봅시다.

```java
package poly.basic;

// 다운캐스팅을 자동으로 하지 않는 이유
public class CastingMain4 {

  public static void main(String[] args) {
    Parent parent1 = new Child();
    Child child1 = (Child) parent1;
    child1.childMethod(); // 문제 없음

    Parent parent2 = new Parent();
    Child child2 = (Child) parent2; // 런타임 오류 - ClassCastException
    child2.childMethod(); // 실행 불가
  }
}
```

**실행 결과**
```
Child.childMethod
Exception in thread "main" java.lang.ClassCastException: class poly.basic.Parent cannot be cast to class poly.basic.Child (poly.basic.Parent and poly.basic.Child are in unnamed module of loader 'app')
	at poly.basic.CastingMain4.main(CastingMain4.java:12)
```

* 실행 결과를 보면 `child1.childMethod()`는 잘 호출되었지만, `child2.childMethod()`는 실행되지 못하고, 그 전에 오류가 발생합니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%8F%E1%85%A2%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC%E1%84%8B%E1%85%B5%E1%84%80%E1%85%A1%E1%84%82%E1%85%B3%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%AE.png?raw=true">

* 예제의 `parent1`의 경우 다운캐스팅을 해도 문제가 되지 않습니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%8F%E1%85%A2%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC%E1%84%8B%E1%85%B5%E1%84%87%E1%85%AE%E1%86%AF%E1%84%80%E1%85%A1%E1%84%82%E1%85%B3%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%AE.png?raw=true">

* 예제의 `parent2`를 다운캐스팅하면 `ClassCastException` 이라는 심각한 런타임 오류가 발생합니다.

이 코드를 자세히 알아봅시다.
```java
Parent parent2 = new Parent()
```

* 먼저 `new Parent()`로 부모 타입으로 객체를 생성합니다.
    * 따라서 메모리 상에 자식 타입은 전혀 존재하지 않습니다.
        * 생성 결과를 `parent2`에 담아둡니다.
            * 이 경우 같은 타입이므로 여기서는 문제가 발생하지 않습니다.

```java
Child child2 = (Child) parent2
```
* 다음으로 `parent2`를 `Child` 타입으로 다운캐스팅합니다.
    * 그런데 `parent2`는 `Parent`로 생성되었습니다.
        * 따라서 메모리상에 `Child` 자체가 존재하지 않습니다. `Child` 자체를 사용할 수 없는 것입니다.

자바에서는 이렇게 사용할 수 없는 타입으로 다운캐스팅하는 경우에 `ClassCastExecption`이라는 예외를 발생시킵니다.
* 예외가 발생하면 다음 동작이 실행되지 않고, 프로그램이 종료됩니다.
    * 따라서 `child2.childMethod()` 코드 자체가 실행되지 않습니다.

## 업캐스팅이 안전하고 다운캐스팅이 위험한 이유
업캐스팅의 경우 이런 문제가 절대로 발생하지 않습니다.
* 왜냐하면 객체를 생성하면 해당 타입의 상위 부모 타입은 모두 함께 생성되기 때문입니다!
    * 따라서 위로만 타입을 변경하는 업캐스팅은 메모리 상에 인스턴스가 모두 존재하기 때문에 항상 안전합니다.
        * 따라서 캐스팅을 생략할 수 있습니다.

반면에 다운캐스팅의 경우 인스턴스에 존재하지 않는 하위 타입으로 캐스팅하는 문제가 발생할 수 있습니다.
* 왜냐하면 객체를 생성하면 부모 타입은 모두 함께 생성되지만 자식 타입은 생성되지 않습니다.
    * 따라서 개발자가 이런 문제를 인지하고 사용해야 한다는 의미로 명시적으로 캐스팅을 해주어야 합니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B7%E1%84%8B%E1%85%B3%E1%84%85%E1%85%A9%E1%84%89%E1%85%A5%E1%86%AF%E1%84%86%E1%85%A7%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%E1%84%8B%E1%85%A5%E1%86%B8%E1%84%8F%E1%85%A2%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC.png?raw=true">

클래스 A, B, C는 상속 관계입니다.
* `new C()`로 인스턴스를 생성하면 인스턴스 내부에 자신과 부모인 A, B, C가 모두 생성됩니다.
    * 따라서 C의 부모 타입인 A, B, C 모두 C 인스턴스를 참조할 수 있습니다.

**"상위로 올라가는 업캐스팅은 인스턴스 내부에 부모가 모두 생성되기 때문에 문제가 발생하지 않습니다."**
* `A a = new C()`: A로 업캐스팅
* `B b = new C()`: B로 업캐스팅
* `C c = new C()`: 자신과 같은 타입

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B7%E1%84%8B%E1%85%B3%E1%84%85%E1%85%A9%E1%84%89%E1%85%A5%E1%86%AF%E1%84%86%E1%85%A7%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%E1%84%83%E1%85%A1%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%8F%E1%85%A2%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC.png?raw=true">

* `new B()`로 인스턴스를 생성하면 인스턴스 내부에 자신과 부모인 A, B가 생성됩니다.
    * 따라서 B의 부모 타입인 A, B 모두 B 인스턴스를 참조 할 수 있습니다.
        * 상위로 올라가는 업캐스팅은 인스턴스 내부에 부모가 모두 생성되기 때문에 문제가 발생하지 않습니다.
            * 하지만 객체를 생성할 때 하위 자식은 생성되지 않기 때문에 하위로 내려가는 다운캐스팅은 인스턴스 내부에 없는 부분을 선택하는 문제가 발생할 수 있습니다.

* `A a = new B()` : A로 업캐스팅
* `B b = new B()` : 자신과 같은 타입
* `C c = new B()` : 하위 타입은 대입할 수 없음, **컴파일 오류**
* `C c = (C) new B()` : 하위 타입으로 강제 다운캐스팅, 하지만 B 인스턴스에 C와 관련된 부분이 없으므로 잘못된 캐스팅, `ClassCastException` **런타임 오류 발생**

### 컴파일 오류 vs 런타임 오류
* 컴파일 오류는 변수명 오류, 잘못된 클래스 이름 사용등 자바 프로그램을 실행하기 전에 발생하는 오류입니다.
    * 이런 오류는 IDE에서 즉시 확인할 수 있기 때문에 안전하고 좋은 오류 입니다.
* 반면에 런타임 오류는 이름 그대로 프로그램이 실행되고 있는 시점에 발생하는 오류입니다.
    * 런타임 오류는 매우 안좋은 오류입니다.
        * 왜냐하면 보통 고객이 해당 프로그램을 실행하는 도중에 발생하기 때문입니다.
