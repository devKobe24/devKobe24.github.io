---
title: "☕️[Java] 다형성(Polymorphism)"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-06-09"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 1️⃣ 다형성(Polymorphism).

**'다형성(Polymorphism)'** 은 **'객체 지향 프로그래밍(OOP)'** 의 중요한 개념 중 하나로, 같은 인터페이스를 통해 서로 다른 데이터 타입의 객체를 조작할 수 있도록 합니다.

다형성은 코드의 재사용성과 유연성을 높여주며, 유지보수를 쉽게 해줍니다.

Java에서 **'다형성'** 은 주로 **'상속'** 과 **'인터페이스'** 를 통해 <u>구현됩니다.</u>

# 2️⃣ 다형성의 개념.

다형성은 **"하나의 인터페이스로 여러 가지 형태를 구현할 수 있는 능력"** 을 의미합니다.

<u>이는 같은 메서드가 다양한 객체에서 다르게 동작할 수 있게 합니다.</u>

# 3️⃣ 다형성의 두 가지 형태.

## 1️⃣ 컴파일 시간 다형성(Compile-time Polymorphism)

- 메서드 오버로딩(Method Overloading)을 통해 구현됩니다.

- 컴파일 시점에 어떤 메서드가 호출될지 결정됩니다.

- 같은 이름의 메서드를 여러 개 정의하지만, 매개변수의 타입이나 개수가 달라야 합니다.

## 2️⃣ 런타임 다형성 (Runtime Polymorphism)

- 메서드 오버라이딩(Method Overriding)을 통해 구현됩니다.

- 실행 시점에 어떤 메서드가 호출될지 결정됩니다.

- 부모 클래스의 메서드를 자식 클래스에서 재정의하여 사용합니다.

# 4️⃣ 컴파일 시간 다형성(Method Overloading).

<u>메서드 오버로딩은 같은 클래스 내에서 같은 이름을 가진 메서드를 여러 개 정의하는 것입니다.</u>

단, 매개변수의 수나 타입이 달라야 합니다.

## 💻 예제.

```java
public class MathOperations {
	// 정수 두 개의 합
	public int add(int a, int b) {
		return a + b;
	}

	// 실수 두 개의 합
	public double add(double a, double b) {
		return a + b;
	}

	// 새 개의 정수의 합
	public int add(int a, int b, int c) {
		return a + b + c;
	}

	public static void main(String[] args) {
		MathOperations mathOperations = new MathOperations();
		System.out.println(mathOperations.add(1, 2)); // 3
		System.out.println(mathOperations.add(1.5, 2.5)); // 4.0
		System.out.println(mathOperations.add(1, 2, 3)); // 6
	}
}
```

# 5️⃣ 런타임 다형성(Method Overriding).

<u>메서드 오버라이딩은 자식 클래스가 부모 클래스의 메서드를 재정의하는 것을 말합니다.</u>

이를 통해 자식 클래스의 객체가 부모 클래스의 메서드를 호출할 때, 자식 클래스의 메서드가 실행되도록 합니다.

## 💻 예제.

```java
class Animal {
	void makeSound() {
		System.out.println("Animal makes a sound");
	}
}

class Dog extends Animal {
	@Override
	void makeSound() {
		System.out.println("Dog barks");
	}
}

class Cat extends Animal {
	@Override
	void makeSound() {
		System.out.println("Cat meows");
	}
}

public class Main {

	public static void main(String[] args) {
		Animal myDog = new Dog(); // Animal 타입으로 Dog 객체 생성
		Animal myCat = new Cat(); // Animal 타입으로 Cat 객체 생성

		myDog.makeSound(); // Dog barks
		myCat.makeSound(); // Cat meows
	}
}
```

# 6️⃣ 인터페이스를 통한 다형성.

인터페이스를 통해서도 다형성을 구현할 수 있습니다.

<u>인터페이스는 메서드의 서명만을 정의하며, 이를 구현하는 클래스가 메서드의 구체적인 동작을 정의합니다.</u>

## 💻 예제.

```java
interface Shape {
	void draw();
}

class Circle implements Shape {
	@Override
	public void draw() {
		System.out.println("Drawing a Circle");
	}
}

class Square implements Shape {
	@Override
	public void draw() {
		System.out.println("Drawing a Square");
	}
}

public class Main {

	public static void main(String[] args) {
		Shape myShape1 = new Circle();
		Shape myShape2 = new Square();

		myShape1.draw(); // Drawing a Circle
		myShape2.draw(); // Drawing a Square
	}
}
```

# 7️⃣ 다형성의 장점.

1. **코드 재사용성 :** 상위 클래스나 인터페이스를 사용하여 다양한 하위 클래스나 구현체를 다룰 수 있어 코드의 재사용성이 높아집니다.

2. **유연성 :** 새로운 클래스나 기능을 추가할 때 기존 코드를 수정할 필요 없이 확장할 수 있습니다.

3. **유지보수성 :** 코드를 이해하고 유지보수하는 것이 더 쉬워집니다. 메서드의 호출이 어디서 어떻게 이루어지는지 명확하기 때문입니다.

# 8️⃣ 예제: 다형성의 실질적 사용.

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class PolymorphismExample {

	public static void main(String[] args) {
		List<String> arrayList = new ArrayList<>();
		List<String> linkedList = new LinkedList<>();

		arrayList.add("ArrayList Item");
		linkedList.add("LinkedList Item");

		printList(arrayList); // ArrayList Item
		printList(linkedList); // LinkedList Item
	}

	public static void printList(List<String> list) {
		for (String item : list) {
			System.out.println(item);
		}
	}
}
```

> 이 예제에서는 **'`List`'** 인터페이스를 사용하여 **'`ArrayList`'** 와 **'`LinkedList`'** 를 동일한 방식으로 처리합니다.
> 이를 통해 다양한 구현체를 다룰 수 있는 유연한 코드를 작성할 수 있습니다.

# 📝 결론.

다형성은 객체 지향 프로그래밍의 핵심 개념 중 하나로, 코드의 유연성과 재사용성을 크게 향상시킵니다.

이를 통해 다양한 형태의 객체를 동일한 방식으로 다룰 수 있으며, 새로운 기능을 쉽게 확장하고 유지보수할 수 있습니다.

다형성은 상속과 인터페이스를 통해 구현되며, 메서드 오버로딩과 오버라이딩을 통해 다양한 형태를 취할 수 있습니다.
