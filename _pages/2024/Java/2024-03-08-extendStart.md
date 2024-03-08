---
title: "☕️[Java] 상속 - 시작"
tags:
    - Java
    - Programming Language
date: "2024-03-08"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 상속 - 시작.

상속 관계가 왜 필요한지 이해하기 위해 다음 예제 코드를 만들어서 실행해봅시다.

## 예제 코드

### 패키지 위치에 주의합시다.

```java
package extends1.ex1;

public class ElectricCar {

  public void move() {
    System.out.println("차를 이동합니다.");
  }

  public void charge() {
    System.out.println("충전합니다.");
  }
}
```

```java
package extends1.ex1;

public class GasCar {

  public void move() {
    System.out.println("차를 이동합니다.");
  }

  public void fillUp() {
    System.out.println("기름을 주유합니다.");
  }
}
```

```java
package extends1.ex1;

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

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%8C%E1%85%A5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A1%E1%84%80%E1%85%A1%E1%84%89%E1%85%A9%E1%86%AF%E1%84%85%E1%85%B5%E1%86%AB%E1%84%8E%E1%85%A1.png?raw=true">

* 전기차(`ElectricCar`)와 가솔린차(`GasCar`)를 만들었습니다.
    * 전기차는 이동(`move()`), 충전(`charge()`) 기능이 있습니다.
    * 가솔린차는 이동(`move()`), 주유(`fillUp()`) 기능이 있습니다.

* 전기차와 가솔린차는 자동차(`Car`)의 좀 더 구체적인 개념입니다.
* 반대로 자동차(`Car`)는 전기차와 가솔린차를 포함하는 추상적인 개념입니다.
    * 그래서인지 잘 보면 둘의 공통 기능이 보입니다.
        * 바로 이동(`move()`)입니다.
            * 전기차든 가솔린차든 주유하는 방식이 다른 것이지 이동하는 것은 똑같습니다.
                * 이런 경우 상속 관계를 사용하는 것이 효과적입니다.
