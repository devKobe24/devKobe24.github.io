---
title: "☕️[Java] 클래스와 객체(2)"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-05-08"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 1️⃣ 클래스와 객체(2)

## 1. 오버로딩(Overloading).
자바 프로그래밍에서 오버로딩(Overloading)은 같은 클래스 내에서 메소드 이름이 같지만 매개변수의 타입이나 개수가 다른 여러 메소드를 정의하는 것을 의미합니다.

오버로딩을 사용하면 같은 기능을 하는 메소드라도 다양한 입력에 대응하여 유연하게 메소드를 호출할 수 있습니다.

오버로딩은 메소드만 가능하며, 생성자에도 적용될 수 있습니다.

## 1.2. 오버로딩의 규칙.
- **1. 메소드 이름이 같아야 합니다 :** 오버로딩된 메소드들은 같은 이름을 공유합니다.
- **2. 매개변수 목록이 달라야 합니다 :** 매개변수의 개수나 타입, 혹은 그 순서가 달라야 합니다. 매개변수의 차이를 통해 자바 컴파일러는 호출할 적절한 메소드를 결정합니다.
- **3. 반환 타입은 오버로딩을 구분하는 데 사용되지 않습니다 :** 오버로딩된 메소드는 반환 타입이 다르더라도, 이는 오버로딩을 구분하는 데 사용되지 않습니다.
    - 즉, 반환 타입만 다른 메소드는 오버로딩이 아닙니다.
- **4. 접근 제어자와 예외는 오버로딩을 구분하는 데 사용되지 않습니다 :** 이 역시 메소드를 구별하는 데 사용되지 않습니다.

## 1.3. 오버로딩의 예.
```java
public class Print {
    // 오버로딩 예제: 같은 메소드 이름, 다른 매개변수 타입
    public void display(int a) {
        System.out.println("Integer: " + a);
    }
    
    public void display(String a) {
        System.out.println("String: " + a);
    }
    
    // 오버로딩 예제: 같은 메소드 이름, 다른 매개변수 개수
    public void display(int a, int b) {
        System.out.println("Two integers: " + a + ", " + b);
    }
    
    // 오버로딩 예제: 같은 메소드 이름, 매개변수의 순서가 다름
    public void display(String a, int b) {
        System.out.println("String and integer: " + a + ", " + b);
    }
}

public class Test {
    public static void main(String[] args) {
        Print prt = new Print();
        prt.display(1); // 출력: Integer: 1
        prt.display("Hello"); // 출력: String: Hello
        prt.display(1, 2); // 출력: Two integers: 1, 2
        prt.display("Age", 30); // 출력: String and integer: Age, 30
    }
}
```

- 이 예제에서 **'Print'** 클래스는 **'display'** 라는 메소드를 여러 번 오버로딩했습니다.
    - 매개변수의 타입, 개수, 순서에 따라 다른 메소드가 호출됩니다.
        - 이를 통해 다양한 타입과 개수의 입력을 유연하게 처리할 수 있습니다.

## 📝 정리.
오버로딩을 통해 프로그램의 가독성을 향상시키고, 유사한 기능을 하는 메소드들을 하나의 이름으로 그룹화함으로써 프로그램을 더욱 직관적으로 만들 수 있습니다.

이러한 방식은 프로그래밍의 복잡성을 줄이고, 코드의 유지보수를 용이하게 합니다.

---

## 2. 접근제어자(Access Modifiers).
자바 프로그래밍에서 접근 재어자(Access Modifiers)는 클래스, 메서드, 변수 등과 같은 멤버들에 대한 접근 권한을 제어하는 키워드입니다.

이러한 접근 제어자를 사용함으로써 클래스의 캡슐화를 강화할 수 있으며, 객체의 데이터와 메서드를 외부에서 직접접으로 접근하거나 수정하는 것을 제한할 수 있습니다.

접근 제어자는 클래스의 멤버(변수, 메서드, 생성자 등)와 클래스 자체에 적용될 수 있습니다.

## 2.1 자바에서 사용하는 주요 접근 제어자
<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%80%E1%85%B3%E1%86%AB%E1%84%8C%E1%85%A6%E1%84%8B%E1%85%A5%E1%84%8C%E1%85%A1_public.jpeg?raw=true">
- **1. public :** 어떤 클래스에서든 접근할 수 있도록 허용합니다.
    - public으로 선언됩 멤버는 어디서든 접근이 가능합니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%80%E1%85%B3%E1%86%AB%E1%84%8C%E1%85%A6%E1%84%8B%E1%85%A5%E1%84%8C%E1%85%A1_protected.jpeg?raw=true">
- **2. protected :** 같은 패키지 내의 클래스 또는 다른 패키지의 서브 클래스에서 접근할 수 있습니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%80%E1%85%B3%E1%86%AB%E1%84%8C%E1%85%A6%E1%84%8B%E1%85%A5%E1%84%8C%E1%85%A1_default.jpeg?raw=true">
- **3. default(package-private) :** 접근 제어자를 명시하지 않은 경우, 같은 패키지 냐의 클래스들만 접근할 수 있습니다. 이를 종종 package-private라고도 합니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%80%E1%85%B3%E1%86%AB%E1%84%8C%E1%85%A6%E1%84%8B%E1%85%A5%E1%84%8C%E1%85%A1_private.jpeg?raw=true">
- **private :** 해당 멤보를 선언한 클래스 내에서만 접근할 수 있습니다. 외부 클래스에서는 접근할 수 없어, 클래스 내부 구현을 숨기는 데 유용합니다.

## 2.2 접근 제어자의 사용 예제.

```java
public class AccessExample {
    public int publicVar = 10; // 어디서든 접근 가능
    protected int protectedVar = 20; // 같은 패키지 또는 상속 받은 클래스에서 접근 가능
    int defaultVar = 30; // 같은 패키지 내에서만 접근 가능
    private int privateVar = 40; // 이 클래스 내에서만 접근 가능
    
    public void show() {
        System.out.println("publicVar: " + publicVar);
        System.out.println("protectedVar: " + pretectedVar);
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
-  위 예제에서는 다양한 접근 제어자가 적용된 변수들을 선언하고, 이에 대한 접근 가능성을 보여줍니다.
-  **'publicVar'** 은 어디서든 접근할 수 있지만, **'privateVar'** 는 오직 선언된 클래스 내부에서만 접근할 수 있습니다.
-  **'protectedVar'** 과 **'defaultVar'** 는 좀 더 제한적인 접근을 허용합니다.

## 📝 정리.
이렇게 접근 제어자를 통해 자바에서는 데이터 보호 및 캡슐화, 객체의 정확한 사용을 보장하여 프로그램의 안정성과 유지보수성을 향상시킬 수 있습니다.

---

## 3. static 키워드.
자바 프로그래밍에서 **'static'** 키워드는 클래스의 멤버(필드, 메서드, 블록 또는 내부 클래스)를 클래스 레벨에 소속 시키는 역할을 합니다.

이는 특정 인스턴스에 속하기보다는 클래스 자체에 속한다는 의미입니다.

**'static'** 멤버는 클래스의 모든 인스턴스에 의해 공유되며, 클래스가 메모리에 로드될 때 생성되고, 클래스가 언로드될 때 소멸됩니다.

## 3.1 static의 특징.
- **1. 클래스 레벨에서 공유 :** **'static'** 필드는 클래스의 모든 인스턴스 간에 공유됩니다.
    - 이는 특정 데이터를 모든 객체가 공유해야 할 필요가 있을 때 유용합니다.
- **2. 인스턴스 생성 없이 접근 기는 :** **'static'** 메서드나 필드는 객체의 인스턴스를 생성하지 않고도 클래스 이름을 통해 직접 접근할 수 있습니다.
- **3. 정적 초기화 블록 :** **'static'** 키워드를 사용한 블록(정적 블록)은 클래스가 처음 메모리에 로그 될 때 단 한 번 실행됩니다.
    - 이는 **'static'** 필드의 초기화에 사용할 수 있습니다.

## 3.2 static 필드와 메서드 사용 예.
```java
public class Calculator {
    // 정적 필드
    public static int calculatorCount = 0;
    
    // 정적 블록
    static {
        System.out.println("Calculator 클래스 로딩!");
    }
    
    // 생성자
    public Calculator() {
        calculatorCount++; // 생성될 때마다 계산기의 수를 증가
    }
    
    // 정적 메서드
    public static int add(int a, int b) {
        return a + b;
    }
}

public class Test {
    public static void main(String[] args) {
        Calculator c1 = new Calculator();
        Calculator c2 = new Calculator();
        
        System.out.println("Created Calculators: " + Calculator.calculatorCount); // 2 출력
        System.out.println("Sum: " + Calculator.add(5,3)); // 8 출력
    }
}
```

- 이 예제에서는 **'Calculator'** 클래스의 인스턴스 생성 횟수를 추적하는 **'static'** 필드 **'calculatorCount'** 와 정적 메서드 **'add'** 를 사용합니다.
    - **'calculatorCount'** 는 **'Calculator'** 의 모든 인스턴스에 의해 공유되며, **'add'** 메서드는 인스턴스를 생성하지 않고도 호출할 수 있습니다.

## 3.3 static 사용 시 주의점
- **'static'** 메서드 내에서는 인스턴스 변수나 메서드를 직접 사용할 수 없습니다.
- **'static'** 은 남용하면 객체지향의 원칙을 해칠 수 있습니다.
    - 예를 들어, 객체 간의 상태 공유가 과도하게 이루어져 객체 간의 결합도가 높아질 수 있습니다.
- **'static'** 변수는 프로그램의 실행이 끝날 때까지 메모리에 남아 있으므로 메모리 사용에 주의해야 합니다.

## 3.4 static 메소드와 static 변수와의 관계성.
자바 프로그래밍에서 **'static'** 메소드와 **'static'** 변수는 두 가지 공통점을 가지고 있습니다.

둘 다 클래스 레벨에서 정의되며, 클래스의 모든 인스턴스 간에 공유됩니다.

이런 공통점 때문에, **'static'** 메소드는 **'static'** 변수에 직접 접근할 수 있지만, 일반 인스턴스 변수에는 접근할 수 없습니다.

## 3.5 static 변수.
**'static'** 변수는 클래스 변수라고도 하며, 특정 클래스의 모든 인스턴스에 의해 공유됩니다.
이 변수는 클래스가 메모리에 로드될 때 생성되고, 클래스가 언로드될 때까지 메모리에 존재합니다.
**'static'** 변수는 특히 클래스의 인스턴스들이 공통적으로 사용해야 하는 데이터를 저장하는데 유용합니다.
예를 들어, 모든 계산기 객체가 공유해야 하는 **'calculatorCount'** 와 같은 경우에 사용됩니다.

## 3.6 static 메소드
**'static'** 메소드 역시 클래스 레벨에 정의되며, 이 메소드는 인스턴스 생성 없이 클래스 이름을 통해 직접 호출할 수 있습니다.

**'static'** 메소드는 인스턴스 필드나 메소드에 접근할 수 없습니다.

그 이유는 **'static'** 메소드가 호출될 때 해당 클래스의 인스턴스가 존재하지 않을 수 있기 때문입니다.
따라서 **'static'** 메소드는 오로지 **'static'** 변수나 다른 **'static'** 메소드에만 접근할 수 있습니다.

## 3.7 두 요소의 상호작용.
**'static'** 메소드에는 **'static'** 변수에 자유롭게 접근하고 수정할 수 있습니다.
이는 **'static'** 변수가 클래스에 속하고 메소드도 클래스 레벨에서 실행되기 때문입니다.
예를 들어, 어떤 클래스의 모든 인스턴스가 사용할 설정 값을 **'static'** 변수에 저장하고, 이 값을 설정하거나 조회하는 **'static'** 메소드를 제공할 수 있습니다.

## 3.8 예제
```java
public class Counter {
    public static int count = 0; // 'static' 변수
    
    public static void increment() { // 'static' 메소드
        count++; // 'static' 변수에 접근하여 값을 증가
    }
    
    public static void displayCount() {
        System.out.println("Count: " + count); // 'static' 변수의 현재 값을 출력
    }
}

public class Test {
    public static void main(String[] args) {
        Counter.increment();
        Counter.increment();
        Countet.displayCount(); // 출력: Count: 2
    }
}
```

- 이 예제에서 **'Counter'** 클래스는 **'static'** 변수 **'count'** 를 가지고 있으며, **increment** 메소드를 통해 이 변수의 값을 증가시키고, **'displayCount'** 메소드를 통해 값을 출력합니다.

- 모든 **'Counter'** 객체가 **'count'** 값을 공유하며, **'static'** 메소드를 통해 이 값을 조작할 수 있습니다.



## 📝 정리.
**'static'** 은 전역 변수나 전역 메서드와 유사한 효과를 제공하지만, 자바의 객체지향적 특성과 일관성을 유지하기 위해 적절히 사용되어야 합니다.

**'static'** 메소드와 **'static'** 변수는 클래스 레벨에서 관리되어 클래스의 모든 인스턴스에 의해 공유되는 특성을 가지고 있습니다.

이를 통해 클래스 전체에 영향을 미치는 작업을 수행할 수 있습니다.
