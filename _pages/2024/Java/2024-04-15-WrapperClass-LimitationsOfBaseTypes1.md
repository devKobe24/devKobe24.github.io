---
title: "☕️[Java] 래퍼 클래스 - 기본형의 한계 1"
tags:
    - Java
    - Programming Language
date: "2024-04-15"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 래퍼 클래스 - 기본형의 한계 1

## 기본형의 한계
자바는 객체 지향 언어입니다.
그런데 자바 안에 객체가 아닌 것이 있습니다.
- 바로 `int`, `double` 같은 기본형(Primitive Type)입니다.

기본형은 객체가 아니기 때문에 다음과 같은 한계가 있습니다.
- **객체가 아님:** 기본형 데이터는 객체가 아니기 때문에, 객체 지향 프로그래밍의 장점을 살릴 수 없습니다. 예를 들어 객체는 유용한 메서드를 제공할 수 있는데, 기본형은 객체가 아니므로 메서드를 제공할 수 없습니다.
    - 추가로 객체 참고가 필요한 컬렉션 프레임워크를 사용할 수 없습니다. 그리고 제네릭도 사용할 수 없습니다.
- **null 값을 가질 수 없음:** 기본형 데이터 타입은 `null` 값을 가질 수 없습니다. 때로는 데이터가 `없음` 이라는 상태를 나타내야 할 필요가 있는데, 기본형은 항상 값을 가지기 때문에 이런 표현을 할 수 없습니다.

기본형의 한계를 이해하기 위해, 두 값을 비교해서 다음과 같은 결과를 출력하는 간단한 코드를 작성해봅시다.
- 왼쪽의 값이 더 작다 `-1`
- 두 값이 같다 `0`
- 왼쪽의 값이 더 크다 `1`

```java
package lang.wrapper;

public class MyIntegerMethodMain0 {

  public static void main(String[] args) {
    int value = 10;
    int i1 = compareTo(value, 5);
    int i2 = compareTo(value, 10);
    int i3 = compareTo(value, 20);
    System.out.println("i1 = " + i1);
    System.out.println("i2 = " + i2);
    System.out.println("i3 = " + i3);
  }

  public static int compareTo(int value, int target) {
    if (value < target) {
      return -1;
    } else if (value > target) {
      return 1;
    } else {
      return 0;
    }
  }
}
```

**실행 결과**
```
i1 = 1
i2 = 0
i3 = -1
```

- 여기서는 `value`와 비교 대상 값을 `compareTo()`라는 외부 메서드를 사용해서 비교합니다.
    - 그런데 자기 자신인 `value`와 다른 값을 연산하는 것이기 때문에 항상 자기 자신의 값인 `value`가 사용됩니다.
        - 이런 경우 만약 `value`가 객체라면 `value` 객체 스스로 가지 자신의 값과 다른 값을 비교하는 메서드를 만드는 것이 더 유용할 것입니다.

## 직접 만든 래퍼 클래스.
`int`를 클래스로 만들어 봅시다.
- `int`는 클래스가 아니지만, `int` 값을 가지고 클래스를 만들면 됩니다.

다음 코드는 마치 `int`를 클래스로 감싸서 만드는 것 처럼 보입니다.
- 이렇게 특정 기본형을 감싸서(Wrap) 만드는 클래스를 래퍼 클래스(Wrapper class)라 합니다.

```java
package lang.wrapper;

public class MyInteger {
  private final int value;

  public MyInteger(int value) {
    this.value = value;
  }

  public int getValue() {
    return value;
  }

  public int compareTo(int target) {
    if (value < target) {
      return -1;
    } else if (value > target) {
      return 1;
    } else {
      return 0;
    }
  }

  @Override
  public String toString() {
    return String.valueOf(value);
  }
}
```

- `MyInteger`는 `int value`라는 단순한 기본형 변수를 하나 가지고 있습니다.
    - 그리고 이 기본형 변수를 편리하게 사용하도록 다양한 메서드를 제공합니다.
    - 앞에서 본 `compareTo()` 메서드를 클래스 내부로 캡슐화 했습니다.
    - 이 클래스는 불변으로 설계했습니다.

`MyInteger` 클래스는 단순한 데이터 조각인 `int`를 내부에 품고, 메서드를 통해 다양한 기능을 추가했습니다.
- 덕분에 데이터 조각에 불과한 `int`를 `MyInteger`를 통해 객체로 다룰 수 있게 되었습니다.

```java
package lang.wrapper;

public class MyIntegerMethodMain1 {

  public static void main(String[] args) {
    MyInteger myInteger = new MyInteger(10);
    int i1 = myInteger.compareTo(5);
    int i2 = myInteger.compareTo(10);
    int i3 = myInteger.compareTo(20);
    System.out.println("i1 = " + i1);
    System.out.println("i2 = " + i2);
    System.out.println("i3 = " + i3);
  }
}
```

**실행 결과**
```
i1 = 1
i2 = 0
i3 = -1
```
- `myInteger.compareTo()`는 자기 자신의 값을 외부의 값과 비교합니다.
- `MyInteger`는 객체이므로 자신이 가진 메서드를 편리하게 호출할 수 있습니다.
- 참고로 `int`는 기본형이기 때문에 스스로 메서드를 가질 수 없습니다.
