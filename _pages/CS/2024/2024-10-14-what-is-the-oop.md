---
title: "💾 [CS] 객체 지향 프로그래밍(Object-Oriented Programming, OOP)는 무엇일까요?"
tags:
    - CS
date: "2024-10-14"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 객체 지향 프로그래밍(Object-Oriented Programming, OOP)는 무엇일까요?
- 객체 지향 프로그래밍(Object-Oriented Programming, OOP)은 프로그램을 **객체(Object)를 중심**으로 구성하는 프로그래밍 패러다임입니다.
- 객체(Object)는 데이터와 이 데이터를 처리하는 함수를 묶은 개념으로, 현실 세계의 사물을 프로그램 내에서 모방하여 설계하는 방식입니다.
- 객체 지향 프로그래밍(Object-Oriented Programming, OOP)의 핵심 개념은 다음과 같습니다.

> 🙋‍♂️ [객체 지향 프로그래밍에서의 객체(Object)란 무엇일까요?](https://www.devkobe24.com/CS/2024/2024-10-13-what-is-the-object.html)

## 1️⃣ 클래스와 객체.

### 👉 클래스(Class).
- 객체(Object)를 생성하기 위한 청사진이나 틀입니다.
    - 클래스는 속성(데이터)과 메서드(함수)를 정의합니다.

### 👉 객체(Object).
- 클래스(Class)를 기반으로 생성된 실체로, 클래스에 정의된 속성과 메서드를 사용합니다.
    - 객체는 클래스의 인스턴스라고도 불립니다.

### 👉 예시.
```java
// 클래스 정의
class Car {
    // 속성 (필드)
    String model;
    String color;
    
    // 생성자
    public Car(String model, String color) {
        this.model = model;
        this.color = color;
    }
    
    // 메서드
    public void drive() {
        System.out.println(model + "이(가) 달립니다.");
    }
}

// 객체 생성 및 사용
public class Main {
    public static void main(String[] args) {
        // Car 클래스의 인스턴스(객체) 생성.
        Car car1 = new Car("Volvo xc60", "Black");
        car1.drive(); // 출력: Volvo xc60이(가) 달립니다.
    }
}
```

- 위 예시는 **Car**라는 **클래스**를 정의하고, 그 클래스를 기반으로 **Volvo xc60**이라는 객체를 생성한 후 `drive()` 메서드를 호출하는 과정입니다.

## 2️⃣ 캡슐화(Encapsulation)
- 객체 내부의 데이터(속성)와 이 데이터를 조작하는 메서드를 하나로 묶는 것을 말합니다.
    - 캡슐화(Encapsulation)를 통해 객체(Object) 외부에서는 내부 구현을 알지 못하게 하고, 제공된 메서드를 통해서만 데이터를 접근하거나 변경할 수 있게 만듭니다.
- 이를 통해 데이터 보호와 코드의 응집성을 높일 수 있습니다.

### 👉 예시

```java
class Person {
    // private 속성 (외부에서 직접 접근할 수 없음)
    private String name;
    private int age;
    
    // 생성자
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // public 메서드를 통해 간접적으로 접근
    public String getName() {
        this.name = name;
    }
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        if(age > 0) { // 유효성 검사
            this.age = age;
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Person p = new Person("Kobe", 25);
        System.out.println(p.getName()); // 출력: Kobe
        p.setAge(30);
        System.out.println(p.getAge()); // 출력: 30
    }
}
```

- 이 예시에서는 **Person 클래스**에서 **캡슐화(Encapsulation)가** 적용되어, `name`과 `age` 같은 속성은 `private`으로 선언되어 외부에서 직접 접근할 수 없으며, 이를 조작하기 위해서는 제공된 메서드(`getName()`, `setAge()`)를 통해 간접적으로 접근하게 됩니다.

## 3️⃣ 상속(Inheritance)
- 상속(Inheritance)은 기존 클래스를 확장하여 새로운 클래스를 만드는 방법입니다.
    - 자식 클래스는 부모 클래스의 속성과 매서드를 물려받아 재사용하며, 필요하면 추가로 기능을 확장하거나 수정할 수 있습니다.
- **상속(Inheritance)을 통해** 코드의 **재사용성을 높이고, 계층 구조를** 만들 수 있습니다.

### 👉 예시
```java
// 부모 클래스.
class Animal {
    String name;
    
    public Animal(String name) {
        this.name = name;
    }
    
    public void makeSound() {
        System.out.println("동물이 소리를 냅니다.")
    }
}

// 자식 클래스 (상속)
class Dog extends Animal {
    public Dog(String name) {
        super(name); // 부모 클래스의 생성자를 호출
    }
    
    // 부모 메서드 오버라이딩
    @Override
    public void makeSound() {
        System.out.println(name + "이(가) 멍멍 짖습니다.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog("나르");
        dog.makeSound(); // 출력: 나르이(가) 멍멍 짖습니다.
    }
}
```

- 위 예시에서, **Dog** 클래스(Class)는 **Animal** 클래스(Class)를 상속받아 **makeSound()** 메서드를 재정의(Overriding, 오버라이딩)하고, 이름을 출력하도록 확장했습니다.
- 상속(Inheritance)을 통해 부모 클래스의 기능을 재사용하면서도 필요에 따라 추가적인 기능을 구현할 수 있습니다.

## 4️⃣ 다형성(Polymorphism)
- **다형성(Polymorphism)은** 같은 이름의 메서드가 다양한 방법으로 동작할 수 있게 하는 기능입니다.
    - 상속(Inheritance) 관계에서 부모 클래스(Parents class, Super class)의 메서드(Methods)를 자식 클래스(Child class, Sub class)에서 재정의(오버라이딩, Overriding)하여 다른 방식으로 동작하게 하거나, 같은 이름의 메서드가 서로 다른 매개변수(Paremeter)에 따라 다르게 동작(오버로딩, Overloading)할 수 있습니다.
- **다형성(Polymorphism)을** 통해 코드의 **유연성과 확장성을 높일 수 있습니다.**

### 👉 예시.
```java
class Animal {
    public void makeSound() {
        System.out.println("동물이 소리를 냅니다.");
    }
}

class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("멍멍");
    }
}

class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("야옹");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal1 = new Dog();
        Animal animal2 = new Cat();
        
        animal1.makeSound(); // 출력: 멍멍
        animal2.makeSount(); // 출력: 야옹
    }
}
```

- 이 예시는 **다형성(Polymorphism)을** 보여주는 좋은 예입니다.
    - `Animal` 타입의 변수에 **Dog**와 **Cat** 객체를 할당할 수 있으며, `makeSound()` 메서드를 **호출하면** 객체의 타입에 맞게 다르게 동작합니다.
        - 이처럼 부모 클래스 타입으로 다양한 자식 객체를 처리할 수 있습니다.

## 5️⃣ 추상화(Abstraction)
- 추상화(Abstraction)은 복잡한 현실의 객체에서 필요한 부분만을 모델링하여 객체로 표현하는 과정입니다.
    - 불필요한 세부 사항은 숨기고 중요한 부분만 드러내어 효율적으로 문제를 해결하는 데 도움을 줍니다.
- 추상화(Abstraction)는 인터페이스(Interface)나 추상 클래스를 통해 구현됩니다.

### 👉 예시.
```java
// 추상 클래스
abstract class Vehicle {
    String model;
    
    public Vehicle(String model) {
        this.model = model;
    }
    
    // 추상 메서드 (구체적인 구현은 하위 클래스에서)
    public abstract void move();
}

// 자식 클래스 (구체적인 구현 제공)
class Car extends Vehicle {
    public Car(String model) {
        super(model);
    }
    
    @Override
    public void move() {
        System.out.println(model + "이(가) 도로에서 달립니다.");
    }
}

class Airplane extends Vehicle {
    public Airplane(String model) {
        super(model);
    }
    
    @Override
    public void move() {
        System.out.println(model + "이(가) 하늘을 납니다.");
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle car = new Car("Volvo xc60");
        Vehicle airplane = new Airplane("Boeing 747");
        
        car.move(); // 출력: Volvo xc60이(가) 도로에서 달립니다.
        airplane.move(); // 출력: Boeing 747이(가) 하늘을 납니다.
    }
}
```

- 위 예시에서는 **추상 클래스(Abstract class)를** 통해서 **추상화(Abstraction)를** 보여줍니다.
    - **Vehicle** 클래스는 **추상 클래스(Abstract class)이며,** 자식 클래스인 **Car** 와 **Airplane**이 **구체적인 구현을 제공**합니다.
        - **추상화(Abstraction)를** 통해 **공통적인 동작을 정의**하면서도 **각 객체가 개별적인 동작을 구현**할 수 있습니다.

## 6️⃣ 갈무리.
- 이러한 개념을 바탕으로 객체 지향 프로그래밍(Object-Oriented Programming, OOP)은 코드의 재사용성을 높이고, 유지보수와 확장성을 개선하며, 현실 세계의 문제를 더 직관적으로 해결할 수 있도록 합니다.
