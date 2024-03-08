---
title: "☕️[Java] 상속과 메모리 구조"
tags:
    - Java
    - Programming Language
date: "2024-03-08"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 상속과 메모리 구조.

**이 부분을 제대로 이해하는 것이 앞으로 정말 중요합니다!**

상속 관계를 객체로 생성할 때 메모리 구조를 확인해봅시다.

```java
ElectricCar electricCar = new ElectricCar();
```

<img src="https://github.com/devKobe24/images/blob/main/electricCar-1.png?raw=true">

* `new ElectricCar()`를 호출하면 `ElectricCar` 뿐만 아니라 상속 관계에 있는 `Car`까지 함께 포함해서 인스턴스를 생성합니다.
* 참조값은 `x001`로 하나이지만 실제로 그 안에서는 `Car`, `ElectricCar`라는 두가지 클래스 정보가 공존하는 것입니다.
* 상속이라고 해서 단순하게 부모의 필드와 메서드만 물려 받는게 아닙니다.
    * 상속 관계를 사용하면 부모 클래스도 함께 포함해서 생성됩니다.
* 외부에서 볼때는 하나의 인스턴스를 생성하는 것 같지만 내부에서는 부모와 자식이 모두 생성되고 공간도 구분됩니다.

**`electricCar.charge()` 호출**

<img src="https://github.com/devKobe24/images/blob/main/electricCar-2.png?raw=true">

* `electricCar.charge()`를 호출하면 참조값을 확인해서 `x001.charge()`을 호출합니다.
    * 따라서 `x001`을 찾아서 `charge()`를 호출하면 되는 것입니다.
* 그런데 상속 관계의 경우에는 내부에 부모와 자식이 모두 존재합니다.
    * 이때 부모인 `Car`를 통해서 `charge()`를 찾을지 아니면 `ElectricCar`를 통해서 `charge()`를 찾을지 선택해야 합니다.
        * 이때는 **"호출하는 변수의 타입(클래스)을 기준으로 선택합니다."**
            * `electricCar` 변수의 타입이 `ElectricCar` 이므로 인스턴스 내부에 같은 타입인 `ElectricCar`를 통해서 `charge()`를 호출합니다.

**`electricCar.move()` 호출**

<img src="https://github.com/devKobe24/images/blob/main/electricCar-3.png?raw=true">

* `electricCar.move()`를 호출하면 먼저 `x001` 참조로 이동합니다.
    * 내부에는 `Car`, `ElectricCar` 두가지 타입이 있습니다.
        * 이때 호출하는 변수인 `electricCar`의 타입이 `ElectricCar` 이므로 이 타입을 선택합니다.
            * 그런데 `ElectricCar`에는 `move()` 메서드가 없습니다.
                * 상속 관계에서는 자식 타입에 해당 기능이 없으면 부모 타입으로 올라가서 찾습니다.
* 이 경우 `ElectricCar`의 부모인 `Car`로 올라가서 `move()`를 찾습니다.
    * 부모인 `Car`에 `move()`가 있으므로 부모에 있는 `move()` 메서드를 호출합니다.

* 만약 부모에서도 해당 기능을 찾지 못하면 더 상위 부모에서 필요한 기능을 찾아봅니다.
    * 부모에 보무로 계속 올라가면서 필드나 메서드를 찾는 것 입니다.
        * 물론 계속 찾아도 없으면 컴파일 오류가 발생합니다.

**"지금까지 설명한 상속과 메모리 구조는 반드시 이해해야 합니다!"**

* 상속 관계의 객체를 생성하면 그 내부에는 부모와 자식이 모두 생성됩니다.
* 상속 관계의 객체를 호출할 때, 대상 타입을 정해야 합니다. 이때 호출자의 타입을 통해 대상 타입을 찾습니다.
* 현재 타입에서 기능을 찾지 못하면 상위 부모 타입으로 기능을 찾아서 실행합니다. 기능을 찾지 못하면 컴파일 오류가 발생합니다.
