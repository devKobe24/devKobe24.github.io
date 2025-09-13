---
title: "☕️[Java] 인터페이스"
tags:
    - Java
    - Programming Language
date: "2024-03-21"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 인터페이스

**"자바는 순수 추상 클래스를 더 편리하게 사용할 수 있는 인터페이스라는 기능을 제공합니다."**

## 순수 추상 클래스
```java
public abstract class AbstractAnimal {
    public abstract void sound();
    public abstract void move();
}
```

**"인터페이스는 `class` 가 아니라 `interface` 키워드를 사용하면 됩니다."**

## 인터페이스
```java
public interface InterfaceAnimal {
    public abstract void sound();
    public abstract void move();
}
```

## 인터페이스 - public abstract 키워드 생략 가능
```java
public interface InterfaceAnimal {
    void sound();
    void move();
}
```

순수 추상 클래스는 다음과 같은 특징을 갖습니다.
* 인스턴스를 생성할 수 없습니다.
* 상속시 모든 메서드를 오버라이딩 해야 합니다.
* 주로 다형성을 위해 사용됩니다.

인터페이스는 앞서 설명한 순수 추상 클래스와 같습니다. 여기에 약간의 편의 기능이 추가 됩니다.
* 인터페이스 메서드는 모두 `public`, `abstract`입니다.
* 메서드에 `public abstract`를 생략할 수 있습니다.(참고로 생략이 권장됩니다.)
* 인터페이스는 다중 구현(다중 상속)을 지원합니다.

## 인터페이스와 멤버 변수
```java
public interface InterfaceAnimal {
    public static final int MY_PI = 3.14;
}
```

* 인터페이스에서 멤버 변수는 `public`, `static`, `final`이 모두 포함되었다고 간주됩니다.
    * `final`은 변수의 값을 한 번 설정하면 수정할 수 없다는 뜻입니다.
    * 자바에서 `static final`을 사용해 정적이면서 고칠 수 없는 변수를 상수라 하고, 관례상 상수는 대문자에 언더스코어`(_)`로 구분합니다.

해당 키워드는 다음과 같이 생략할 수 있습니다.(생략이 권장됩니다.)
```java
public interface InterfaceAnimal {
    int MY_PI = 3.14;
}
```

### 예제 5

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%90%E1%85%A5%E1%84%91%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%E1%84%8B%E1%85%A8%E1%84%8C%E1%85%A65.png?raw=true">

* 클래스 상속 관계는 UML에서 실선을 사용하지만, 인터페이스 구현(상속)관계는 UML에서 점선을 사용합니다.

```java
package poly.ex5;

public class InterfaceMain {

  public static void main(String[] args) {
    // 인터페이스 생성 불가
    // InterfaceAnimal interfaceAnimal = new InterfaceAnimal();

    Cat cat = new Cat();
    Dog dog = new Dog();
    Caw caw = new Caw();

    soundAnimal(cat);
    soundAnimal(dog);
    soundAnimal(caw);
  }

  // 변하지 않는 부분
  private static void soundAnimal(InterfaceAnimal animal) {
    System.out.println("동물 소리 테스트 시작");
    animal.sound();
    System.out.println("동물 소리 테스트 종료");
  }
}
```

**실행 결과**
```
동물 소리 테스트 시작
야옹
동물 소리 테스트 종료

동물 소리 테스트 시작
멍멍
동물 소리 테스트 종료

동물 소리 테스트 시작
음메
동물 소리 테스트 종료
````

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%90%E1%85%A5%E1%84%91%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3dog.png?raw=true">

앞서 설명한 순수 추상 클래스 예제와 거의 유사합니다.
* 순수 추상 클래스가 인터페이스가 되었을 뿐입니다.

#### 클래스, 추상 클래스, 인터페이스는 모두 똑같습니다.
* 클래스, 추상 클래스, 인터페이스는 프로그램 코드, 메모리 구조상 모두 똑같습니다.
    * 모두 자바에서는 `.class로` 로 다루어집니다.
        * 인터페이스를 작성할 때도 `.java`에 인터페이스를 정의합니다.
* 인터페이스는 순수 추상 클래스와 비슷하다고 생각하면 됩니다.

#### 상속 vs 구현
부모 클래스의 기능을 자식 클래스가 상속 받을 때, 클래스는 상속 받는다고 표현하지만, 부모 인터페이스의 기능을 자식이 상속 받을 때는 인터페이스를 구현한다고 표현합니다.

이렇게 서로 다르게 표현하는 이유를 알아봅시다.

* 상속은 이름 그대로 부모의 기능을 물려 받는 것이 목적입니다.
    * 하지만 인터페이스는 모든 메서드가 추상 메서드입니다.
        * 따라서 물려 받을 수 있는 기능이 없고, 오히려 인터페이스에 정의한 모든 메서드를 자식이 오버라이딩 해서 기능을 구현해야 합니다.
            * 따라서 구현한다고 표현합니다.
* 인터페이스는 메서드 이름만 있는 설계도이고, 이 설계도가 실제 어떻게 작동하는지는 하위 클래스에서 모두 구현해야 합니다.
    * 따라서 인터페이스의 경우 상속이 아니라 해당 인터페이스를 구현한다고 표현합니다.

상속과 구현은 사람이 표현하는 단어만 다를 뿐이지 자바 입장에서는 똑같습니다. 일반 상속 구조와 동일하게 작동합니다.

#### 인터페이스를 사용해야 하는 이유.
모든 메서드가 추상 메서드인 경우 순수 추상 클래스로 만들어도 되고, 인터페이스를 만들어도 됩니다.

그런데 왜 인터페이스를 사용해야 할까요? 단순히 편리하다는 이유를 넘어서 다음과 같은 이유가 있습니다.

* **제약 :** 인터페이스를 만드는 이유는 인터페이스를 구현하는 곳에서 인터페이스의 메서드를 반드시 구현하라는 규약(제약)을 주는 것입니다.
    * USB 인터페이스를 생각해봅시다. USB 인터페이스에 맞추어 키보드, 마우스를 개발하고 연결해야 합니다. 그렇지 않으면 작동하지 않습니다.
    * 인터페이스의 규약(제약)은 반드시 구현해야 하는 것입니다.
        * 그런데 순수 추상 클래스의 경우 미래에 누군가 그곳에 실행 가능한 메서드를 끼워 넣을 수 있습니다.
            * 이렇게 되면 추가된 기능을 자식 클래스에서 구현하지 않을 수도 있고, 또 더는 순수 추상 클래스가 아니게 됩니다.
                * 인터페이스는 모든 메서드가 추상 메서드입니다. 따라서 이런 문제를 원천 차단할 수 있습니다.
* **다중 구현 :** 자바에서 클래스 상속은 부모를 하나만 지정할 수 있습니다.
    * 반면에 인터페이스는 부모를 여러명 두는 다중 구현(다중 상속)이 가능합니다.

좋은 프로그램은 제약이 있는 프로그램입니다.

> **참고**
> 자바 8에 등장한 `default` 메서드를 사용하면 인터페이스도 메서드를 구현할 수 있습니다.
> 하지만 이것은 예외적으로 아주 특별한 경우에만 사용해야 합니다.
> 자바 9에서 등장한 인터페이스의 `private` 메서드도 마찬가지입니다.
> 지금 학습 단계에서는 이 부분들을 고려하지 않는 것이 좋습니다.
> 이 부분은 추후에 따로 학습하고 정리할 것 입니다.
