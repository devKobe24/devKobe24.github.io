---
title: "☕️[Java] 다형성 활용3"
tags:
    - Java
    - Programming Language
date: "2024-03-19"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 다형성 활용 3

이번에는 배열과 for문을 사용해서 중복을 제거해보겠습니다.

```java
package poly.ex2;

public class AnimalPolyMain2 {

  public static void main(String[] args) {
    Dog dog = new Dog();
    Cat cat = new Cat();
    Caw caw = new Caw();
    Animal[] animalArr = {dog, cat, caw};

    // 변하지 않는 부분
    for (Animal animal : animalArr) {
      System.out.println("동물 소리 테스트 시작");
      animal.sound();
      System.out.println("동물 소리 테스트 종료");
    }
  }
}
```

**실행 결과**
```
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

* 배열은 같은 타입의 데이터를 나열할 수 있습니다.
    * `Dog`, `Cat`, `Caw`는 모두 `Animal`의 자식이므로 `Animal` 타입입니다.

`Animal` 타입의 배열을 만들고 다형적 참조를 사용하면 됩니다.
```java
// 둘은 같은 코드입니다.
Animal[] animalArr = new Anima[]{dog, cat, caw};
Animal[] animalArr = {dog, cat, caw};
```
* 다형적 참조 덕분에 `Dog`, `Cat`, `Caw`의 부모 타입인 `Animal` 타입으로 배열을 만들고, 각각을 배열에 포함했습니다.

이제 배열을 for문을 사용해서 반복하면 됩니다.
```java
// 변하지 않는 부분
for (Animal animal: animalArr) {
    System.out.println("동물 소리 테스트 시작");
    animal.sound();
    System.out.println("동물 소리 테스트 종료");
}
```

* `animal.sound()`를 호출하지만 배열에는 `Dog`, `Cat`, `Caw`의 인스턴스가 들어있습니다.
    * 메서드 오버라이딩에 의해 각 인스턴스의 오버라이딩 된 `sound()` 메서드가 호출됩니다.

## 조금 더 개선

```java
package poly.ex2;

public class AnimalPolyMain3 {

  public static void main(String[] args) {
    Animal[] animalArr = {new Dog(), new Cat(), new Cat()};
    for (Animal animal : animalArr) {
      soundAnimal(animal);
    }
  }

  // 변하지 않는 부분
  private static void soundAnimal(Animal animal) {
    System.out.println("동물 소리 테스트 시작");
    animal.sound();
    System.out.println("동물 소리 테스트 종료");
  }
}
```

* `Animal[] animalArr`를 통해서 배열을 사용합니다.
* `soundAnimal(Animal animal)`
    * 하나의 동물을 받아서 로직을 처리합니다.

* 새로운 동물이 추가되어도 `soundAnimal(...)` 메서드는 코드 변경 없이 유지할 수 있습니다.
    * 이렇게 할 수 있는 이유는 이 메서드는 `Dog`, `Cat`, `Caw` 같은 구체적인 클래스를 참조하는 것이 아니라 `Animal`이라는 추상적인 부모를 참조하기 때문입니다.
        * 따라서 `Animal`을 상속 받은 새로운 동물이 추가되어도 이 메서드의 코드는 병경 없이 유지할 수 있습니다.

* 여기서 잘 보면 새로운 동물이 추가되었을 때 코드가 변하는 부분과 변하지 않는 부분이 있습니다.
    * `main()`은 코드가 변하는 부분입니다.
        * 새로운 동물을 생성하고 필요한 메서드를 호출합니다.
            * `soundAnimal(...)`는 코드가 변하지 않는 부분입니다.

**"새로운 기능이 추가되었을 때 변하는 부분을 최소화 하는 것이 잘 작성된 코드입니다."**
* 이렇게 하기 위해서는 코드에서 변하는 부분과 변하지 않는 부분을 명확하게 구분하는 것이 좋습니다.

### 남은 문제
지금까지 설명한 코드에는 사실 2가지 문제가 있습니다.
* `Animal` 클래스를 생성할 수 있는 문제
* `Animal` 클래스를 상속 받는 곳에서 `sound()` 메서드 오버라이딩을 하지 않을 가능성.

### Animal 클래스를 생성할 수 있는 문제

`Animal` 클래스는 동물이라는 클래스입니다.
이 클래스를 다음과 같이 직접 생성해서 사용할 일이 있을까요?

```java
Animal animal = new Animal();
```

* 개, 고양이, 소가 실제 존재하는 것은 당연하지만, 동물이라는 추상적인 개념이 실제로 존재하는 것은 이상합니다.
    * 이 클래스는 다형성을 위해서 필요한 것이지 직접 인스턴스를 생성해서 사용할 일은 없습니다.
    * 하지만 `Animal`도 클래스이기 때문에 인스턴스를 생성하고 사용하는데 아무런 제약이 없습니다.
    * 누군가 실수로 `new Animal()`을 사용해서 `Animal`의 인스턴스를 생성할 수 있다는 것 입니다.
    * 이렇게 생성된 인스턴스는 작동은 하지만 제대로된 기능을 수행하지는 않습니다.

### Animal 클래스를 상속 받는 곳에서 `sound()` 메서드 오버라이딩을 하지 않을 가능성.
예를들어서 `Animal`을 상속 받은 `Pig` 클래스를 만든다고 가정해봅시다.
* 우리가 기대하는 것은 `Pig` 클래스가 `sound()` 메서드를 오버라이딩 해서 "꿀꿀"이라는 소리가 나도록 하는 것입니다.
    * 그런데 개발자가 실수로 `sound()` 메서드를 오버라이딩 하는 것을 빠트릴 수 있습니다.
    * 이렇게 되면 부모의 기능을 상속받습니다.
        * 따라서 코드상 아무런 문제가 발생하지 않습니다.
            * 물론 프로그램을 실행하면 기대와 다르게 "꿀꿀"이 아니라 부모 클래스에 있는 `Animal.sound()`가 호출될 것입니다.

좋은 프로그램은 제약이 있는 프로그램입니다.

추상 클래스와 추상 메서드를 사용하면 이런 문제를 한번에 해결할 수 있습니다.

