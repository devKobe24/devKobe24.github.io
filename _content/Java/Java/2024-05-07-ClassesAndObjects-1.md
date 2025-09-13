---
title: "☕️[Java] 클래스와 객체(1)"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-05-07"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 1️⃣ 클래스와 객체(1)

## 1. 클래스(Class)
자바 프로그래밍 언어에서 클래스는 객체를 생성하기 위한 설계도 혹은 템플릿입니다.
클래스는 객체의 상태를 정의하는 필드(변수)와 객체의 행동을 정의하는 메서드(함수)로 구성됩니다.

클래스를 사용하는 주된 목적은 데이터와 그 데이터를 조작하는 방법들은 하나의 장소에 묶어 관리하기 위함입니다.
이를 통해 데이터 추상화, 캡슐화, 상속, 다형성 등의 객체지향 프로그래밍의 주요 개념들을 구현할 수 있습니다.

## 1.2 클래스의 구성 요소.
- **1. 필드(Field) :** 객체의 데이터 또는 상태를 저장하는 변수입니다.
- **2. 메서드(Method) :** 객체가 수행할 수 있는 행동을 정의한 코드 블록입니다.
    - 메서드는 필드의 값을 처리하거나 다른 메서드를 호출할 수 있습니다.
- **3. 생성자(Constructor) :** 클래스로부터 객체를 생성할 때 초기화를 담당하는 특별한 종류의 메서드입니다.
    - 생성자는 클래스 이름과 같은 이름을 가집니다.

## 1.3 클래스 예제
자바에서의 간단한 클래스 예제를 살펴보겠습니다.
```java
public class Car {
    // 필드(변수)
    private String color;
    private String model;
    
    // 생성자
    public Car(String color, String model) {
        this.color = color;
        this.model = model;
    }
    
    // 메서드
    public void drive() {
        System.out.println(model + " 색상의 " + color + " 자동차가 주행 중입니다.");
    
    }
}

// 객체 생성 및 사용
public class Test {
    public static void main(String[] args) {
        Car myCar = new Car("레드", "테슬라");
        myCar.drive();
    }
}
```

- 위의 예제에서 **'Car'** 클래스는 'color'와 'model'이라는 두 개의 필드를 가지며, 이는 각각 자동차의 색상과 모델을 나타냅니다.
- **'Car'** 클래스의 객체를 생성할 때는 **'new'** 키워드와 함께 생성자를 호출하여 초기 상태를 설정합니다.
- **'drive'** 메서드는 자동차가 주행하고 있음을 시뮬레이션하는 기능을 합니다.

## 📝 정리.
클래스를 사용함으로써 코드의 재사용성, 관리성 및 확장성이 향상되며, 대규모 소프트웨어 개발에서 필수적인 요소가 됩니다.

---

## 2. 객체(Object)와 인스턴스(Instance).
자바 프로그래밍에서 "객체(Object)"와 "인스턴스(Instance)"는 매우 중요한 개념입니다.
이 두 용어는 종종 서로 바꿔 쓰이지만, 각각의 의미에는 약간의 차이가 있습니다.

## 2.1 객체(Object).
객체는 소프트웨어 세계의 구성 요소로, 실제 세계의 객체를 모방한 것입니다.
객체는 데이터(속성)와 그 데이터를 조작할 수 있는 함수(메서드)를 캡슐화합니다.
객체는 클래스에 정의된 속성과 기능을 실제로 사용할 수 있도록 메모리상에 할당된 구조입니다.
객체의 개념은 클래스의 특성을 실제로 구현하는 것입니다.

## 2.2 인스턴스(Instance).
인스턴스는 클래스 타입에 따라 생성된 객체를 의미합니다.
예를 들어, **'Car'** 클래스의 구체적인 객체(예: 빨간색 테슬라 자동차, 파란색 현대 자동차 등)는 모두 **'Car'** 클래스의 인스턴스입니다.
즉, 인스턴스는 특정 클래스의 구현체입니다.
인스턴스라는 용어는 주로 객체가 메모리에 할당되어 실제로 생성되었음을 강조할 때 사용됩니다.

## 2.3 객체와 인스턴스의 관계.
간단히 말해, 모든 인스턴스는 객체입니다, 하지만 사용된 맥락에 따라 '인스턴스'라는 용어는 그 객체가 특정 클래스의 구현체임을 명시적으로 나타낼 때 사용됩니다.
예를 들어, 우리가 **'new Car("blue", "Hyundai")'** 를 통해 생성한 객체는 **'Car'** 클래스의 인스턴스입니다.

## 2.4 예제 코드.
```java
public class Animal {
    private String name;
    
    public Animal(String name) {
        this.name = name;
    }
    
    public void speak() {
        System.out.println(name + " makes a noise.");
    }
}

public class Test {
    public static void main(String[] args) {
        // 여기서 'dog'는 Animal 클래스의 객체이자 인스턴스입니다.
        Animal dog = new Animal("Dog");
        dog.speak();
    }
}
```

- 위 예제에서 **'Animal'** 클래스가 있고, **'main'** 메서드에서 **'Animal'** 클래스의 새 객체를 생성합니다.
    - 여기서 **'dog'** 는 **'Animal'** 클래스의 인스턴스이며 객체입니다.
        - **'dog'** 는 **'Animal'** 클래스에 정의된 메서드와 필드를 사용할 수 있습니다.

## 📝 정리.
요약하면, 객체는 속성과 메서드를 갖는 소프트웨어의 기본 구성 단위이고, 인스턴스는 그 객체가 특정 클래스의 실제 구현체임을 의미합니다.
이 두 용어는 프로그래밍에서 클래스 기반의 객체를 생성하고 다룰 때 핵심적인 역할을 합니다.


클래스와 객체의 관계를 이해
기본 사용 방법과 생성자 및 this의 

---

## 3. 메소드(Method).
자바 프로그래밍에서 메소드(Method)는 클래스에 속한 함수로서, 특정 작업을 수행하는 코드 블록입니다.
메소드는 객체의 행동을 정의하며, 클래스 내에서 정의된 데이터나 상태(필드)를 조작하는 데 사용됩니다.

메소드를 통해 객체지향 프로그래밍의 중요한 특징인 캡슐화와 추상화를 구현할 수 있습니다.

## 3.1 메소드의 주요 특징.
- **1. 재사용성 :** 메소드는 코드의 재사용성을 증가시킵니다. 한 번 정의된 메소드는 여러 위치에서 호출되어 사용될 수 있습니다.
- **2. 모듈성 :** 메소드를 사용함으로써 큰 프로그램을 작은 단위로 나누어 관리할 수 있습니다. 이는 코드의 가독성과 유지보수성을 향상시킵니다.
- **3. 정보 은닉 :** 메소드를 통해 구현 세부사항을 숨기고 사용자에게 필요한 기능만을 제공할 수 있습니다.

## 3.2 메소드의 구성 요소.
- **1. 메소드 이름 :** 메소드를 식별하는 데 사용되며, 메소드가 수행하는 작업을 설명하는 명확한 이름을 가집니다.
- **2. 매개변수 목록(Parameter List) :** 메소드에 전달되는 인자의 타입, 순서, 그리고 개수를 정의합니다. 매개변수는 선택적일 수 있습니다.
- **3. 반환 타입 :** 메소드가 작업을 수행한 후 반환하는 데이터의 타입입니다. 반환할 데이터가 없으면 **'void'** 로 지정됩니다.
- **4. 메소드 바디 :** 실제로 메소드가 수행할 작업을 구현하는 코드 블록입니다.

## 3.3 예제.
자바에서 간단한 메소드 예제를 보여드리겠습니다.
```java
public class Calculator {
    // 메소드 정의: 두 정수의 합을 반환
    public int add(int num1, int num2) {
        return num1 + num2;
    }
    
    // 메소드 정의: 두 정수의 차를 반환
    public int subtract(int num1, int num2) {
        return num1 - num2;
    }
}

public class Test {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        int result1 = calc.add(5, 3); // 8 반환
        int result2 = calc.subtract(5, 3); // 2 반환
        System.out.println("Addition Result: " + result1);
        System.out.println("Subtraction Result: " + result2);
    }
}
```

- 이 예제에서 **'Calculator'** 클래스는 두 개의 메소드 **'add'** 와 **'subtract'** 를 가지고 있습니다.
    - 각각의 메소드는 두 개의 정수를 받아 그 결과를 반환합니다.
        - 이렇게 메소드를 사용하면 코드를 효율적으로 관리할 수 있으며, 필요에 따라 재사용할 수 있습니다.

## 📝 정리.
메소드는 자바 프로그래밍에서 기능을 모듈화하고 코드의 재사용을 가능하게 하는 핵심 요소입니다.

---

## 4. 접근 제어자(Access Modifiers)
자바 프로그래밍에서 접근 제어자(Access Modifiers)는 클래스, 메서드, 변수 등과 같은 멤버들에 대한 접근 권한을 제어하는 키워드입니다.

이러한 접근 제어자를 사용함으로써 클래스의 캡슐화를 강화할 수 있으며, 객체의 데이터와 메서드를 외부에서 직접적으로 접근하거나 수정하는 것을 제한할 수 있습니다.

접근 제어자는 클래스의 멤버(변수, 메서드, 생성자 등)와 클래스 자체에 적용될 수 있습니다.

## 4.1 자바에서 사용하는 주요 접근 제어자.
- **1. public :** 어떤 클래스에서든 접근할 수 있도록 허용합니다.
    - public으로 선언된 멤버는 어디서든 접근이 가능합니다.

- **2. protected :** 같은 패키지 내의 클래스 또는 다른 패키지의 서브 클래스에서 접근할 수 있습니다.

- **3. default(package-private) :** 접근 제어자를 명시하지 않은 경우, 같은 패키지 내의 클래스들만 접근할 수 있습니다.
    - 이를 종종 package-private라고도 합니다.

- **4. private :** 해당 멤버를 선언한 클래스 내에서만 접근할 수 있습니다.
    - 외부 클래스에서는 접근할 수 없어, 클래스 내부 구현을 숨기는 데 유용합니다.

## 4.2 접근 제어자의 사용 예제.
```java
public class AccessExample {
    public int publicVar = 10; // 어디서든 접근 가능
    protexted int protectedVar = 20; // 같은 패키지 또는 상속받은 클래스에서 접근 가능
    int defaultVar = 30; // 같은 패키지 내에서만 접근 가능
    private int privateVar = 40; // 이 클래스 내에서만 접근 가능
    
    public void show() {
        System.out.println("publicVar: " + publicVar);
        System.out.println("protectedVar: " + protectedVar);
        System.out.println("defaultVar: " + defaultVar);
        System.out.println("privateVar: " + privateVar);
    }
}

public class Test {
    public static void main(String[] args) {
        AccessExample example = new AccessExample();
        System.out.println(example.publicVar); // 접근 가능
        System.out.println(example.protectedVar); // 다른 패키지에 있지 않은 이상 접근 가능
        System.out.println(example.defaultVar); // 같은 패키지에 있을 경우 접근 가능
        // System.out.println(example.privateVar); // 컴파일 에러 발생, 접근 불가능
        example.show(); // 모든 변수 출력 가능
    }
}
```

- 위 예제에서는 다양한 접근 제어자가 적용된 변수들을 선언하고, 이에 대한 접근 가능성을 보여줍니다.
    - **'publicVar'** 은 어디서든 접근할 수 있지만, **'privateVar'** 는 오직 선언된 클래스 내부에서만 접근할 수 있습니다.
    - **'protectedVar'** 과 **'defaultVar'** 는 좀 더 제한적인 접근을 허용합니다.

## 📝 정리.
이렇게 접근 제어자를 통해 자바에서는 데이터 보호 및 캡슐화, 객체의 정확한 사용을 보장하여 프로그램의 안정성과 유지보수성을 향상시킬 수 있습니다.

---

## 5. static 키워드.
자바 프로그래밍에서 **'static'** 키워드는 특정 필드나 메소드, 또는 중첩 클래스를 클래스의 인스턴스가 아닌 클래스 자체에 소속되게 합니다.

이를 사용함으로써 해당 멤버는 클래스의 모든 인스턴스에 걸쳐 공유되며, 인스턴스 생성 없이 클래스 이름을 통해 직접 접근할 수 있습니다.

## 5.1 static의 주요 사용 사례.
- **1. 정적 필드(Static Fields) :** 모든 인스턴스에 의해 공유되는 클래스 변수입니다.
    - 예를 들어, 회사의 모든 직원이 같은 회사 이름을 공유할 때 사용할 수 있습니다.

- **2. 정적 메소드(Static Methods) :** 인스턴스 변수에 접근할 필요 없이, 클래스 이름을 통해 직접 호출할 수 있는 메소드입니다.
    - 유틸리티 함수나 핼퍼 함수를 작성할 때 자주 사용됩니다.

- **3. 정적 초기화 블록(Static Initialization Blocks) :** 클래스가 처음 로딩될 때 한 번 실행되며, 정적 변수를 초기화하는 데 사용됩니다.

- **4. 정적 중첩 클래스(Static Nested Classes) :** 다른 클래스 내부에 위치하면서도 독립적으로 사용될 수 있는 클래스입니다.

## 5.2 static 키워드의 장점과 단점.
**장점.**
- **메모리 효율성 :** static 멤버는 클래스 로드 시 메모리에 한 번만 할당되고 모든 인스턴스가 공유하기 때문에 메모리 사용을 최소화할 수 있습니다.
- **편리성 :** 객체 생성 없이 바로 접근할 수 있어, 유틸리티 함수 같은 공통 기능 구현에 유용합니다.

**단점.**
- **과도한 사용은 객체지향 원칙에 어긋남 :** 객체 간의 결합도가 높아지고, 객체의 상태 관리가 어려워질 수 있습니다.
- **테스트가 어려워질 수 있슴 :** static 메소드는 오버라이드가 불가능하며, 상태를 공유하기 때문에 병렬 테스트 환경에서 문제를 일으킬 수 있습니다.

## 5.3 예제.
```java
public class Company {
    // 정적 필드
    public static String companyName = "Global Tech";
    
    // 정적 메소드
    public static void printCompanyName() {
        System.out.println("Company Name: " + companyName);
    }
}

public class Test {
    public static void main(String[] args) {
        // 객체 생성 없이 정적 메소드 호출
        Company.printCompanyName();
    }
}
```

- 이 예제에서 **'Company'** 클래스에는 정적 필드 **'companyName'** 과 정적 메소드 **'printCompanyName()'** 이 있습니다.
    - **'main'** 메소드에서는 **'Company'** 클래스의 객체를 생성하지 않고도 **'printCompanyName()'** 메소드를 호출하려 회사 이름을 출력합니다.

## 📝 정리.
정적 멤버는 클래스와 관련된, 변하지 않는 값이나, 모든 인스턴스가 공유해야 하는 정보를 관리할 때 유용하게 사용됩니다.

---

## 6. 생성자(Constructor)
자바 프로그래밍에서 생성자(Constructor)는 클래스로부터 객체가 생성될 때 호출되는 특별한 유형의 메서드입니다.
생성자의 주요 목적은 새로 생성된 객체를 초기화하는 것으로, 객체의 기본 상태를 설정하는 데 사용됩니다.

생성자는 메서드처럼 보일 수 있지만, 리턴 타입이 없고 클래스 이름과 동일한 이름을 가집니다.

## 6.1 생성자 특징.
- **1. 클래스 이름과 동일 :** 생성자의 이름은 항상 선언된 클래스의 이름과 동일해야 합니다.
- **2. 리턴 타입 없음 :** 생성자는 값을 반환하지 않으며, 리턴 타입도 선언하지 않습니다.
- **3. 자동 호출 :** 객체가 생성될 때 자동으로 호출됩니다.
    - 이는 객체의 필드를 초기화하거나, 객체 생성 시 실행해야 할 다른 시작 루틴을 실행하는 데 사용할 수 있습니다.
- **4. 오버로딩 가능 :** 하나의 클래스에 여러 생성자를 정의할 수 있습니다.
    - 이를 생성자 오버로딩이라고 하며, 파라미터의 수나 타입에 따라 다른 생성자를 호출할 수 있습니다.

## 6.2 생성자의 유형.
- **1. 기본 생성자(Default Constructor) :** 개발자가 명시적으로 생성자를 정의하지 않으면, 자바 컴파일러는 매개변수가 없는 기본 생성자를 제공합니다.
    - 이 기본 생성자는 객체의 필드를 기본값으로 초기화합니다.

- **2. 매개변수가 있는 생성자(Parameterized Constructor) :** 하나 이상의 매개변수를 받아 객체의 초기 상태를 세팅 할 수 있도록 해줍니다.

## 6.3 예제.
```java
public class Person {
    private String name;
    private int age;
    
    // 기본 생성자
    public Person() {
        this.name = "Unknown";
        this.age = 0;
    }
    
    // 매개변수가 있는 생성자
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void displayInfo() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class Test {
    public static void main(String[] args) {
        // 기본 생성자를 사용하여 객체 생성
        Person person1 = new Person();
        person1.displayInfo(); // 출력: Name: Unknown, Age: 0
        
        // 매개변수가 있는 생성자를 사용하여 객체 생성
        Person person2 = new Person("Jhon", 25);
        person2.displayInfo(); // 출력: Name: Jhon, Age: 25
    }
}
```
- 이 예제에서 **'Person'** 클래스는 두 가지 유형의 생성자를 가집니다.
    - 하나는 매개변수가 없어 기본값으로 객체를 초기화하고, 다른 하나는 이름과 나이를 받아 객체를 초기화합니다.

## 📝 정리.
생성자를 사용함으로써 클래스의 인스턴스가 유효한 상태로 시작될 수 있도록 보장하며, 필요한 초기 설정을 자동으로 수행할 수 있습니다.
이는 객체 지향 프로그래밍에서 객체의 무결성을 유지하는 중요한 방법입니다.

---

## 7. this 키워드와 this() 생성자 호출.
자바에서 **'this'** 키워드와 **'this()'** 생성자 호출은 객체 자신을 참조하고 객체의 생성자를 호출하는 데 사용되는 중요한 요소입니다.

이들은 객체 내부에서 사용되며, 클래스의 멤버(필드, 메서드, 생성자)와 관련된 동작을 명확히 하는 데 유용합니다.

## 7.1 this 키워드.
**'this'** 키워드는 현재 객체, 즉 메서드나 생성자를 호출하는 인스턴스를 참조하는 데 사용됩니다.

주로 다음과 같은 상황에서 사용됩니다.

- **1. 필드와 매개변수 이름이 같을 때 구분 :** 메서드나 생성자의 매개변수와 클래스의 필드 이름이 같을 때, 필드와 매개변수를 구분하기 위해 사용됩니다.

- **2. 메서드 체이닝 :** 객체의 메서드를 연속적으로 호출할 때 **'this'** 를 반환함으로써 메서드 체이닝을 구현할 수 있습니다.

- **3. 현재 객체를 다른 메서드에 전달 :** 현재 객체의 참조를 다른 메서드에 전달할 때 사용됩니다.

## 7.2 this() 생성자 호출.
**'this()'** 는 같은 클래스의 다른 생성자를 호출하는 데 사용됩니다.
주로 생성자 오버로딩이 있을 때, 중복 코드를 최소화하고, 하나의 생성자에서 다른 생성자를 호출하여 필드 초기화 등의 공통 작업을 중앙집중적으로 관리할 수 있게 해줍니다.

- **1. 생성자 오버로딩 처리 :** 클래스에 여러 생성자가 있을 때,  **'this()'** 를 사용하여 한 생성자에서 다른 생성자를 호출함으로써 공통 코드를 재사용할 수 있습니다.

- **2. 코드 간결성 유지 :** 필수적인 초기화 작업을 주 생성자에만 명시하고, 나머지 생성자는 이 주 생성자를 호출하게 함으로써 코드의 간결성을 유지합니다.

## 7.3 예제.
```java
public class Rectangle {
    private int width;
    private int height;
    
    // 주 생성자
    public Rectangle(int width, int height) {
        this.width = width; // 'this'로 필드와 매개변수 구분
        this.height = height;
    }
    
    // 부 생성자
    public Rectangle() {
        this(10, 10) // 'this()' 로 다른 생성자 호출
    }
    
    public void displaySize() {
        System.out.println("Width: " + this.width + ", Height: " + this.height);
    }
}

public class Test {
    public static void main(String[] args) {
        Rectangle rect1 = new Rectangle(30, 40);
        rect1.displaySize(); // 출력: Width: 30, Height: 40
        
        Rectangle rect2 = new Rectangle();
        rect2.displaySize(); // 출력: Width: 10, Height: 10
    }
}
```

- 이 예제에서 **'Rectangle'** 클래스는 두 개의 생성자를 가지고 있습니다.
    - 기본 생성자는 **'this()'** 를 사용하여 주 생성자를 호출하고, 주 생성자에서는 **'this'** 키워드를 사용하여 클래스 필드와 생성자 매개변수를 구분합니다.
        - 이렇게 **'this'** 와 **'this()'** 를 사용함으로써 코드의 중복을 줄이고, 초기화 로직을 하나의 생성자에 집중할 수 있습니다.

## 📝 정리.
이처럼 **'this'** 와 **'this()'** 는 자바에서 클래스의 인스턴스 자신을 참조하거나 클래스 내 다른 생성자를 호출하는 데 매우 유용한 도구입니다.
