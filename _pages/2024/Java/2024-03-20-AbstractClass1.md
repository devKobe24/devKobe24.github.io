---
title: "☕️[Java] 추상 클래스 1"
tags:
    - Java
    - Programming Language
date: "2024-03-20"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 추상 클래스 1

## 추상 클래스

동물(`Animal`)과 같이 부모 클래스는 제공하지만, 실제 생성되면 안되는 클래스를 추상 클래스라 합니다.
* 추상 클래스는 이름 그대로 추상적인 개념을 제공하는 클래스입니다.
    * 따라서 실체인 인스턴스가 존재하지 않습니다.
    * 대신에 상속을 목적으로 사용되고, 부모 클래스 역할을 담당합니다.

```java
abstract class AbstractAnimal {...}
```
* 추상 클래스는 클래스를 선언할 때 앞에 "추상"이라는 의미의 `abstract` 키워드를 붙여주면 됩니다.
* 추상 클래스는 기존 클래스와 완전히 같습니다.
    * 다만 `new AbstractAnimal()`와 같이 직접 인스턴스를 생성하지 못하는 제약이 추가된 것입니다.

## 추상 메서드

부모 클래스를 상속 받은 자식 클래스가 반드시 오버라이딩 해야 하는 메서드를 부모 클래스에 정의할 수 있습니다.
* 이것을 추상 메서드라 합니다.
* 추상 메서드는 이름 그대로 추상적인 개념을 제공하는 메서드입니다.
    * 따라서 실체가 존재하지 않고, 메서드 바디가 없습니다.

```java
public abstract void sound();
```
* 추상 메서드는 선언할 때 메서드 앞에 추상이라는 의미의 `abstract` 키워드를 붙여주면 됩니다.
* **추상 메서드가 하나라도 있는 클래스는 추상 클래스로 선언해야 합니다.**
    * 그렇지 않으면 컴파일 오류가 발생합니다.
    * 추상 메서드는 메서드 바디가 없습니다.
        * 따라서 작동하지 않는 메서드를 가진 불완전한 클래스로 볼 수 있습니다.
            * 따라서 직접 생성하지 못하도록 추상 클래스로 선언해야 합니다.
* **추상 메서드는 상속 받는 자식 클래스가 반드시 오버라이딩 해서 사용해야 합니다.**
    * 그렇지 않으면 컴파일 오류가 발생합니다.
    * 추상 메서드는 자식 클래스가 반드시 오버라이딩 해야 하기 때문에 메서드 바디 부분이 없습니다. 바디 부분을 만들면 컴파일 오류가 발생합니다.
    * 오버라이딩 하지 않으면 자식도 추상 클래스가 되어야 합니다.
* 추상 메서드는 기존 메서드와 완전히 같습니다.
    * 다만 메서드 바디가 없고, 자식 클래스가 해당 메서드를 반드시 오버라이딩 해야 한다는 제약이 추가된 것입니다.

이제 추상 클래스와 추상 메서드를 사용해서 예제를 만들어봅시다.

### 예제 3
<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8E%E1%85%AE%E1%84%89%E1%85%A1%E1%86%BC%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%84%89%E1%85%B3%E1%84%8B%E1%85%A8%E1%84%8C%E1%85%A63.png?raw=true">

```java
package poly.ex3;

public abstract class AbstractAnimal {
  public abstract void sound();

  public void move() {
    System.out.println("동물이 움직입니다.");
  }
}
```

* `AbstractAnimal`은 `abstract`가 붙은 추상 클래스입니다.
    * 이 클래스는 직접 인스턴스를 생성할 수 없습니다.
* `sound()`는 `abstract`가 붙은 추상 메서드입니다.
    * 이 메서드는 자식이 반드시 오버라이딩 해야 합니다.

**"이 클래스는 `move()`라는 메서드를 가지고 있는데, 이 메서드는 추상 메서드가 아닙니다."**
* 따라서 자식 클래스가 오버라이딩 하지 않아도 됩니다.

> 참고로 추상 클래스라고 `AbstractAnimal` 처럼 클래스 이름 앞에 꼭 `Abstract`를 써야하는 것은 아닙니다.
> 그냥 `Animal` 이라는 클래스 이름으로도 충분합니다.
> 여기에서는 예제 코드를 다른 예제 코드와 구분해서 설명하기 위해 앞에 `Abstract`를 붙였습니다.

```java
package poly.ex3;

public abstract class AbstractAnimal {
  public abstract void sound();

  public void move() {
    System.out.println("동물이 움직입니다.");
  }
}
```

```java
package poly.ex3;

public class Cat extends AbstractAnimal {
  @Override
  public void sound() {
    System.out.println("야옹");
  }
}
```

```java
package poly.ex3;

public class Caw extends AbstractAnimal {
  @Override
  public void sound() {
    System.out.println("음매");
  }
}
```

```java
package poly.ex3;

public class Dog extends AbstractAnimal {

  @Override
  public void sound() {
    System.out.println("멍멍");
  }
}
```

```java
package poly.ex3;

public class AbstractMain {

  public static void main(String[] args) {
    // 추상클래스 생성 불가
    //AbstractAnimal animal = new AbstractAnimal();

    Dog dog = new Dog();
    Cat cat = new Cat();
    Caw caw = new Caw();

    cat.sound();
    cat.move();

    soundAnimal(dog);
    soundAnimal(cat);
    soundAnimal(caw);
  }

  // 변하지 않는 부분
  private static void soundAnimal(AbstractAnimal animal) {
    System.out.println("동물 소리 테스트 시작");
    animal.sound();
    System.out.println("동물 소리 테스트 종료");
  }
}
```

**실행 결과**
```
야옹
동물이 움직입니다.

동물 소리 테스트 시작
멍멍
동물 소리 테스트 종료

동물 소리 테스트 시작
야옹
동물 소리 테스트 종료

동물 소리 테스트 시작
음매
동물 소리 테스트 종료
```

추상 클래스는 생성이 불가능합니다.
* 다음 코드의 주석을 풀고 실행하면 컴파일 오류가 발생합니다.
```java
    // 추상클래스 생성 불가
    //AbstractAnimal animal = new AbstractAnimal();
```

**컴파일 오류 - 인스턴스 생성**
```
java: poly.ex3.AbstractAnimal is abstract; cannot be instantiated
```
* `AbstractAnimal`가 추상이어서 인스턴스 생성이 불가능하다는 뜻입니다.

추상 메서드는 반드시 오버라이딩 해야 합니다.
* 만약 자식에서 오버라이딩 메서드를 만들지 않으면 다음과 같이 컴파일 오류가 발생합니다.

`Dog`의 `sound()` 메서드를 잠시 주석처리해봅시다.

```java
package poly.ex3;

public class Dog extends AbstractAnimal {
/*
  @Override
  public void sound() {
    System.out.println("멍멍");
  }
*/
}
```

**컴파일 오류 - 오버라이딩 X**
```java
java: poly.ex3.Dog is not abstract and does not override abstract method sound() in poly.ex3.AbstractAnimal
```
* `Dog`는 추상클래스가 아닌데 `sound()`가 오버라이딩 되지 않았다는 뜻 입니다.

지금까지 설명한 제약을 제외하고 나머지는 모두 일반적인 클래스와 동일합니다.
* 추상 클래스는 제약이 추가된 클래스일 뿐입니다.
    * 메모리 구조, 실행 결과 모두 동일합니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8E%E1%85%AE%E1%84%89%E1%85%A1%E1%86%BC%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%84%89%E1%85%B3Dog.png?raw=true">

**정리**
* 추상 클래스 덕분에 실수로 `Animal` 인스턴스를 생성할 문제를 근본적으로 방지해줍니다.
* 추상 메서드 덕분에 동물의 자식 클래스를 만들때 실수로 `sound()` 오버라이딩 하지 않을 문제를 근본적으로 방지해줍니다.
