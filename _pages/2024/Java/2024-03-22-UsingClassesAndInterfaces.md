---
title: "☕️[Java] 클래스와 인터페이스 활용"
tags:
    - Java
    - Programming Language
date: "2024-03-22"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 클래스와 인터페이스 활용

이번에는 클래스 상속과 인터페이스 구현을 함께 사용하는 예를 알아보겠습니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%84%89%E1%85%B3%E1%84%89%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A9%E1%86%A8%E1%84%80%E1%85%AA%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%90%E1%85%A5%E1%84%91%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%E1%84%8B%E1%85%B3%E1%86%AF%E1%84%92%E1%85%A1%E1%86%B7%E1%84%81%E1%85%A6%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC.png?raw=true">

* `AbstractAnimal`은 추상 클래스입니다.
    * `sound()` : 동물의 소리를 내기 위한 `sound()` 추상 메서드를 제공합니다.
    * `move()` : 동물의 이동을 표현하기 위한 메서드 입니다. 이 메서드는 추상 메서드가 아닙니다. 상속을 목적으로 사용됩니다.
* `Fly`는 인터페이스 입니다. 나는 동물은 이 인터페이스를 구현할 수 있습니다.
    * `Bird`, `Chicken`은 날 수 있는 동물입니다. `fly()` 메서드를 구현해야 합니다.

## 예제 6
```java
package poly.ex6;

public class SoundFlyMain {

  public static void main(String[] args) {
    Dog dog = new Dog();
    Bird bird = new Bird();
    Chicken chicken = new Chicken();

    soundAnimal(dog);
    soundAnimal(bird);
    soundAnimal(chicken);

    flyAnimal(bird);
    flyAnimal(chicken);
  }

  //AbstractAnimal 사용 가능
  private static void soundAnimal(AbstractAnimal animal) {
    System.out.println("동물 소리 테스트 시작");
    animal.sound();
    System.out.println("동물 소리 테스트 종료");
  }

  //Fly 인터페이스가 있으면 사용 가능
  private static void flyAnimal(Fly fly) {
    System.out.println("날기 테스트 시작");
    fly.fly();
    System.out.println("날기 테스트 종료");
  }
}
```

**실행 결과**
```
동물 소리 테스트 시작
멍멍
동물 소리 테스트 종료

동물 소리 테스트 시작
짹짹
동물 소리 테스트 종료

동물 소리 테스트 시작
꼬끼오
동물 소리 테스트 종료

날기 테스트 시작
새 날기
날기 테스트 종료

날기 테스트 시작
닭 날기
날기 테스트 종료
```

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%84%89%E1%85%B3%E1%84%8B%E1%85%AA%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%90%E1%85%A5%E1%84%91%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BCBird.png?raw=true">

`soundAnimal(AbstractAnimal animal)`
`AbstractAnimal`를 상속한 `Dog`, `Bird`, `Chicken`을 전달해서 실행할 수 있습니다.

**실행 과정**
* `soundAnimal(bird)`를 호출한다고 가정합시다.
* 메서드 안에서 `animal.sound()`를 호출하면 참조 대상인 `x001` `Bird` 인스턴스를 찾습니다.
* 호출한 `animal` 변수는 `AbstractAnimal` 타입입니다. 따라서 `AbstractAnimal.sound()`를 찾습니다. 해당 메서드는 `Bird.sound()`에 오버라이딩 되어 있습니다.
* `Bird.sound()`가 호출됩니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%84%89%E1%85%B3%E1%84%8B%E1%85%AA%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%90%E1%85%A5%E1%84%91%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BCBird-fly.png?raw=true">

`flyAnimal(Fly fly)`
`Fly` 인터페이스를 구현한 `Bird`, `Chicken`을 전달해서 실행할 수 있습니다.

**실행과정**
* `fly(bird)`를 호출한다고 가정합시다.
* 메서드 안에서 `fly.fly()`를 호출하면 참조 대상인 `x001` `Bird` 인스턴스를 찾습니다.
* 호출한 `fly` 변수는 `Fly` 타입입니다. 따라서 `Fly.fly()`를 찾습니다. 해당 메서드는 `Bird.fly()`에 오버라이딩 되어 있습니다.
* `Bird.fly()`가 호출됩니다.
