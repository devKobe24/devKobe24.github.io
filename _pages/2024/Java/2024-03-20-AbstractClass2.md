---
title: "☕️[Java] 추상 클래스 2"
tags:
    - Java
    - Programming Language
date: "2024-03-20"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 추상 클래스 2
## 순수 추상 클래스: 모든 메서드가 추상 메서드인 추상 클래스

앞서 만든 예제에서 `move()`도 추상 메서드로 만들어야 한다고 가정해봅시다.
* 이 경우 `AbstractAnimal` 클래스의 모든 메서드가 추상 메서드가 됩니다.
    * 이런 클래스를 **"순수 추상 클래스"** 라 합니다.

`move()`가 추상 메서드가 되었으니 자식들은 `AbstractAnimal`의 모든 기능을 오버라이딩 해야 합니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%AE%E1%86%AB%E1%84%89%E1%85%AE%E1%84%8E%E1%85%AE%E1%84%89%E1%85%A1%E1%86%BC%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%84%89%E1%85%B3.png?raw=true">

### 예제 4
```java
package poly.ex4;

public class AbstractMain {

  public static void main(String[] args) {
    // 추상클래스 생성 불가
    //AbstractAnimal animal = new AbstractAnimal();

    Dog dog = new Dog();
    Cat cat = new Cat();
    Caw caw = new Caw();

    soundAnimal(dog);
    soundAnimal(cat);
    soundAnimal(caw);

    moveAnimal(dog);
    moveAnimal(cat);
    moveAnimal(caw);
  }

  // 변하지 않는 부분
  private static void soundAnimal(AbstractAnimal animal) {
    System.out.println("동물 소리 테스트 시작");
    animal.sound();
    System.out.println("동물 소리 테스트 종료");
  }

  // 변하지 않는 부분
  private static void moveAnimal(AbstractAnimal animal) {
    System.out.println("동물 이동 테스트 시작");
    animal.move();
    System.out.println("동물 이동 테스트 종료");
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
음매
동물 소리 테스트 종료

동물 이동 테스트 시작
댕댕이 이동
동물 이동 테스트 종료

동물 이동 테스트 시작
냥냥이 이동
동물 이동 테스트 종료

동물 이동 테스트 시작
소 이동
동물 이동 테스트 종료
```

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%AE%E1%86%AB%E1%84%89%E1%85%AE%E1%84%8E%E1%85%AE%E1%84%89%E1%85%A1%E1%86%BC%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%84%89%E1%85%B32.png?raw=true">

### 순수 추상 클래스

모든 메서드가 추상 메서드인 순수 추상 클래스는 코드를 실행할 바디 부분이 전혀 없습니다.
```java
public abstract class AbstractAnimal {
    public abstract void sound();
    public abstract void move();
}
```
* 이러한 순수 추상 클래스는 실행 로직을 전혀 가지고 있지 않습니다.
    * 단지 다형성을 위한 부모 타입으로써 껍데기 역할만 제공할 뿐입니다.

순수 추상 클래스는 다음과 같은 특징을 가집니다.
* 인스턴스를 생성할 수 없습니다.
* 상속시 자식은 모든 메서드를 오버라이딩 해야 합니다.
* 주로 다형성을 위해 사용됩니다.

### 상속하는 클래스는 모든 메서드를 구현해야 합니다.
"상속시 자식은 모든 메서드를 오버라이딩 해야 합니다."라는 특징은 상속 받는 클래스 입장에서 보면 부모의 모든 메서드를 구현해야 하는 것 입니다.
* 이런 특징을 잘 생각해보면 순수 추상 클래스는 마치 어떤 규격을 지켜서 구현해야 하는 것 처럼 느껴집니다.
* `AbstractAnimal`의 경우 `sound()`, `move()` 라는 규격에 맞추어 구현을 해야 합니다.

이것은 우리가 일반적으로 이야기하는 인터페이스와 같이 느껴집니다.
예를 들어서 USB 인터페이스를 생각해봅시다.
USB 인터페이스는 분명한 규격이 있습니다.
이 규격에 맞추어 제품을 개발해야 연결이 됩니다.
* 순수 추상 클래스가 USB 인터페이스 규격이라고 한다면 USB 인터페이스에 맞추어 마우스, 키보드 같은 연결 장치들을 구현할 수 있습니다.

이런 순수 추상 클래스의 개념은 프로그래밍에서 매우 자주 사용됩니다.
* 자바는 순수 추상 클래스를 더 편리하게 사용할 수 있도록 인터페이스라는 개념을 제공합니다.
