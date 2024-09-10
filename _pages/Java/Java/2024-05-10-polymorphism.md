---
title: "☕️[Java] 다형성"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-05-10"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 1️⃣ 다형성.

## 1. 다형성(Polymorphism)
자바에서 말하는 **다형성(Polymorphism)은 객체가 여러 형태를 취할 수 있는 능력을 말합니다.**

이는 같은 이름의 메소드 호출이 객체의 타입에 따라 다은 동작을 수행할 수 있게 해 주어 코드의 유연성과 재사용성을 증가시킵니다.

자바에서는 주로 두 가지 형태의 다형성을 지원하는데, 이는 컴파일 시간 다형성과 런타임 다형성입니다.

## 1.2. 컴파일 시간 다형성(정적 다형성).
컴파일 시간 다형성은 주로 메소드 오버로딩을 통해 구현됩니다.
메소드 오버로딩은 동일한 메소드 이름을 가지면서 매개변수 타입, 순서, 개수가 다른 여러 메소드를 같은 클래스 내에 선언하는 것을 의미합니다.
이러한 메소드들은 컴파일 시에 그 타입에 따라 구별되어 처리됩니다.

## 1.3 컴파일 시간 다형성 예시.
```java
public class Display {
    public void print(int num) {
        System.out.println("Printing integer: " + num);
    }
    
    public void print(String str) {
        System.out.println("Printing string: " + str);
    }
}
```

## 1.4 런타임 다형성(동적 다형성).
런타임 다형성은 메소드 오버라이딩을 통해 구현됩니다.
이 경우 서브클래스에서 상속받은 부모 클래스의 메소드를 재정의하여 동일한 메소드 호출이 서로 다른 클래스 객체에 대해 다른 동작을 할 수 있도록 합니다.
이는 실행 중에 결정되므로 동적 다형성이라고 합니다.

## 1.5 런타임 다형성 예시.
```java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    void sound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {
    void sound() {
        System.out.println("Cat meows");
    }
}

public class TestPolymorphism {
    public static void main(String[] args) {
        Animal myAnimal = new Dog();
        myAnimal.sound(); // 출력: Dog barks
        
        myAnimal = new Cat();
        myAnimal.sound(); // 출력: Cat meows
    }
}
```
- 여기서 **'Animal'** 클래스의 **'sound()'** 메소드는 **'Dog'** 와 **Cat** 클래스에서 오버라이딩되었습니다.
    - **'myAnimal'** 참조 변수는 **'Animal'** 타입이지만, 참조하는 객체의 실제 타입에 따라 적절한 **'sound()'** 메소드가 호출됩니다.

## 1.6 다형성의 장점.
- **유연성 :** 다형성을 사용하면 프로그램을 더 유연하게 설계할 수 있습니다.
    - 예를 들어, 다양한 지식 클래스의 객체들을 부모 클래스 타입의 컬렉션에 저장하고, 각 객체에 대해 공통된 인터페이스를 통해 작업을 수행할 수 있습니다.

- **코드 재사용과 유지 보수의 향상 :** 공통 인터페이스를 사용함으로써 코드를 재사용하고, 새로운 클래스 타입을 추가하거나 기존 클래스를 수정할 때 유지 보수가 용이해집니다.

## 📝 정리.
이렇게 다형성은 객체 지향 프로그래밍의 중요한 특성 중 하나로, 프로그램의 다양한 부분에서 유용하게 활용됩니다.

---

## 2. instanceof
자바 프로그래밍에서 **'instanceof'** 연산자는 특정 객체가 지정한 타입의 인스턴스인지를 검사하는 데 사용됩니다.
이 연산자는 객체의 타입을 확인할 때 유용하게 쓰이며, 주로 객체의 실제 타입을 판별하여 안전하게 형 변환을 하기 전이나 특정 타입에 따른 조건 분기를 실행할 때 사용됩니다.

## 2.1 instanceof 연산자의 사용법.
**'instanceof'** 는 구 개의 피 연산자를 비교합니다.
- 왼쪽 피연산자는 객체를 나타내며, 오른쪽 피연산자는 타입(클래스나 인터페이스)을 나타냅니다.
- 연산의 결과는 불리언 값입니다.
    - 만약 왼쪽 피연산자가 오른쪽 피연산자가 지정하는 타입의 인스턴스면 **'true'** 를, 그렇지 않으면 **'false'** 를 반환합니다.

**기본 구조**
```java
if (object instanceof ClassName) {
    // 조건이 참일 때 실행될 코드
}
```

**예시**
```java
class Animal {}
class Dog extends Animal {}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Animal();
        Dog dog = new Dog();
        Animal animalDog = new Dog();
        
        System.out.println(animal instanceof Animal); // true
        System.out.println(dog instanceof Animal); // true
        System.out.println(animalDog instanceof Animal); // true
        System.out.println(animal instanceof Dog); // false
    }
}
```

- 이 예시에서 **'dog instanceof Animal'** 은 **'true'** 를 반환합니다.
    - 왜냐하면 **'Dog'** 클래스가 **'Animal'** 클래스의 서브클래스이기 때문입니다.
    - 하지만 **'animal instanceof Dog'** 은 **'false'** 를 반환하는데, 이는 **'Animal'** 인스턴스가 **'Dog'** 타입이 아니기 때문입니다.

## 2.2 instanceof의 주의점
- **1. null 검사 :** **'instanceof'** 는 객체 참조가 **'null'** 일 때 항상 **'false'** 를 반환합니다.
    - 따라서 **'null'** 값에 대한 추가적인 검사 없이도 안전하게 사용할 수 있습니다.

- **2. 다운캐스팅 검증 :** 객체를 하위 클래스 타입으로 다운캐스팅하기 전에 **'instanceof'** 를 사용하여 해당 객체가 실제로 해당 하위 클래스의 인스턴스인지를 확인하는 것이 안전합니다.
    - 이를 통해 **'ClassCastException'** 을 예발할 수 있습니다.

- **3. 인터페이스 검사 :** **'instanceof'** 는 클래스 뿐만 아니라 인터페이스 타입에 대해서도 사용할 수 있습니다. 객체가 특정 인터페이스를 구현하는지 여부를 검사할 수 있습니다.

## 📝 정리.
**'instanceof'** 는 다형성을 사용하는 객체 지향 프로그램에서 객체의 타입을 안전하게 확인하고, 타입에 맞는 적절한 동작을 수행하도록 도와주는 중요한 도구입니다.

---

## 3. 업캐스팅(Upcasting).
자바 프로그래밍에서 업캐스팅(Upcasting)은 서브클래스의 객체를 슈퍼클래스 타입의 참조로 변환하는 과정을 말합니다.
이는 일반적으로 자동으로 수행되며, 명시적으로 타입을 지정할 필요가 없습니다.
업캐스팅은 객체 지향 프로그래밍의 다형성을 활용하는 데 핵심적인 역할을 합니다.

## 3.1 업캐스팅의 특징과 이점.
- **1. 자동 형 변환 :** 자바에서는 서브클래스의 객체를 슈퍼클래스 타입의 탐조 변수에 할당할 때 자동으로 업캐스팅이 발생합니다.
- **2. 안전성 :** 업캐스팅은 항상 안전하며, 데이터 손실이나 오류 없이 수행됩니다. 이는 서브클래스가 슈퍼클래스의 모든 특성을 상속받기 때문입니다.
- **3. 다형적 행동 :** 업캐스팅을 통해 서브클래스의 객체들을 슈퍼클래스 타입으로 다룰 수 있어, 다양한 타입의 객체들을 일관된 방식으로 처리할 수 있습니다. 이를 통해 코드의 유연성과 재사용성이 향상됩니다.

## 3.2 예시.
아래는 업캐스팅을 사용한 자바 코드 예시입니다.
```java
class Animal {
    public void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
    public void bark() {
        System.out.println("Dog is barking");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog myDog = new Dog();
        Animal myAnimal = myDog; // Dog 객체를 Animal 타입으로 업캐스팅
        
        myAnimal.eat(); // 호출 가능
        // myAnimal.bark(); // 컴파일 에러, Animal 타입은 bark 메소드를 알지 못함
    }
}
```

- 이 예시에서 **'Dog'** 객체가 **'Animal'** 타입으로 업캐스팅 되었습니다.
    - **'myAnimal'** 변수는 **'Animal'** 클래스의 메소드만 호출할 수 있으며, **'Dog'** 클래스의 **'bark()'** 메소드는 호출할 수 없습니다.

## 3.3 업캐스팅 후의 제한사항.
업캐스팅을 한 후에는 원래 서브클래스의 특정 메소드나 속성에 접근할 수 없게 됩니다.
즉, 업캐스팅된 객체는 슈퍼클래스의 필드와 메소드만 사용할 수 있으며, 추가된 서브클래스의 특성은 사용할 수 없습니다.
이는 다형성의 한 예로서, 슈퍼 클래스 타입을 통해 다양한 서브클래스의 객체들을 통합적으로 다룰 수 있도록 해주며, 프로그램을 더 유연하고 확장 가능하게 만듭니다.

---

## 4. 다운캐스팅(Downcasting).
자바 프로그래밍에서 다운캐스팅(Downcasting)은 슈퍼클래스 타입의 객체 참조를 서브클래스 타입의 참조로 변환하는 과정을 말합니다.
다운캐스팅은 업캐스팅의 반대 과정으로, 업캐스팅된 객체를 다시 원래의 서브클래스 타입으로 변환할 때 사용됩니다.
다운캐스팅은 명시적으로 수행되어야 하며, 자바에서는 이 과정이 자동으로 이루어지지 않습니다.

## 4.1 다운캐스팅의 필요성.
업캐스팅을 통해 객체가 슈퍼클래스 타입으로 변환되면, 해당 객체는 슈퍼클래스의 메소드와 필드만 접근 가능합니다.
서브클래스에만 있는 메소드나 필드에 접근하려면 다운캐스팅을 사용하여 해당 객체를 다시 서브클래스 타입으로 변환해야 합니다.

## 4.2 다운캐스팅의 사용법과 주의사항.
다운캐스팅은 타입 캐스팅 연산자를 사용하여 수행되며, 반드시 **'instanceof'** 연산자로 타입 체크를 먼저 수행하는 것이 안전합니다.
이는 변환하려는 객체가 실제로 해당 서브클래스의 인스턴스인지 확인하여 **'ClassCastExecption'** 을 방지하기 위함입니다.

## 4.3 예시.
다운캐스팅을 사용하는 자바 코드 예시입니다.
```java
class Animal {
    public void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
    public void bark() {
        System.out.println("Dog is barking");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myAnimal = new Dog(); // 업캐스팅
        myAnimal.eat();
        
        // 다운캐스팅 전에 instanceof로 체크
        if (myAnimal instanceof Dog) {
            Dog myDog = (Dog) myAnimal; // 다운캐스팅
            myDog.bark(); // 이제 서브클래스의 메소드 호출 가능
        }
    }
}
```

- 이 예시에서 **'Animal'** 타입의 **'myAnimal'** 객체는 **'Dog'** 클래스의 인스턴스입니다.
    - **'myAnimal'** 을 **'Dog'** 타입으로 다운캐스팅하여 **'Dog'** 클래스의 **'bark()'** 메소드에 접근할 수 있습니다.
    - 다운캐스팅을 수행하기 전에 **'instanceof'** 를 사용해 **'myAnimal'** 이 실제로 **'Dog'** 의 인스턴스인지 확인함으로써 안정을 확보합니다.

## 4.4 주의사항.
- 다운캐스팅은 객체의 실제 타입이 캐스팅하려는 클래스 타입과 일치할 때만 안전하게 수행됩니다.
- 잘못된 다운캐스팅은 런타임에 **'ClassCastException'** 을 발생시킬 수 있습니다.

## 📝 정리.
다운캐스팅은 특정 상황에서 필수적이며, 객체의 모든 기능을 활용하기 위해 사용되지만, 항상 타입 검사를 수행하고 신중하게 사용해야 합니다.
