---
title: "☕️[Java] Object와 OCP"
tags:
    - Java
    - Programming Language
date: "2024-03-31"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# Object와 OCP.

만약 `Object`가 없고, 또 `Object`가 제공하는 `toString()`이 없다면 서로 아무 관계가 없는 객체의 정보를 출력하기 어려울 것입니다.

여기서 아무 관계가 없다는 것은 공통의 부모가 없다는 뜻 입니다.

아마도 다음의 `BadObjectPrinter` 클래스와 같이 각각의 클래스마다 별도의 메서드를 작성해야 할 것입니다.

**BadObjectPrinter**
```java
public class BadObjectPrinter {
    public static void print(Car car) { // Car 전용 메서드
        String string = "객체 정보 출력: " + car.carInfo(); // carInfo() 메서드 만듬
        System.out.println(string);
    }
    
    public static void print(Dog dog) { // Dog 전용 메서드
        String string = "객체 정보 출력: " + dog.dogInfo(); // dogInfo() 메서드 만듬
        System.out.println(string);
    }
}
```

## 구체적인 것에 의존
`BadObjectPrinter`는 구체적인 타입인 `Car`, `Dog`를 사용합니다.
- 따라서 이후에 출력해야 할 구체적인 클래스가 10개로 늘어나면 구체적인 클래스에 맞추어 메서드도 10개로 계속 늘어나게 됩니다.
    - 이렇게 `BadObjectPrinter` 클래스가 구체적인 특정 클래스인 `Car`, `Dog`를 사용하는 것을 `BadObjectPrinter`는 `Car`, `Dog`에 의존한다고 표현합니다.

다행히도 자바에는 **객체의 정보를 사용할 때,** 다형적 참조 문제를 해결해줄 `Object` 클래스와 메서드 오버라이딩 문제를 해결해줄 `Object.toString()` 메서드가 있습니다.(물론 직접 `Object`와 비슷한 공통의 부모 클래스를 만들어서 해결할 수도 있습니다.)

## 추상적인 것에 의존
앞서 만든 `ObjectPrinter` 클래스는 `Car`, `Dog` 같은 구체적인 클래스를 사용하는 것이 아니라, 추상적인 `Object` 클래스를 사용합니다.
- 이렇게 `ObjectPrinter` 클래스가 `Object` 클래스를 사용하는 것을 `Object`에 클래스에 의존한다고 표현합니다.

```java
public class ObjectPrinter {
    public static void print(Object obj) {
        String string = "객체 정보 출력: " + obj.toString();
        System.out.println(string);
    }
}
```

`ObjectPrinter`는 구체적인 것에 의존하는 것이 아니라 추상적인 것에 의존합니다.

> **추상적 :** 여기서 말하는 추상적이라는 뜻은 단순히 추상 클래스나 인터페이스만 뜻하는 것은 아닙니다.
> `Animal`과 `Dog`, `Cat`의 관계를 떠올려봅시다.
> `Animal` 같은 부모 타입으로 올라갈 수록 개념은 더 추상적이게 되고, `Dog`, `Cat`과 같이 하위 타입으로
> 내려갈 수록 개념은 더 구체적이게 됩니다.

<img src = "https://github.com/devKobe24/images/blob/main/Object%E1%84%8B%E1%85%AAOCP%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B71.png?raw=true">

`ObjectPrinter`와 `Object`를 사용하는 구조는 다형성을 매우 잘 활용하고 있습니다.
- 다형성을 잘 활용한다는 것은 다형적 참조와 메서드 오버라이딩을 적절하게 사용한다는 뜻입니다.

`ObjectPrinter`의 `print()` 메서드와 전체 구조를 분석해봅시다.
- **다형적 참조 :** `print(Object obj)`, `Object` 타입을 매개변수로 사용해서 다형적 참조를 사용합니다. `Car`, `Dog` 인스턴스를 포함한 세상의 모든 객체 인스턴스를 인수로 받을 수 있습니다.
- **메서드 오버라이딩 :** `Object`는 모든 클래스의 부모입니다. 따라서 `Dog`, `Car`와 같은 구체적인 클래스는 `Object`가 가지고 있는 `toString()` 메서드를 오버라이딩 할 수 있습니다.
    - 따라서 `print(Object obj)` 메서드는 `Dog`, `Car`와 같은 구체적인 타입에 의존(사용)하지 않고, 추상적인 `Object` 타입에 의존하면서 런타임에 각 인스턴스의 `toString()`을 호출할 수 있습니다.

## OCP 원칙
OCP 원칙을 떠올려 봅시다.
- **Open :** 새로운 클래스를 추가하고, `toString()`을 오버라이딩해서 기능을 확장할 수 있습니다.
- **Closed :** 새로운 클래스를 추가해도 `Object`와 `toString()`을 사용하는 클라이언트 코드인 `ObjectPrinter`는 변경하지 않아도 됩니다.

다형적 참조, 메서드 오버라이딩, 그리고 클라이언트 코드가 구체적인 `Car`, `Dog`에 의존하는 것이 아니라 추상적인 `Object`에 의존하면서 OCP 원칙을 지킬 수 있었습니다.
- 덕분에 새로운 클래스를 추가하고 `toString()` 메서드를 새롭게 오버라이딩해서 기능을 확장할 수 있습니다.
    - 그리고 이러한 변화에도 불구하고 클라이언트 코드인 `ObjectPrinter`는 변경할 필요가 없습니다.

`ObjectPrinter`는 모든 타입의 부모인 `Object`를 사용하고, `Object`가 제공하는 `toString()` 메서드만 사용합니다.
- 따라서 `ObjectPrinter`를 사용하면 세상의 모든 객체의 정보(`toString()`)를 편리하게 출력할 수 있습니다.

## System.out.println()
지금까지 설명한 `ObjectPrinter.print()`는 사실 `System.out.println()`의 작동 방식을 설명하기 위해 만든 것입니다.
- `System.out.println()` 메서드도 `Object` 매개변수를 사용하고 내부에서 `toString()`을 호출합니다.
    - 따라서 `System.out.println()`를 사용하면 세상의 모든 객체의 정보(`toString()`)를 편리하게 출력할 수 있습니다.

<img src = "https://github.com/devKobe24/images/blob/main/Object%E1%84%8B%E1%85%AAOCP%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B72.png?raw=true">

**자바 언어는 객체지향 언어 답게 언어 스스로도 객체지향의 특징을 매우 잘 활용합니다.**
- 지금까지 배운 `toString()` 메서드와 같이, 자바 언어가 기본으로 제공하는 다양한 메서드들은 개발자가 필요에 따라 어버라이딩해서 사용할 수 있도록 설계되어 있습니다.

**참고 - 정적 의존관계 vs 동적 의존관계**
- 정적 의존관계는 컴파일 시간에 결정되며, 주로 클래스 간의 관계를 의미합니다. 앞서 보여준 클래스 의존 관계 그림이 바로 정적 의존관계입니다.
    - 쉽게 이야기해서 프로그램을 실행하지 않고, 클래스 내에서 사용하는 타입들만 보면 쉽게 의존관계를 파악할 수 있습니다.
- 동적 의존관계는 프로그램을 실행하는 런타임에 확인할 수 있는 의존관계입니다. 앞서 `ObjectPrinter.print(Object obj)`에 인자로 어떤 객체가 전달 될 지는 프로그램을 실행해봐야 알 수 있습니다.
    - 어떤 경우에는 `Car` 인스턴스가 넘어오고, 어떤 경우에는 `Dog` 인스턴스가 넘어옵니다. 이렇게 런타임에 어떤 인스턴스를 사용하는지를 나타내는 것이 동적 의존관계입니다.
- 참고로 단순히 의존관계 또는 어디에 의존한다고 하면 주로 정적 의존관계를 뜻합니다.
    - 예) `ObjectPrinter`는 `Object`에 의존합니다.
