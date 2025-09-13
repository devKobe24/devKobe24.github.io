---
title: "☕️[Java] 인터페이스 - 다중구현"
tags:
    - Java
    - Programming Language
date: "2024-03-22"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 인터페이스 - 다중구현
## 자바가 다중 상속을 지원하지 않는 이유 - 복습
자바는 다중 상속을 지원하지 않습니다.

그래서 `extend` 대상은 하나만 선택할 수 있습니다.
* 부모를 하나만 선택할 수 있다는 뜻입니다.
    * 물론 부모가 또 부모를 가지는 것은 괜찮습니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%8C%E1%85%AE%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A9%E1%86%A8%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B7.png?raw=true">

만약 비행기와 자동차를 상속 받아서 하늘을 나는 자동차를 만든다고 가정해봅시다.

만약 그림과 같이 다중 상속을 사용하게 되면 `AirplaneCar` 입장에서 `move()`를 호출할 때 어떤 부모의 `move()`를 사용해야 할지 애매한 문제가 발생합니다.
* 이것을 다이아몬드 문제라 합니다.

그리고 다중 상속을 사용하면 클래스 계층 구조가 매우 복잡해질 수 있습니다.

이런 문제점 때문에 자바는 클래스의 다중 상속을 허용하지 않습니다.
* 대신에 인터페이스의 다중 구현을 허용하여 이러한 문제를 피합니다.

클래스는 앞서 설명한 이유로 다중 상속이 안되는데, 인터페이스의 다중 구현은 허용한 이유는 무엇일까요?
* 인터페이스는 모두 추상 메서드로 이루어져 있기 때문입니다.

다음 예제를 봅시다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%90%E1%85%A5%E1%84%91%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%E1%84%83%E1%85%A1%E1%84%8C%E1%85%AE%E1%86%BC%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B7.png?raw=true">

`InterfaceA`, `InterfaceB`는 둘 다 같은 `methodCommon()`을 가지고 있습니다.

그리고 `Child`는 두 인터페이스를 구현했습니다.
* 상속 관계의 경우 두 부모 중에 어떤 한 부모의 `methodCommon()`을 사용해야 할지 결정해야 하는 다이아몬드 문제가 발생합니다.
    * 하지만 인터페이스 자신은 구현을 가지지 않습니다.
        * 대신에 인터페이스를 구현하는 곳에서 해당 기능을 모두 구현해야 합니다.

여기서 `InterfaceA`, `InterfaceB`는 같은 이름의 `methodCommon()`를 제공하지만 이것의 기능은 `Child`가 구현합니다.
* 그리고 오버라이딩에 의해 어차피 `Child`에 있는 `methodCommon()`이 호출됩니다.
    * 결과적으로 두 부모 중에 어떤 한 부모의 `methodCommon()`을 선택하는 것이 아니라 그냥 인터페이스들을 구현한 `Child`에 있는 `methodCommon()`이 사용됩니다.
        * 이런 이유로 인터페이스는 다이아몬드 문제가 발생하지 않습니다.
            * 따라서 인터페이스의 경우 다중 구현을 허용합니다.

예제를 코드로 작성해봅시다.

```java
package poly.diamond;

public class Child implements InterfaceA, InterfaceB {

  @Override
  public void methodA() {
    System.out.println("Child.methodA");
  }

  @Override
  public void methodB() {
    System.out.println("Child.methodB");
  }

  @Override
  public void methodCommon() {
    System.out.println("Child.methodCommon");
  }
}
```

* `ìmplements InterfaceA, InterfaceB`와 같이 다중 구현을 할 수 있습니다.
    * `implements` 키워드 위에 `,`로 여러 인터페이스를 구분하면 됩니다.
* `methodCommon()`의 경우 양쪽 인터페이스에 다 있지만 같은 메서드이므로 구현은 하나만 하면 됩니다.

```java
package poly.diamond;

public class DiamondMain {

  public static void main(String[] args) {
    InterfaceA a = new Child();
    a.methodA();
    a.methodCommon();

    InterfaceB b = new Child();
    b.methodB();
    b.methodCommon();
  }
}
```

**실행 결과**
```
Child.methodA
Child.methodCommon
Child.methodB
Child.methodCommon
```

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%90%E1%85%A5%E1%84%91%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%E1%84%83%E1%85%A1%E1%84%8C%E1%85%AE%E1%86%BC%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%ABchild.png?raw=true">

1. `a.methodCommon()`을 호출하면 먼저 `x001` `Child` 인스턴스를 찾는다.
2. 변수 `a`가 `InterfaceA` 타입이므로 해당 타입에서 `methodCommon()`을 찾습니다.
3. `methodCommon()`은 하위 타입인 `Child`에서 오버라이딩 되어 있습니다. 따라서 `Child`의 `methodCommon()`이 호출됩니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%90%E1%85%A5%E1%84%91%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%E1%84%83%E1%85%A1%E1%84%8C%E1%85%AE%E1%86%BC%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%ABchildB.png?raw=true">

4. `b.methodCommon()`을 호출하면 먼저 `x001` `Child` 인스턴스를 찾습니다.
5. 변수 `b`가 `InterfaceB` 타입으로 해당 타입에서 `methodCommon()`을 찾습니다.
6. `methodCommon()`은 하위 타입인 `Child`에서 오버라이팅 되어 있습니다. 따라서 `Child`의 `methodCommon()`이 호출됩니다.
