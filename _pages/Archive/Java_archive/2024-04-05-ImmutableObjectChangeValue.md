---
title: "☕️[Java] 불변 객체 - 값 변경"
tags:
    - Java
    - Programming Language
date: "2024-04-05"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 불변 객체 - 값 변경.
불변 객체를 사용하지만 그래도 값을 변경해야 하는 메서드가 필요하면 어떻게 해야할까요?
- 예를 들어서 기존 값에 새로운 값을 더하는 `add()`와 같은 메서드가 있다고 합시다.

먼저 변경 가능한 객체에서 값을 변경하는 간단한 예를 만들어봅시다.

```java
package lang.immutable.change;

public class MutableMain {

  public static void main(String[] args) {
    MutableObj obj = new MutableObj(10);
    obj.add(20);
    // 계산 이후의 기존 값은 사라짐
    System.out.println("obj = " + obj.getValue());
  }
}
```

**실행 결과**
```
obj = 30
```

- `MutableObj`을 `10`이라는 값으로 생성합니다.
    - 이후에 `obj.add(20)`을 통해서 `10 + 20`을 수행합니다.
        - 계산 이후에 기존에 있던 `10`이라는 값은 사라집니다.
        - `MutableObj`의 상태(값)가 `10` -> `30`으로 변경되었습니다.
    - `obj.getValue()`를 호출하면 `30`이 출력됩니다.

이번에는 불변 객체에서 `add()` 메서드를 어떻게 구현하는지 알아봅시다.
- 참고로 불변 객체는 변하지 않아야 합니다.

```java
package lang.immutable.change;

public class ImmutableObj {

  private final int value;

  public ImmutableObj(int value) {
    this.value = value;
  }

  public ImmutableObj add(int addValue) {
    int result = value + addValue;
    return new ImmutableObj(result);
  }

  public int getValue() {
    return value;
  }
}
```
- 여기서 핵심은 `add()` 메서드 입니다.
- 불변 객체는 값을 변경하면 안됩니다!
    - 그러면 이미 불변 객체가 아닙니다!
    - 하지만 여기서는 기존 값에 새로운 값을 더해야 합니다.
- 불변 객체는 기존 값은 변경하지 않고 대신에 계산 결과를 바탕으로 새로운 객체를 만들어서 반환합니다.
    - 이렇게 하면 불변도 유지하면서 새로운 결과도 만들 수 있습니다.

```java
package lang.immutable.change;

public class ImmutableMain1 {

  public static void main(String[] args) {
    ImmutableObj obj1 = new ImmutableObj(10);
    ImmutableObj obj2 = obj1.add(20);

    // 계산 이후에도 기존값과 신규값 모두 확인 가능
    System.out.println("obj1 = " + obj1.getValue());
    System.out.println("obj2 = " + obj2.getValue());
  }
}
```

**실행 결과**
```
obj1 = 10
obj2 = 30
```
- 불변 객체를 설계할 때 기존 값을 변경해야 하는 메서드가 필요할 수 있습니다.
    - 이때는 기존 객체의 값을 그대로 두고 대신에 변경된 결과를 새로운 객체에 담아서 반환하면 됩니다.
        - 결과를 보면 기존 값이 그대로 유지되는 것을 확인할 수 있습니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%87%E1%85%AE%E1%86%AF%E1%84%87%E1%85%A7%E1%86%AB%E1%84%80%E1%85%A2%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%80%E1%85%A1%E1%86%B9%E1%84%87%E1%85%A7%E1%86%AB%E1%84%80%E1%85%A7%E1%86%BC%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B7-1.png?raw=true">

1. `add(20)`을 호출합니다.
2. 기존 객체에 있는 `10`과 인수로 입력한 `20`을 더합니다. 이때 기존 객체의 값을 변경하면 안되므로 계산 결과를 기반으로 새로운 객체를 만들어서 반환합니다.
3. 새로운 객체는 `x002` 참조를 가집니다. 새로운 객체의 참조값을 `obj2`에 대입합니다.

만약 여기서 다음과 같이 새로 생성된 반환 값을 사용하지 않으면 어떻게 될까요?
```java
package lang.immutable.change;

public class ImmutableMain2 {

  public static void main(String[] args) {
    ImmutableObj obj1 = new ImmutableObj(10);
    obj1.add(20);

    // 계산 이후에도 기존값과 신규값 모두 확인 가능
    System.out.println("obj1 = " + obj1.getValue());
  }
}
```

**실행 결과**
```
obj1 = 10
````
- 실행 결과처럼 아무것도 처리되지 않은 것 처럼 보일 것입니다.
    - 불변 객체에서 변경과 관련된 메서드들은 보통 객체를 새로 만들어서 반환하기 때문에 **꼭! 반환 값을 받아야 합니다.**
