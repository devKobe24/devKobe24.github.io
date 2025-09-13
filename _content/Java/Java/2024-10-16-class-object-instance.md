---
title: "☕️[Java] 클래스, 객체, 인스턴스"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-10-16"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] 클래스, 객체, 인스턴스.
- **클래스(Class), 객체(Object), 인스턴스(Instance)는 객체 지향 프로그래밍(Object-Oriented Programming, OOP)에서 중요한 개념이며, 서로 밀접하게 관련이 있지만 다른 의미를 가집니다.**

> 🙋‍♂️ [객체 지향 프로그래밍(Object-Oriented Programming, OOP)](https://www.devkobe24.com/CS/2024/2024-10-14-what-is-the-oop.html)

## 1️⃣ 클래스(Class)
- **클래스(Class)는 "객체(Object)를 정의하기 위한 청사진(설계도)"입니다.**
    - 클래스(Class)는 객체(Object)의 **속성(필드)과 행동(메서드)을** 정의합니다.
- 즉, 클래스는 어떤 유형의 객체(Object)를 만들 것인지에 대한 정보를 가지고 있으며, 객체(Object)를 생성하는 데 사용되는 틀 역할을 합니다.

### 예시.
```java
// 클래스 선언.
public class Car {
    // 필드(속성)
    String model;
    String color;
    
    // 메서드(행동)
    void drive() {
        System.out.println("The car is driving.");
    }
}
```

- 위의 `Car` 클래스는 자동차의 **모델과 색상**을 나타내는 속성(필드)과, 자동차가 **달리는 행동**을 정의하는 메서드를 가지고 있습니다.
    - 이 자체는 실제 객체(Object)가 아니라 객체(Object)를 만들기 위한 설계도(blueprint, 청사진)입니다.

## 2️⃣ 객체(Object)
- **객체(Object)는 클래스(Class)에 의해 생성된 실체로, 클래스의 인스턴스(Instance)라고도 불립니다.**
- 객체(Object)는 클래스(Class)에서 정의된 속성(필드)과 행동(메서드)를 실제로 가지고 있는 **메모리 상의 실체**입니다.
    - 즉, 클래스의 설계도(blueprint, 청사진)을 바탕으로 만들어진 실질적인 데이터입니다.
- 객체(Object)는 클래스(Class)에서 정의된 구조에 따라 동작하며, 프로그램 내에서 데이터와 기능을 담당합니다.

### 객체(Object) 생성 예시.

```java
public class Main {
    public static void main(String[] args) {
        // 객체 생성
        Car myCar = new Car(); // Car 클래스의 객체 생성.
        myCar.model = "Volvo xc60";
        myCar.color = "Black";
        myCar.drive(); // 출력: The car is driving
    }
}
```

- 위 코드에서 `Car myCar = new Car();`는 `Car` 클래스(Class)의 **객체(Object)를 생성합니다.**
    - 이 `myCar` 객체(Object)는 `Car` 클래스(Class)에서 정의한 속성(필드)과 행동(메서드)를 가지고 있으며, 프로그램에서 실제로 사용됩니다.

## 3️⃣ 인스턴스(Instance)
- **인스턴스(Instance)는 특정 클래스(Class)에서 생성된 객체(Object)를 의미합니다.**
    - 즉, **클래스(Class)의 인스턴스(Instance)는** 그 클래스(Class)에 의해 생성된 **실체 객체(Object)를 말합니다.**
- 흔히 **인스턴스(Instance)와 객체(Object)는** 같은 의미로 사용되지만, **"인스턴스(Instance)"는 특정 클래스(Class)에서 만들어진 객체(Object)라는** 의미에 좀 더 **초점**을 맞추고 있습니다.
- 예를 들어, `myCar`는 `Car` 클래스의 인스턴스이며, 또한 객체입니다.

### 인스턴스(Instance) 예시.
```java
Car myCar = new Car(); // Car 클래스의 인스턴스 생성.
Car yourCar = new Car(); // 또 다른 Car 클래스의 인스턴스 생성.
```

- 위 코드에서 `myCar`와 `yourCar`는 모두 `Car` 클래스의 **인스턴스(Instance)입니다.**
    - 즉, 동일한 클래스(Class)에서 만들어진 여러개의 객체(인스턴스)를 생성할 수 있습니다.

## 4️⃣ 요약.
- **클래스(Class) :** 객체(Object)를 생성하기 위한 설계도(blueprint, 청사진)입니다. 속성(필드)과 행동(메서드)를 정의합니다.
- **객체(Object) :** 클래스(Class)에 의해 생성된 **실체**입니다. 프로그램 내에서 사용되며, 클래스의 인스턴스(Instance)라고도 불립니다.
- **인스턴스(Instance) :** **특정 클래스에서 생성된 객체(Object)를** 가리키는 용어입니다. 클래스의 실체로서 메모리 내에 존재하는 객체(Object)를 의미합니다.
- **객체(Object)와 인스턴스(Instance)는 서로 거의 같은 의미로 사용**되며, **클래스(Class)는 이 객체(Object)들을 만들기 위한 설계도(blueprint, 청사진)입니다.**
