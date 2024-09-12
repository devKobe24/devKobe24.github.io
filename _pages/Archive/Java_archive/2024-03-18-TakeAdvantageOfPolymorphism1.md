---
title: "☕️[Java] 다형성 활용1"
tags:
    - Java
    - Programming Language
date: "2024-03-18"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 다형성 활용1

지금까지 학습한 다형성을 왜 사용하는지, 그 장점을 알아보기 위해 우선 다형성을 사용하지 않고 프로그램을 개발한 다음에 다형성을 사용하도록 코드를 변경해보겠습니다.

아주 단순하고 전통적인 동물 소리 문제로 접근해보겠습니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC1.png?raw=true">

개, 고양이, 소의 울음 소리를 테스트하는 프로그램을 작성해봅시다.

먼저 다형성을 사용하지 않고 코드를 작성해봅시다.

```java
package poly.ex1;

public class Dog {

  public void sound() {
    System.out.println("멍멍");
  }
}
```

```java
package poly.ex1;

public class Cat {

  public void sound() {
    System.out.println("야옹");
  }
}
```

```java
package poly.ex1;

public class Caw {

  public void sound() {
    System.out.println("음메");
  }
}
```

```java
package poly.ex1;

public class AnimalSoundMain {

  public static void main(String[] args) {
    Dog dog = new Dog();
    Cat cat = new Cat();
    Caw caw = new Caw();

    System.out.println("동물 소리 테스트 시작");
    dog.sound();
    System.out.println("동물 소리 테스트 종료");

    System.out.println("동물 소리 테스트 시작");
    cat.sound();
    System.out.println("동물 소리 테스트 종료");

    System.out.println("동물 소리 테스트 시작");
    caw.sound();
    System.out.println("동물 소리 테스트 종료");
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

단순히 개, 고양이, 소 동물들의 울음 소리를 출력하는 프로그램입니다.

만약 여기서 새로운 동물이 추가되면 어떻게 될까요?

만약 기존 코드에 소가 없다고 가정해봅시다.

소가 추가된다고 가정하면 `Caw` 클래스를 만들고 다음 코드도 추가해야 합니다.

```java
// Caw를 생성하는 코드
Caw caw = new Caw();

// Caw를 사용하는 코드
System.out.println("동물 소리 테스트 시작");
caw.sound();
System.out.println("동물 소리 테스트 종료");
```

`Caw`를 생성하는 부분은 당연히 필요하니 크게 상관이 없지만, `Dog`, `Cat`, `Caw`를 사용해서 출력하는 부분은 계속 중복이 증가합니다.

**중복 코드**
```java
System.out.println("동물 소리 테스트 시작");
dog.sound();
System.out.println("동물 소리 테스트 종료");

System.out.println("동물 소리 테스트 시작");
cat.sound();
System.out.println("동물 소리 테스트 종료");

System.out.println("동물 소리 테스트 시작");
caw.sound();
System.out.println("동물 소리 테스트 종료");
```

중복을 제거하기 위해서는 메서드를 사용하거나, 또는 배열과 `for`문을 사용하면 됩니다.
그런데 `Dog`, `Cat`, `Caw`는 서로 완전히 다른 클래스입니다.

## 중복 제거 시도
### 메서드로 중복 제거 시도

메서드를 사용하면 다음과 같이 매개변수의 클래스를 `Caw`, `Dog`, `Cat` 중에 하나로 정해야 합니다.

```java
private static void soundCaw(Caw caw) {
    System.out.println("동물 소리 테스트 시작");
    caw.sound();
    System.out.println("동물 소리 테스트 종료");
}
```
* 따라서 이 메서드는 `Caw` 전용 메서드가 되고 `Dog`, `Cat`은 인수로 사용할 수 없습니다.
    * `Dog`, `Cat`, `Caw`의 타입(클래스)이 서로 다르기 때문에 `soundCaw` 메서드를 함께 사용하는 것은 불가능합니다.

### 배열과 for문을 통한 중복 제거 시도
```java
Caw[] cawArr = { cat, dog, caw }; // 컴파일 오류 발생!
System.out.println("동물 소리 테스트 시작");
    for (Caw caw : cawArr) {
        cawArr.sound()
}
System.out.println("동물 소리 테스트 종료");
```

배열과 for문 사용해서 중복을 제거하려고 해도 배열의 타입을 `Dog`, `Cat`, `Caw` 중에 하나로 지정해야 합니다.
* 같은 `Caw`들을 배열에 담아서 처리하는 것은 가능하지만 타입이 서로 다른 `Dog`, `Cat`, `Caw`을 하나의 배열에 담는 것은 불가능합니다.
    * 결과적으로 지금 상황에서는 해결방법이 없습니다.
        * 새로운 동물이 추가될 때 마다 더 많은 중복 코드를 작성해야 합니다.

지금까지 설명한 모든 중복 제거 시도가 `Dog`, `Cat`, `Caw`의 타입이 서로 다르기 때문에 불가능합니다.
* **"문제의 핵심은 바로 타입이 다르다는 점"** 입니다.
    * 반대로 이야기하면 `Dog`, `Cat`, `Caw`가 모두 같은 타입을 사용할 수 있는 방법이 있다면 메서드와 배열을 활용해서 코드의 중복을 제거할 수 있다는 것입니다.

다형성의 핵심은 다형적 참조와 메서드 오버라이딩입니다.
* 이 둘을 활용하면 `Dog`, `Cat`, `Caw` 가 모두 같은 타입을 사용하고, 각자 자신의 메서드로 호출할 수 있습니다.
