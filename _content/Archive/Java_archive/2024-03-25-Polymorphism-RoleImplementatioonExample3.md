---
title: "☕️[Java] 다형성 - 역할 구현 예제 3"
tags:
    - Java
    - Programming Language
date: "2024-03-25"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 다형성 - 역할과 구현 예제 3

다형성을 활용하면 역할과 구현을 분리해서, 클라이언트 코드의 변경 없이 구현 객체를 변경할 수 있습니다.

다음 관계에서 `Driver`가 클라이언트입니다.

예제를 통해서 자세히 알아봅시다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC-%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%92%E1%85%A1%E1%86%AF%E1%84%80%E1%85%AA%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%E1%84%8B%E1%85%A8%E1%84%8C%E1%85%A63.png?raw=true">

앞서 설명한 자동차 예제를 코드로 구현해보겠습니다.
- `Driver`: 운전자는 자동차(`Car`)의 역할에만 의존합니다. 
    - 구현인 K3, Model3 자동차에 의존하지 않습니다.
        - `Driver` 클래스는 `Car car` 멤버 변수를 가집니다. 따라서 `Car` 인터페이스를 참조합니다.
        - 인터페이스를 구현한 `K3Car`, `Model3Car`에 의존하지 않고, `Car`인터페이스에만 의존합니다.
            - 여기서 설명하는 의존은 클래스 의존 관계를 뜩합니다. 클래스 상에서 어떤 클래스를 알고 있는가를 뜻합니다.
                - `Driver` 클래스 코드를 보면 `Car` 인터페이스만 사용하는 것을 확인할 수 있습니다.
- `Car`: 자동차의 역할이고 인터페이스입니다. `K3Car`, `Model3Car` 클래스가 인터페이스를 구현합니다.

```java
package poly.car1;

public interface Car {
  void startEngine();
  void offEngine();
  void pressAccelerator();
}
```

```java
package poly.car1;

public class K3Car implements Car {
  @Override
  public void startEngine() {
    System.out.println("K3Car.startEngine");
  }

  @Override
  public void offEngine() {
    System.out.println("K3Car.offEngine");
  }

  @Override
  public void pressAccelerator() {
    System.out.println("K3Car.pressAccelerator");
  }
}
```

```java
package poly.car1;

public class Model3Car implements Car{
  @Override
  public void startEngine() {
    System.out.println("Model3Car.startEngine");
  }

  @Override
  public void offEngine() {
    System.out.println("Model3Car.offEngine");
  }

  @Override
  public void pressAccelerator() {
    System.out.println("Model3Car.pressAccelerator");
  }
}
```

```java
package poly.car1;

public class Driver {
  private Car car;

  public void setCar(Car car) {
    System.out.println("자동차를 설정합니다: " + car);
    this.car = car;
  }

  public void drive() {
    System.out.println("자동차를 운전합니다.");
    car.startEngine();
    car.pressAccelerator();
    car.offEngine();
  }
}
```
- `Driver`는 멤버 변수로 `Car car`를 가집니다.
- `setCar(Car car)`: 멤버 변수에 자동차를 설정합니다.
    - 외부에서 누군가 이 메서드를 호출해주어야 `Driver`는 새로운 자동차를 참조하거나 변경할 수 있습니다.
- `drive()`: `Car` 인터페이스가 제공하는 기능들을 통해 자동차를 운전합니다.

```java
package poly.car1;

public class CarMain1 {

  public static void main(String[] args) {
    Driver driver = new Driver();

    // 차량 선택(k3)
    K3Car k3Car = new K3Car();
    driver.setCar(k3Car);
    driver.drive();

    // 차량 변경(k3 -> model3)
    Model3Car model3Car = new Model3Car();
    driver.setCar(model3Car);
    driver.drive();
  }
}
```

**실행 결과**
```
자동차를 설정합니다: poly.car1.K3Car@4a574795
자동차를 운전합니다.
K3Car.startEngine
K3Car.pressAccelerator
K3Car.offEngine

자동차를 설정합니다: poly.car1.Model3Car@23fc625e
자동차를 운전합니다.
Model3Car.startEngine
Model3Car.pressAccelerator
Model3Car.offEngine
```

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC-%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%92%E1%85%A1%E1%86%AF%E1%84%80%E1%85%AA%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%E1%84%8B%E1%85%A8%E1%84%8C%E1%85%A63-K3Car.png?raw=true">

- 먼저 `Driver`와 `K3Car`를 생성합니다.
- `driver.setCar(k3Car)`를 호출해서 `Driver`의 `Car car` 필드가 `K3Car`의 인스턴스를 참조하도록 합니다.
- `driver.drive()`를 호출하면 `x001`을 참조합니다.
    - `car` 필드가 `Car` 타입이므로 `Car` 타입을 찾아서 실행하지만 메서드 오버라이딩에 의해 `K3Car`의 기능이 호출됩니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC-%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%92%E1%85%A1%E1%86%AF%E1%84%80%E1%85%AA%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%E1%84%8B%E1%85%A8%E1%84%8C%E1%85%A63-Model3Car.png?raw=true">

- `Model3Car`를 생성합니다.
- `driver.setCar(model3Car)`를 호출해서 `Driver`의 `Car car` 필드가 `Model3Car`의 인스턴스를 참조하도록 변경합니다.
- `driver.drive()`를 호출하면 `x002`을 참조합니다.
    - `car` 필드가 `Car` 타입이므로 `Car` 타입을 찾아서 실행하지만 메서드 오버라이딩에 의해 `Model3Car`의 기능이 호출됩니다.
