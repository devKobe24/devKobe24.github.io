---
title: "💾 [CS] 객체 지향 프로그래밍에서의 객체(Object)란 무엇일까요?"
tags:
    - CS
date: "2024-10-13"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 객체 지향 프로그래밍에서의 객체(Object)란 무엇일까요?

- **객체 지향 프로그래밍(Object-Oriented Programming, OOP)에서의 객체(Object)는 클래스(Class)에** 의해 정의된 **데이터**와 **그 데이터를 처리하는 동작(메소드, Method)을** 포함하는 **독립적인 개체**입니다.
- 객체(Object)는 프로그램 내에서 **상태(속성 또는 필드)와 행위(메소드 또는 함수)를 가지며,** 이러한 상태와 행위를 통해 현실 세계의 사물을 모델링하거나 시스템 내의 개념을 추상화하는 방식으로 사용됩니다.

> 🙋‍♂️ [추상화(Abstraction)](https://www.devkobe24.com/CS/2024/2024-08-31-Abstraction.html)
> 🙋‍♂️ [DIP의 정의에서 말하는 '추상화된 것'이란 무엇일까?](https://www.devkobe24.com/CS/2024/2024-10-07-what-is-the-abstracted-thing-mentioned-in-the-definition-of-DIP.html)
> 🙋‍♂️ [DIP의 정의에서 말하는 '추상화된 것'과 '추상화'의 개념의 차이점.](https://www.devkobe24.com/CS/2024/2024-10-07-diff-btw-abstracted-thing-and-abstraction.html)

## 1️⃣ 객체의 구성 요소.

### 1️⃣ 속성(Attributes) 또는 필드(Fields)
- 객체의 **데이터**를 나타냅니다.
    - 이는 객체가 가지는 **상태**를 표현하는 변수로, 클래스에서 정의된 속성(Attributes)에 따라 각 객체는 고유한 값을 
- 예를 들어, "자동차" 객체에는 **색상, 속도** 같은 속성이 있을 수 있습니다.
- Java에서 속성은 **인스턴스 변수**로 표현됩니다.

```java
class Car {
    private String color;
    private int speed;
    
    public Car(String color, int speed) {
        this.color = color; // 속성
        this.speed = this.speed; // 속성
    }
}
```

### 2️⃣ 메소드(Methods)
- 객체의 **행동**을 정의하는 함수입니다.
    - 메소드는 객체의 데이터를 조작하거나 특정 작업을 수행할 수 있습니다.
- 예를 들어, "자동차" 객체(Object)는 **달리기** 또는 **멈추기** 같은 메소드(Methods)를 가질 수 있습니다.
- Java에서 메소드(Methods)는 **함수**로 정의됩니다.

```java
class Car {
    private String color;
    private int speed;
    
    public Car(String color, int speed) {
        this.color = color;
        this.speed = speed;
    }
    
    public void run() {
        System.out.println("The car is running at " + this.speed + " km/h"); // 메소드(Methods)
    }
}
```

### 3️⃣ 클래스(Class)
- **객체(Object)를 생성하기 위한 청사진** 또는 **설계도**입니다.
    - 클래스는 객체의 속성(Attributes)과 메소드(Methods)를 정의하며, 객체(Object)는 이 클래스(Class)를 기반으로 생성됩니다.
- 예를 들어, "Car"라는 클래스(Class)는 자동차의 속성(색상, 속도)과 행동(달리기, 멈추기)을 정의하고, 이 클래스(Class)를 사용해 다양한 "자동차" 객체(Object)를 만들 수 있습니다.
- Java에서 클래스는 다음과 같이 정의됩니다.

```java
class Car {
    private String color;
    private int speed;
    
    public Car(String color, int speed) {
        this.color = color;
        this.speed = speed;
    }
    
    public void run() {
        System.out.println("The car is running at " + this.speed + " km/h");
    }
}
```

### 4️⃣ 인스턴스(Instance)
- **클래스(Class)로부터 생성된 실제 객체(Object)를** 의미합니다.
    - 하나의 클래스(Class)는 여러 개의 인스턴스(Instance)를 가질 수 있으며, 각 인스턴스는 고유한 속성(Attributes) 값을 가질 수 있습니다.
- 예를 들어,"Car" 클래스에서 "red_car"라는 인스턴스(Instance)를 생성할 수 있습니다.

```java
Car redCar = new Car("red", 120);
redCar.run(); // "The car is running at 120 km/h" 출력
```

## 2️⃣ 객체의 특성.

### 1️⃣ 캡슐화(Encapsulation)
- 객체(Object)는 자신의 데이터를 외부로부터 **은닉**하고, 해당 데이터를 조작하는 메소드(Methods)를 통해서만 접근을 허용하는 특성을 가집니다.
    - 이를 통해 데이터의 **무결성**을 유지할 수 있습니다.
- **캡슐화(Encapsulation)는** 객체(Object) 내부 구현 세부 사항을 외부에서 알 필요 없이 **인터페이스(Interface)만을** 통해 상호작용할 수 있도록 합니다.

### 2️⃣ 추상화(Abstraction)
- 객체(Object)는 현실 세계의 사물이나 개념을 **추상화**하여 나타냅니다.
    - 복잡한 시스템을 단순화하여 중요한 정보만을 표현하고, 불필요한 세부 사항을 숨깁니다.
- 예를 들어, 자동차의 복잡한 엔진 내부 구조는 숨기고, 사용자는 단순히 "달리기" 메소드(Methods)로 자동차를 움직일 수 있습니다.

### 3️⃣ 상속(Inheritance)
- 객체 지향 프로그래밍에서 **상속(Inheritance)은** 한 클래스가 다른 클래스의 **속성(Attributes)과 메소드(Methods)를 물려받는 것**을 의미합니다.
    - 이를 통해 기존 클래스의 기능을 확장하거나 수정하여 새로운 클래스를 정의할 수 있습니다.
- 예를 들어, "Car" 클래스는 "ElectricCar"라는 하위 클래스로 확장될 수 있습니다.
```java
class ElectricCar extends Car {
    private int batteryCapacity;
    
    public ElectricCar(String color, int speed, int batteryCapacity) {
        super(color, speed);
        this.batteryCapacity = batteryCapacity;
    }
}
```

### 4️⃣ 다형성(Polymorphism)
- **다형성(Polymorphism)은** 객체(Object)가 **같은 인터페이스(Interface)를** 사용하여 다양한 방식으로 동작할 수 있는 특성입니다.
    - 즉, 동일한 메소드(Methods)가 **다양한 클래스**에서 **다르게 구현될** 수 있습니다.
- 예를 들어, "Animal"이라는 클래스에 "speak()"라는 메소드가 있다면, 이를 상속받는 "Dog"와 "Cat" 클래스는 각각의 방식으로 이 메소드를 다르게 구현할 수 있습니다.

```java
class Animal {
    public void speak() {
        // 메소드 구현 생략
    }
}

class Dog extends Animal {
    @Override
    public void speak() {
        System.out.println("Woof");
    }
}

class Cat extends Animal {
    @Override
    public void speak() {
        System.out.println("Meow");
    }
}
```

## 3️⃣ 객체의 예시.

### 1️⃣ 자동차 객체 예시.
```java
class Car {
    private String color;
    private int speed;
    
    public Car(String color, int speed) {
        this.color = color;
        this.speed = speed;
    }
    
    public void run() {
        System.out.println("The " + this.color + " car is running at " + this.speed + " km/h");
    }
    
    public class Main {
        public static void main(String[] args) {
            // 인스턴스 생성
            Car myCar = new Car("red", 100);
            myCar.run(); // "The red car is running at 100 km/h" 출력
        }
    }
}
```

### 2️⃣ 은행 계좌 객체 예시.
```java
class BankAccount {
    private String accountNumber;
    private double balance;
    
    public BankAccount(String accountNumber, double balance) {
        this.accountNumber = accountNumber;
        this.balance = balance;
    }
    
    public void deposit(double amount) {
        balance += amount;
        System.out.println(amount + " deposited. Ned balance: " + balance);
    }
    
    public void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
            System.out.println(amount + " withdrawn. Remaining balance: " + balance);
        } else {
            System.out.println("Insufficient balance");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        // 인스턴스 생성.
        BankAccount myAccount = new BankAccount("123-456", 5000);
        myAccount.deposit(1000); // "1000 deposited. New balance: 6000" 출력
        myAccount.withdraw(2000); // "2000 withdraw Remaining balance: 4000" 출력
    }
}
```

## 4️⃣ 결론.
- **객체(Object)는 클래스(Class)에 정의된 속성(Attributes)과 메소드(Methods)를 가진 독립적인 개체로, 객체 지향 프로그래밍의 핵심 구성 요소입니다.**
- 객체(Object)는 현실 세계의 개념이나 시스템의 구성 요소를 프로그래밍적으로 표현하며, **상태(속성)와 행위(메소드)를** 가지고 있습니다.
- 객체는 **추상화, 캡슐화, 상속, 다형성**과 같은 객체 지향 원칙을 따르며, **재사용 가능성**과 **유지보수성을** 향상 시킵니다.
