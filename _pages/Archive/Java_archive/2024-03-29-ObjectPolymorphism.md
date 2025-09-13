---
title: "☕️[Java] Object 다형성"
tags:
    - Java
    - Programming Language
date: "2024-03-29"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# Object 다형성.
`Object`는 모든 클래스의 부모 클래스입니다.
- 따라서 `Object`는 모든 객체를 참조할 수 있습니다.

예제를 통해서 `Object`의 다형성에 대해서 알아봅시다.

<img src = "https://github.com/devKobe24/images/blob/main/Object%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B71.png?raw=true">

`Dog`와 `Car`은 서로 아무런 관련이 없는 클래스입니다.
- 둘다 부모가 없으므로 `Object`를 자동으로 상속 받습니다.

```java
package lang.object.poly;

public class Car {
 public void move() {
   System.out.println("자동차 이동");
 }
}
```

```java
package lang.object.poly;

public class Dog {

  public void sound() {
    System.out.println("멍멍");
  }
}
```

```java
package lang.object.poly;

public class ObjectPolyExample1 {

  public static void main(String[] args) {
    Dog dog = new Dog();
    Car car = new Car();

    action(dog);
    action(car);
  }

  private static void action(Object obj) {
    //obj.sound(); // 컴파일 오류, Object는 sound()가 없다.
    //obj.move(); // 컴파일 오류, Object는 move()가 없다.

    // 객체에 맞는 다운캐스팅이 필요함.
    if (obj instanceof Dog dog) {
      dog.sound();
    } else if (obj instanceof Car car) {
      car.move();
    }
  }
}
```

**실행 결과**
```
멍멍
자동차 이동
```

`Object`는 모든 타입의 부모입니다.

부모는 자식을 담을 수 있으므로 앞의 코드를 다음과 같이 변경해도 됩니다.

```java
Object dog = new Dog(): // Dog -> Object
Object car = new Car(): // Car -> Object
```

## Object 다형성의 장점.
`action(Object obj)` 메서드를 분석해봅시다.
- 이 메서드는 `Object` 타입의 매개변수를 사용합니다.
    - 그런데 `Object`는 모든 객체의 부모입니다.
        - 따라서 어떤 객체든지 인자로 전달 할 수 있습니다.

```java
action(dog) // main에서 dog 전달
void action(Object obj = dog(Dog)) //Object는 자식인 Dog 타입을 참조할 수 있습니다.
```

```java
action(car)
void action(Object obj = car(Car)) // Object는 자식인 Car 타입을 참조할 수 있습니다.
```

## Object 다형성의 한계.
```java
action(dog) // main에서 dog 전달
private static void action(Object obj) {
    obj.sound(); // 컴파일 오류, Object는 sound()가 없습니다.
}
```

`action()` 메서드안에서 `obj.sound()`를 호출하면 오류가 발생합니다.
- 왜냐하면 매개변수인 `obj`는 `Object` 타입이기 때문입니다.
    - `Object`에는 `sound()` 메서드가 없습니다.

<img src = "https://github.com/devKobe24/images/blob/main/Object%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%B4%E1%84%92%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A8.png?raw=true">

**obj.sound() 호출**
- `obj.sound()`를 호출합니다.
- `obj`는 `Object` 타입이므로 `Object` 타입에서 `sound()`를 찾습니다.
- `Object`에서 `sound()`를 찾을 수 없습니다. 
    - `Object`는 최종 부모이므로 더는 올라가서 찾을 수 없습니다. 
        - 따라서 오류가 발생합니다.

`Dog` 인스턴스의 `sound()`를 호출하려면 다음과 같이 다운캐스팅을 해야합니다.
```java
if (obj instanceof Dog dog) {
    dog.sound();
}
```

<img src = "https://github.com/devKobe24/images/blob/main/Object%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%B4%E1%84%92%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A82.png?raw=true">
- `Object obj`의 참조값을 `Dog dog`로 다운캐스팅 하면서 전달합니다.
- `dog.sound()`를 호출하면 `Dog` 타입에서 `sound()`를 찾아서 호출합니다.

## Object를 활용한 다형성의 한계.
- `Object`는 모든 객체를 대상으로 다형적 참조를 할 수 있습니다.
    - 쉽게 이야기해서 `Object`는 모든 객체의 부모이므로 모든 객체를 담을 수 있습니다.
- `Object`를 통해 전달 받은 객체를 호출하려면 각 객체에 맞는 다운캐스팅 과정이 필요합니다.
    - `Object`가 세상의 모든 메서드를 알고 있는 것이 아닙니다.

다형성을 제대로 활용하려면 **다형적 참조 + 메서드 오버라이딩**을 함께 사용해야 합니다.
- 그런면에서 `Object`를 사용한 다형성에는 한계가 있습니다.

`Object`는 모든 객체의 부모이므로 모든 객체를 대상으로 다형적 참조를 할 수 있습니다.
- 하지만 `Object`에는 `Dog.sound()`, `Car.move()`와 같은 다른 객체의 메서드가 정의되어 있지 않습니다.
    - 따라서 메서드 오버라이딩을 활용할 수 없습니다.
        - 결국 각 객체의 기능을 호출하려면 다운캐스팅을 해야 합니다.
- 참고로 `Object` 본인이 보유한 `toString()` 같은 메서드는 당연히 자식 클래스에서 오버라이딩 할 수 있습니다.
    - 여기서 이야기하는 것은 앞서 설명한 `Dog.sound()`, `Car.move()` 같은 `Object`에 속하지 않은 메서드를 말합니다.

결과적으로 다형적 참조는 가능하지만, 메서드 오버라이딩이 안되기 때문에 다형성을 활용하기에는 한계가 있습니다.
