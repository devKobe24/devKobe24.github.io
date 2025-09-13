---
title: "☕️[Java] static 메서드 2"
tags:
    - Java
    - Programming Language
date: "2024-03-05"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# static 메서드 2

정적 메서드는 객체 생성없이 클래스에 있는 메서드를 바로 호출할 수 있다는 장점이 있습니다.
하지만 정적 메서드는 언제나 사용할 수 있는 것이 아닙니다.

## 정적 메서드 사용법
* `static` 메서드는 `static`만 사용할 수 있습니다.
    * 클래스 내부의 기능을 사용할 때, 정적 메서드는 `static`이 붙은 **정적 메서드나 정적 변수만 사용할 수 있습니다.**
    * 클래스 내부의 기능을 사용할 때, 정적 메서드는 인스턴스 변수나, 인스턴스 메서드를 사용할 수 없습니다.
* 반대로 모든 곳에서 `static`을 호출할 수 있습니다.
    * 정적 메서드는 공용 기능입니다.
        * 따라서 접근 제어자만 허락한다면 클래스를 통해 모든 곳에서 `static`을 호출할 수 있습니다.

예제를 통해 정적 메서드의 사용법을 확인해봅시다.

**DecoData**
```java
package static2;

public class DecoData {

  private int instanceValue;
  private static int staticValue;

  public static void staticCall() {
    //instanceValue++; // 인스턴스 변수 접근, compile error
    //instanceMethod(); // 인스턴스 메서드 접근, compile error

    staticValue++; // 정적 변수 접근
    staticMethod(); // 정적 메서드 접근
  }

  public void instanceCall() {
    instanceValue++; // 인스턴스 변수 접근
    instanceMethod(); // 인스턴스 메서드 접근

    staticValue++; // 정적 변수 접근
    staticMethod(); // 정적 메서드 접근
  }

  private void instanceMethod() {
    System.out.println("instanceValue=" + instanceValue);
  }

  private static void staticMethod() {
    System.out.println("staticValue=" + staticValue);
  }
}
```

이번 예제에서는 접근 제어자를 적극 활용해서 필드를 포함한 외부에서 직접 필요하지 않은 기능은 모두 막아두었습니다.

* `instanceValue`는 인스턴스 변수입니다.
* `staticValue`는 정적 변수(클래스 변수)입니다.
* `instanceMethod()`는 인스턴스 메서드입니다.
* `staticMethod()`는 정적 메서드(클래스 메서드)입니다.

**`staticCall()` 메서드를 봐봅시다.**
* 이 메서드는 정적 메서드입니다.
    * 따라서 `static` 만 사용할 수 있습니다.
    * 정적 변수, 정적 메서드에는 접근할 수 있지만, `static`이 없는 인스턴스 변수나 인스턴스 메서드에 접근하면 **컴파일 오류가 발생**합니다.
    * 코드를 보면 `staticCall()` -> `staticMethod()`로 `static`에서 `static`을 호출하는 것을 확인할 수 있습니다.

**`instanceCall()` 메서드를 봐봅시다.** 
* 이 메서드는 인스턴스 메서드입니다.
    * 모든 곳에서 공용인 `static`을 호출할 수 있습니다.
        * 따라서 정적 변수, 정적 메서드에 접근할 수 있습니다.
        * 물론 인스턴스 변수, 인스턴스 메서드에도 접근할 수 있습니다.

**DecoDataMain**
```java
package static2;

public class DecoDataMain {

  public static void main(String[] args) {
    System.out.println("1. 정적 호출");
    DecoData.staticCall();

    System.out.println("2. 인스턴스 호출1");
    DecoData data1 = new DecoData();
    data1.instanceCall();

    System.out.println("3. 인스턴스 호출2");
    DecoData data2 = new DecoData();
    data2.instanceCall();
  }
}
```

**실행 결과**
```
1. 정적 호출
staticValue=1
2. 인스턴스 호출1
instanceValue=1
staticValue=2
3. 인스턴스 호출2
instanceValue=1
staticValue=3
```

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%E1%84%86%E1%85%A6%E1%84%89%E1%85%A5%E1%84%83%E1%85%B3%E1%84%80%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%89%E1%85%B3%E1%84%90%E1%85%A5%E1%86%AB%E1%84%89%E1%85%B3%E1%84%8B%E1%85%B4%E1%84%80%E1%85%B5%E1%84%82%E1%85%B3%E1%86%BC%E1%84%8B%E1%85%B3%E1%86%AF%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AF%E1%84%89%E1%85%AE%E1%84%8B%E1%85%A5%E1%86%B9%E1%84%82%E1%85%B3%E1%86%AB%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%B2.png?raw=true">

**정적 메서드가 인스턴스의 기능을 사용할 수 없는 이유**
* 정적 메서드는 클래스의 이름을 통해 바로 호출할 수 있습니다.
    * 그래서 인스턴스처럼 참조값의 개념이 없습니다.
        * 특정 인스턴스의 기능을 사용하려면 참조값을 알아야 하는데, 정적 메서드는 참조값 없이 호출합니다.
            * 따라서 정적 메서드 내부에서 인스턴스 변수나 인스턴스 메서드를 사용할 수 없습니다.

물론 당연한 이야기지만 다음과 같이 객체의 참조값을 직접 매개변수로 전달하면 정적 메서드도 인스턴스의 변수나 메서드를 호출할 수 있습니다.
```java
public static void staticCall(DecoData data) {
    data.instanceValue++;
    data.instanceMethod();
}
```
