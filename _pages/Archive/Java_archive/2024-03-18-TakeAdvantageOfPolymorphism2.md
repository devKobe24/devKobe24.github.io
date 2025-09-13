---
title: "☕️[Java] 다형성 활용2"
tags:
    - Java
    - Programming Language
date: "2024-03-18"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 다형성 활용2

이번에는 앞서 설명한 예제를 다형성을 사용하여 변경해보겠습니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC2%E1%84%8B%E1%85%A8%E1%84%8C%E1%85%A6.png?raw=true">

다형성을 사용하기 위해 여기서는 상속 관계를 사용합니다.
* `Animal`(동물) 이라는 부모 클래스를 만들고 `sound()` 메서드를 정의합니다.
    * 이 메서드는 자식 클래스에서 오버라이딩 할 목적으로 만들었습니다.
* `Dog`, `Cat`, `Caw`는 `Animal` 클래스를 상속받았습니다.
    * 그리고 각각 부모의 `sound()` 메서드를 오버라이딩 합니다.

기존 코드를 유지하기 위해 새로운 패키지를 만들고 새로 코드를 작성해보겠습니다.
**"주의! 패키지 이름에 주의합시다 `import`를 사용해서 다른 패키지에 있는 같은 이름의 클래스를 사용하면 안됩니다."**

```java
package poly.ex2;

public class Dog extends Animal {
  @Override
  public void sound() {
    System.out.println("멍멍");
  }
}
```

```java
package poly.ex2;

public class Cat extends Animal {
  @Override
  public void sound() {
    System.out.println("야옹");
  }
}
```

```java
package poly.ex2;

public class Caw extends Animal {
  @Override
  public void sound() {
    System.out.println("음메");
  }
}
```

```java
package poly.ex2;

public class Animal {
  public void sound() {
    System.out.println("동물 울음 소리");
  }
}
```

```java
package poly.ex2;

public class AnimalPolyMain1 {

  public static void main(String[] args) {
    Dog dog = new Dog();
    Cat cat = new Cat();
    Caw caw = new Caw();
    soundAnimal(dog);
    soundAnimal(cat);
    soundAnimal(caw);
  }

  private static void soundAnimal(Animal animal) {
    System.out.println("동물 소리 테스트 시작");
    animal.sound();
    System.out.println("동물 소리 테스트 종료");
  }
}
```

**실행 결과**
```java
동물 소리 테스트 시작
멍멍
동물 소리 테스트 종료

동물 소리 테스트 시작
야옹
동물 소리 테스트 종료

동물 소리 테스트 시작
음메
동물 소리 테스트 종료
```

실행 결과는 기존 코드와 같습니다.

**코드를 분석해봅시다.**
* `soundAnimal(dog)`을 호출하면
    * `soundAnimal(Animal animla)`에 `Dog` 인스턴스가 전달됩니다.
        * `Animal animal = dog`로 이해하면 됩니다. 부모는 자식을 담을 수 있습니다. `Animal`은 `Dog`의 부모입니다.
* 메서드 안에서 `animal.sound()` 메서드를 호출합니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC2%E1%84%8F%E1%85%A9%E1%84%83%E1%85%B3%E1%84%87%E1%85%AE%E1%86%AB%E1%84%89%E1%85%A5%E1%86%A8.png?raw=true">

* `animal` 변수의 타입은 `Animal`이므로 `Dog` 인스턴스에 있는 `Animal` 클래스 부분을 찾아서 `sound()` 메서드를 실행합니다.
    * 그런데 하위 클래스인 `Dog`에서 `sound()` 메서드를 오버라이딩 했습니다.
        * 따라서 오버라이딩한 메서드가 우선권을 가집니다.
* `Dog` 클래스에 있는 `sound()` 메서드가 호출되므로 "멍멍"이 출력됩니다.

이 코드의 핵심은 `Animal animal` 부분입니다.
* **다형적 참조** 덕분에 `animal` 변수는 자식인 `Dog`, `Cat`, `Caw`의 인스턴스를 참조할 수 있습니다.(부모는 자식을 담을 수 있습니다.)
* **메서드 오버라이딩** 덕분에 `animal.sound()`를 호출해도 `Dog.sound()`, `Cat.sound()`, `Caw.sound()`와 같이 각 인스턴스의 메서드를 호출할 수 있습니다.
    * 만약 자바에 메서드 오버라이딩이 없었다면 모두 `Animal`의 `sound()`가 호출되었을 것입니다.

다형성 덕분에 이후에 새로운 동물을 추가해도 다음 코드를 그대로 재사용 할 수 있습니다.
* 물론 다형성을 사용하기 위해 새로운 동물은 `Animal`을 상속 받아야합니다.
```java
private static void soundAnimal(Animal animal) {
    System.out.println("동물 소리 테스트 시작");
    animal.sound();
    System.out.println("동물 소리 테스트 종료");
}
```
