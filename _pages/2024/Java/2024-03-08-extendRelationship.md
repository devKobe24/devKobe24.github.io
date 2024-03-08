---
title: "☕️[Java] 상속관계"
tags:
    - Java
    - Programming Language
date: "2024-03-08"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 상속관계
* 상속은 객체 지향 프로그래밍의 핵심 요소 중 하나로, 기존 클래스의 필드와 메서드를 새로운 클래스에서 재사용하게 해줍니다.
    * 이름 그대로 기존 클래스의 속성과 기능을 그대로 물려받는 것입니다.
* 상속을 하려면 `extends` 키워드를 사용하면 됩니다.
    * 그리고 `extends` **대상은 하나만 선택**할 수 있습니다.

**용어 정리**
* **부모 클래스(슈퍼 클래스)**: 상속을 통해 자신의 필드와 메서드를 다른 클래스에 제공하는 클래스
* **자식 클래스(서브 클래스)**: 부모 클래스로부터 필드와 메서드를 상속받는 클래스

**⛔️주의⛔️**
**지금부터 코드를 작성할 때 기존 코드를 유지하기 위해, 새로운 패키지에 기존 코드를 옮겨가면서 코드를 작성할 것입니다.**
**이름이 같기 때문에 패키지 명과 import 사용에 주의해야 합니다.**

상속 관계를 사용하도록 코드를 작성해봅시다.

**기존 코드를 유지하기 위해 ex2 패키지를 새로 만들겠습니다.**
```java
package extends1.ex2;

public class Car {

  public void move() {
    System.out.println("차를 이동합니다.");
  }
}
```

```java
package extends1.ex2;

public class ElectricCar extends Car {

  public void charge() {
    System.out.println("충전합니다.");
  }
}
```

```java
package extends1.ex2;

public class GasCar extends Car {

  public void fillUp() {
    System.out.println("기름을 주유합니다.");
  }
}
```

```java
package extends1.ex2;

public class CarMain {

  public static void main(String[] args) {
    ElectricCar electricCar = new ElectricCar();
    electricCar.move();
    electricCar.charge();

    GasCar gasCar = new GasCar();
    gasCar.move();
    gasCar.fillUp();
  }
}
```

**실행 결과**
```
차를 이동합니다.
충전합니다.
차를 이동합니다.
기름을 주유합니다.
```

* 실행 결과는 기존 예제와 완전히 동일합니다.

**상속 구조도**
<img src="https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A9%E1%86%A8%E1%84%80%E1%85%AE%E1%84%8C%E1%85%A9%E1%84%83%E1%85%A9.png?raw=true">

* 전기차와 가솔린차가 `Car`를 상속 받은 덕분에 `electricCar.move()`,`gasCar.move()`를 사용할 수 있습니다.

> 참고로 당연한 이야기지만 상속은 부모의 기능을 자식이 물려 받는 것입니다.
> 따라서 자식이 부모의 기능을 물려 받아서 사용할 수 있습니다.
> 반대로 부모 클래스는 자식 클래스에 접근할 수 없습니다.
> 자식 클래스는 부모 클래스의 기능을 물려 받기 때문에 접근할 수 있지만, 그 반대는 아닙니다.

**부모 코드를 봐봅시다!**
* 자식에 대한 정보가 하나도 없습니다.
    * 반면에 자식 코드는 `extends Parents`를 통해서 부모를 알고 있습니다.

## 단일 상속

자바는 다중 상속을 지원하지 않습니다.
* 그래서 `extend` 대상은 하나만 선택할 수 있습니다.
    * 부모를 하나만 선택할 수 있다는 뜻입니다.
        * 물론 부모가 또 다른 부모를 하나 가지는 것은 괜찮습니다.

**다중 상속 그림**

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%8C%E1%85%AE%E1%86%BC%E1%84%89%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A9%E1%86%A8%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B7.png?raw=true">

만약 비행기와 자동차를 상속 받아서 하늘을 나는 자동차를 만든다고 가정해봅시다.
* 만약 그림과 같이 다중 상속을 사용하게 되면 `AirplaneCar` 입장에서 `move()`를 호출할 때 어떤 부모의 `move()`를 사용해야 할지 애매한 문제가 발생합니다.
    * 이것을 다이아몬드 문제라고 합니다.
    * 그리고 다중 상속을 사용하면 클래스 계층 구조가 매우 복잡해질 수 있습니다.
        * 이런 문제점 때문에 자바는 클래스의 다중 상속을 허용하지 않습니다.
            * 대신에 추후에 공부할 인터페이스의 다중 구현을 허용해서 이러한 문제를 피합니다.
