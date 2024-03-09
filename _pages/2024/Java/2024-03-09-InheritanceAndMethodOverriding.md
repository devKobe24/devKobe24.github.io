---
title: "☕️[Java] 상속과 메서드 오버라이딩"
tags:
    - Java
    - Programming Language
date: "2024-03-09"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 상속과 메서드 오버라이딩.

* 부모 타입의 기능을 자식에서는 다르게 재정의 하고 싶을 수 있습니다.
    * 예를 들어서 자동차의 경우 `Car.move()`라는 기능이 있습니다.
        * 이 기능을 사용하면 단순히 "차를 이동합니다." 라고 출력합니다.
            * 전기차의 경우 보통 더 빠르기 때문에 전기차가 `move()`를 호출한 경우에는 "전기차를 빠르게 이동합니다."라고 출력을 변경하고 싶습니다.

이렇게 부모에게서 상속 받은 기능을 자식이 **재정의 하는 것을 "메서드 오버라이딩(Overriding)"** 이라고 합니다.

**package extends1.overriding - Car**

```java
package extends1.overriding;

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
* 기존 코드와 같습니다.

**package extends1.overriding - GasCar**

```java
package extends1.overriding;

public class GasCar extends Car {

  public void fillUp() {
    System.out.println("기름을 주유합니다.");
  }
}
```

**package extends1.overriding - HydrogenCar**

```java
package extends1.overriding;

public class HydrogenCar extends Car {

  public void fillHydrogen() {
    System.out.println("수소를 충전합니다.");
  }
}
```

**package extends1.overriding - ElectricCar**

```java
package extends1.overriding;

public class ElectricCar extends Car {

  @Override
  public void move() {
    System.out.println("전기차를 빠르게 이동합니다.");
  }

  public void charge() {
    System.out.println("충전합니다.");
  }
}
```

* `ElectricCar`는 부모인 `Car`의 `move()` 기능을 그대로 사용하고 싶지 않습니다.
    * 메서드 이름은 같지만 새로운 기능을 사용하고 싶습니다.
        * 그래서 `ElectricCar`의 `move()` 메서드를 새로 만들었습니다.

**이렇게 부모의 기능을 자식이 새로 재정의하는 것을 "매서드 오버라이딩"** 이라고 합니다.
* 이제 `ElectricCar`의 `move()`를 호출하면 `Car`의 `move()`가 아니라 `ElectricCar`의 `move()`가 호출됩니다.

### @Override
* `@`이 붙은 부분을 애노테이션(어노테이션, annotattion)이라 합니다.
    * 애노테이션은 주석과 비슷한데, 프로그램이 읽을 수 있는 특별한 주석이라 생각하면 됩니다.
* 이 애노테이션은 상위 클래스의 매서드를 오버라이드하는 것임을 나타냅니다.
    * 이름 그대로 오버라이딩한 매서드 위에 이 애노테이션을 붙여야 합니다.
    * 컴파일러는 이 애노테이션을 보고 매서드가 정확히 오버라이드 되었는지 확인합니다.
    * 오바라이딩 조건을 만족시키지 않으면 컴파일 에러를 발생시킵니다.
        * 따라서 실수로 오버라이딩을 못하는 경우를 방지해줍니다.
            * 예를 들어서 이 경우에 만약 부모에 `move()` 메서드가 없다면 컴파일 오류가 발생합니다.

> 참고로 이 기능은 필수는 아니지만 코드의 명확성을 위해 붙여주는 것이 좋습니다.

**package extends1.overrding - CarMain**
```java
package extends1.overriding;

public class CarMain {

  public static void main(String[] args) {
    ElectricCar electricCar = new ElectricCar();
    electricCar.move();

    GasCar gasCar = new GasCar();
    gasCar.move();
  }
}
```

**실행 결과**
```
전기차를 빠르게 이동합니다.
차를 이동합니다.
```

* 실행 결과를 보면 `electricCar.move()`를 호출했을 때 오버라이딩한 `ElectricCar.move()` 메서드가 실행된 것을 확인할 수 있습니다.

### 오버라이딩과 클래스

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%8B%E1%85%A9%E1%84%87%E1%85%A5%E1%84%85%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%83%E1%85%B5%E1%86%BC%E1%84%80%E1%85%AA%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%84%89%E1%85%B3.png?raw=true">

* `Car`의 `move()` 메서드를 `ElectricCar`에서 오버라이딩 했습니다.

### 오버라이딩과 메모리 구조

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%8B%E1%85%A9%E1%84%87%E1%85%A5%E1%84%85%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%83%E1%85%B5%E1%86%BC%E1%84%80%E1%85%AA%20%E1%84%86%E1%85%A6%E1%84%86%E1%85%A9%E1%84%85%E1%85%B5%E1%84%80%E1%85%AE%E1%84%8C%E1%85%A9.png?raw=true">

1. `electricCar.move()`를 호출합니다.
2. 호출한 `electricCar`의 타입은 `ElectricCar`입니다. 따라서 인스턴스 내부의 `ElectricCar` 타입에서 시작합니다.
3. `ElectricCar` 타입에 `move()` 메서드가 있습니다. 해당 메서드를 실행합니다. 이때 실행할 메서드를 이미 찾았으므로 부모 타입을 찾지 않스빈다.

### 오버로딩(Overloading)과 오버라이딩(Overriding)
* **메서드 오버로딩**: 메서드 이름이 같고 매개변수(파라미터)가 다른 메서드를 여러개 정의하는 것을 메서드 오버로딩(Overloading)이라고 합니다.
    * 오버로딩은 번역하면 과적인데, 과하게 물건을 담았다는 뜻입니다.
        * 따라서 같은 이름의 메서드를 여러개 정의했다고 이해하면 됩니다.
* **메서드 오버라이딩**: 메서드 오버라이딩은 하위 클래스에서 상위 클래스의 메서드를 재정의하는 과정을 의미합니다.
    * 따라서 상속 관계에서 사용합니다. 부모의 기능을 자식이 다시 정의하는 것입니다.
        * 오버라이딩을 단순히 해석하면 무언가를 넘어서 타는 것을 말합니다. 
            * 자식의 새로운 기능이 부모의 기존 기능을 넘어 타서 기존 기능을 새로운 기능으로 덮어버린다고 이해하면 됩니다.
                * 오버라이딩을 번역하면 무언가를 다시 정의한다고 해서 **재정의**라 합니다.
                    * 상속 관계에서는 기존 기능을 다시 정의한다고 이해하면 됩니다. 
                        * 실무에서는 메서드 오버라이딩, 메서드 재정의 둘 다 사용합니다.

### 메서드 오버라이딩 조건

메서드 오버라이딩은 다음과 같은 까다로운 조건을 가지고 있습니다.
* 다음 내용은 아직 학습하지 않은 내용들도 있으므로 이해하려고 하기 보다는 참고만 합시다.
    * 지금은 단순히 **부모 메서드와 같은 메서드를 오버라이딩 할 수 있다 정도로 이해하면 충분합니다.**

#### 메서드 오버라이딩 조건
* **메서드 이름**: 메서드 이름이 같아야 합니다.
* **메서드 매개변수(파라미터)**: 매개변수(파라미터) 타입, 순서, 개수가 같아야 합니다.
* **반환 타입**: **반환 타입이 같아야 합니다.** 단, 반환 타입이 하위 클래스 타입일 수 있습니다.
* **접근 제어자**: 오버라이딩 메서드의 접근 제어자는 상위 클래스의 메서드보다 더 제한적이어서는 안됩니다.
    * 예를 들어, 상위 클래스의 메서드가 `protected`로 선언되어 있으면 하위 클래스에서 이를 `public` 또는 `protected`로 오버라이드 할 수 있지만, `private` 또는 `default`로 오버라이드 할 수 없습니다.
* **예외**: 오버라이딩 메서드는 상위 클래스의 메서드보다 더 많은 체크 예외를 `throws`로 선언할 수 없습니다.
    * 하지만 더 적거나 같은 수의 예외, 또는 하위 타입의 예외는 선언할 수 있습니다.
* `static`, `final`, `private`: 키워드가 붙은 메서드는 오버라이딩 될 수 없습니다.
    * `static`은 클래스 레벨에서 작동하므로 인스턴스 레벨에서 사용하는 오버라이딩이 의미가 없습니다.
        * 쉽게 이야기해서 그냥 클래스 이름을 통해 필요한 곳에 직접 접근하면 됩니다.
    * `final` 메서드는 재정의를 금지합니다.
    * `private` 메서드는 해당 클래스에서만 접근 가능하기 때문에 하위 클래스에서 보이지 않습니다.
        * 따라서 오버라이딩 할 수 없습니다.
* **생성자 오버라이딩**: 생성자는 오버라이딩 할 수 없습니다.
