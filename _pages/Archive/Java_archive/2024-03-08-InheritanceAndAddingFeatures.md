---
title: "☕️[Java] 상속과 기능 추가"
tags:
    - Java
    - Programming Language
date: "2024-03-08"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 상속과 기능 추가.

이번에는 상속 관계의 장점을 알아보기 위해, 상속 관계에 다음 기능을 추가해보겠습니다.
* 모든 차량에 문열기(`openDoor()`) 기능을 추가합니다.
* 새로운 수소차(`HydrogenCar`)를 추가합니다.
    * 수소차는 `fillHydrogen()` 기능을 통해 수소를 충전할 수 있습니다.

**package ex3 - Car**
```java
package extends1.ex3;

public class Car {

  public void move() {
    System.out.println("차를 이동합니다.");
  }

  // 추가
  public void openDoor() {
    System.out.println("문을 엽니다.");
  }
}
```

* 모든 차량에 문열기 기능을 추가할 때는 상위 부모인 `Car`에 `openDoor()` 기능을 추가하면 됩니다.
    * 이렇게 하면 `Car`의 자식들은 해당 기능을 모두 물려받게 됩니다.
        * 만약 상속 관계가 아니었다면 각각의 차량에 해당 기능을 모두 추가해야 합니다.

**package ex3 - ElectricCar**
```java
package extends1.ex3;

public class ElectricCar extends Car {

  public void charge() {
    System.out.println("충전합니다.");
  }
}
```
* 기존 코드와 같습니다.

**package ex3 - GasCar**
```java
package extends1.ex3;

public class GasCar extends Car {

  public void fillUp() {
    System.out.println("기름을 주유합니다.");
  }
}
```
* 기존 코드와 같습니다.

**package ex3 - HydrogenCar**
```java
package extends1.ex3;

public class HydrogenCar extends Car {

  public void fillHydrogen() {
    System.out.println("수소를 충전합니다.");
  }
}
```
* 수소차를 추가했습니다.
* `Car`를 상속받은 덕분에 `move()`, `openDoor()`와 같은 기능을 바로 사용할 수 있습니다.
* 수소차는 전용 기능인 수소 충전(`fillHydrogen()`) 기능을 제공합니다.

**package ex3 - CarMain**
```java
package extends1.ex3;

public class CarMain {

  public static void main(String[] args) {
    ElectricCar electricCar = new ElectricCar();
    electricCar.move();
    electricCar.charge();
    electricCar.openDoor();

    GasCar gasCar = new GasCar();
    gasCar.move();
    gasCar.fillUp();
    gasCar.openDoor();

    HydrogenCar hydrogenCar = new HydrogenCar();
    hydrogenCar.move();
    hydrogenCar.fillHydrogen();
    hydrogenCar.openDoor();
  }
}
```

**실행 결과**
```
차를 이동합니다.
충전합니다.
문을 엽니다.
차를 이동합니다.
기름을 주유합니다.
문을 엽니다.
차를 이동합니다.
수소를 충전합니다.
문을 엽니다.
```

**기능 추가와 클래스 확장**

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%80%E1%85%B5%E1%84%82%E1%85%B3%E1%86%BC%E1%84%8E%E1%85%AE%E1%84%80%E1%85%A1%E1%84%8B%E1%85%AA%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%84%89%E1%85%B3%E1%84%92%E1%85%AA%E1%86%A8%E1%84%8C%E1%85%A1%E1%86%BC.png?raw=true">

* 상속 관계 덕분에 중복은 줄어들고, 새로운 수소차를 편리하게 확장(extend)한 것을 알 수 있습니다.
