---
title: "☕️[Java] 람다식은 하나만!"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-05-13"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 람다식은 하나만!😆
자바에서는 **"하나의 추상 메소드를 갖는 인터페에스에 대해서만 람다식을 직접 사용할 수 있습니다."**
이를 함수형 인터페이스라고 부르며, 람다식은 이런 함수형 인터페이스의 구현을 간단히 할 수 있는 방법을 제공합니다.

하지만 아래의 코드와 같이 인터페이스 내에 두 개의 추상 메서드 (**'plus', 'minus'**)가 있기 때문에, 이 인터페이스를 람다식으로 직접 구현하는 것은 불가능합니다.

```java
interface Carculator {
    public abstract int plus(int x, int y);
    public abstract int minus(int x, int y);
}
```

람다식을 사용하려면 함수형 인터페이스가 필요하므로, 두 메소드 각각을 위한 두 개의 별도의 인터페이스를 정의하거나 기존 인터페이스 중 하나를 수정해야 합니다.

아래의 코드는 이를 위해 각 메소드를 분리하여 두 개의 함수형 인터페이스를 만든 예시입니다.
```java
interface Calculator {
  public abstract int operation(int x, int y);
}
public class Main {

  public static void main(String[] args) {
    Calculator plus = (x, y) -> { return x + y; };
    System.out.println(plus.operation(10,2)); // 12
    Calculator minus = (x, y) -> { return x - y; };
    System.out.println(minus.operation(10,2)); // 8
  }
}
```

위 코드는 각 연산을 람다식으로 간단히 구현하고 있습니다.
만약 원래의 **'Carculator'** 인터페이스를 유지고하고 싶다면 이를 직접적으로 람다식으로 구현할 수는 없으며, 대신 익명 클래스나 정규 클래스를 사용해야 합니다.

아래의 코드는 익명 클래스를 사용하는 방법을 보여줍니다.
```java
Calculator calclator = new Calculator() {
    @Override
    public int plus(int x, int y) {
        return x + y;
    }
    
    @Override
    public int minus(int x, int y) {
        return x - y;
    }
}
```
