---
title: "☕️[Java] 다형성과 메서드 오버라이딩"
tags:
    - Java
    - Programming Language
date: "2024-03-17"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 다형성과 메서드 오버라이딩.

**"다형성을 이루는 또 하나의 중요한 핵심 이론은 바로 오버라이딩입니다."**
메서드 오버라이딩에서 꼭! 기억해야 할 점은 **"오버라이딩 된 메서드가 항상 우선권을 가진다"** 는 점입니다.
그래서 이름도 기존 기능을 덮어 새로운 기능을 재정의 한다는 뜻의 오버라이딩 입니다.

앞서 메서드 오버라이딩을 학습했지만 지금까지 학습한 메서드 오버라이딩은 반쪽짜리입니다.
**"메서드 오버라이딩의 진짜 힘은 다형성과 함께 사용할 때 나타납니다."**

다음 코드를 통해 다형성과 메서드 오버라이딩을 알아봅시다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%E1%84%80%E1%85%AA%E1%84%86%E1%85%A6%E1%84%89%E1%85%A5%E1%84%83%E1%85%B3%E1%84%8B%E1%85%A9%E1%84%87%E1%85%A5%E1%84%85%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%83%E1%85%B5%E1%86%BC.png?raw=true">

* `Parent`, `Child` 모두 `Value` 라는 같은 멤버 변수를 가지고 있습니다.
    * 멤버 변수는 오버라이딩 되지 않습니다.
* `Parent`, `Child` 모두 `method()`라는 같은 메서드를 가지고 있습니다. `Child`에서 메서드를 오버라이팅 했습니다.
    * 메서드는 오버라이딩 됩니다.

```java
package poly.overriding;

public class OverridingMain {

  public static void main(String[] args) {
    // 자식 변수가 자식 인스턴스 참조
    Child child = new Child();
    System.out.println("Child -> Child");
    System.out.println("value = " + child.value);
    child.method();

    // 부모 변수가 부모 인스턴스 참조
    Parent parent = new Parent();
    System.out.println("Parent -> Parent");
    System.out.println("value = " + parent.value);
    parent.method();

    // 부모 변수가 자식 인스턴스 참조(다형적 참조)
    Parent poly = new Child();
    System.out.println("Parent -> Child");
    System.out.println("value = " + poly.value); // 변수는 오버라이딩 x
    poly.method(); // 메서드 오버라이딩!
  }
}
```

**실행 결과**
```
Child -> Child
value = child
Child.method

Parent -> Parent
value = parent
Parent.method

Parent -> Child
value = parent
Child.method
```

그림을 통해 코드를 분석해봅시다.

<img src="https://github.com/devKobe24/images/blob/main/child-%3Echild.png?raw=true">

* `child` 변수는 `Child` 타입 입니다.
    * 따라서 `child.value`, `child.method()`를 호출하면 인스턴스의 `Child` 타입에서 기능을 찾아서 실행합니다.

<img src="https://github.com/devKobe24/images/blob/main/parent-%3Eparent.png?raw=true">

* `parent` 변수는 `Parent` 타입 입니다.
    * 따라서 `parent.value`, `parent.method()`를 호출하면 인스턴스의 `Parent` 타입에서 기능을 찾아서 실행합니다.

<img src="https://github.com/devKobe24/images/blob/main/parent-%3Echild.png?raw=true">

* **이 부분이 중요합니다.**
* `poly` 변수는 `Parent` 타입 입니다.
    * 따라서 `poly.value`,`poly.method()`를 호출하면 인스턴스의 `Parent` 타입에서 기능을 찾아서 실행합니다.
        * `poly.value`: `Parent` 타입에 있는 `value` 값을 읽습니다.
        * `poly.method()` : `Parent` 타입에 있는 `method()`를 실행하려고 합니다.
            * 그런데 `Child.method()`가 오버라이딩 되어있습니다.
            * **"오버라이딩 된 메서드는 항상 우선권을 가집니다."**
                * 따라서 `Parent.method()`가 아니라 `Child.method()`가 실행됩니다.

**오버라이딩 된 메서드는 항상 우선권을 가집니다.**
* 오버라이딩은 부모 타입에서 정의한 기능을 자식 타입에서 재정의하는 것입니다.
    * 만약 자식에서도 오버라이딩하고 손자에서도 메서드를 오버라이딩을 하면 손자의 오버라이딩 메서드가 우선권을 가집니다.
        * "더 하위 자식의 오버라이딩 된 메서드가 우선권을 가지는 것입니다."

지금까지 다형성을 이루는 핵심 이론인 다형적 참조와 메서드 오버라이딩에 대해 학습했습니다.
* **다형적 참조** : 하나의 변수 타입으로 다양한 자식 인스턴스를 참조할 수 있는 기능
* **메서드 오버라이딩** : 기존 기능을 하위 타입에서 새로운 기능으로 재정의
