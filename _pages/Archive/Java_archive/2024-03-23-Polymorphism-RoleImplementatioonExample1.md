---
title: "☕️[Java] 다형성 - 역할 구현 예제 1"
tags:
    - Java
    - Programming Language
date: "2024-03-23"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 다형성 - 역할 구현 예제 1
앞서 설명한 내용을 더 깊이있게 이해하기 위해, 간단한 운전자와 자동차의 관계를 개발해봅시다.

먼저 다형성을 사용하지 않고, 역할과 구현을 분리하지 않고 단순하게 개발해봅시다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A5%E1%86%AB%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%AA%E1%84%8C%E1%85%A1%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A11.png?raw=true">

`Driver`는 `K3Car`를 운전하는 프로그램입니다.

```java
package poly.car0;

public class K3Car {
  public void startEngine() {
    System.out.println("K3Car.startEngine");
  }

  public void offEngine() {
    System.out.println("K3Car.offEngine");
  }

  public void pressAccelerator() {
    System.out.println("K3Car.pressAccelerator");
  }
}
```

```java
package poly.car0;

public class Driver {

  private K3Car k3Car;

  public void setK3Car(K3Car k3Car) {
    this.k3Car = k3Car;
  }

  public void drive() {
    System.out.println("자동차를 운전합니다.");
    k3Car.startEngine();
    k3Car.pressAccelerator();
    k3Car.offEngine();
  }
}
```

```java
package poly.car0;

public class CarMain0 {

  public static void main(String[] args) {
    Driver driver = new Driver();
    K3Car k3Car = new K3Car();

    driver.setK3Car(k3Car);
    driver.drive();
  }
}
```

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%92%E1%85%A1%E1%86%AF%E1%84%80%E1%85%AA%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%E1%84%8B%E1%85%A8%E1%84%8C%E1%85%A6-%E1%84%86%E1%85%A6%E1%84%86%E1%85%A9%E1%84%85%E1%85%B5%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B7.png?raw=true">
