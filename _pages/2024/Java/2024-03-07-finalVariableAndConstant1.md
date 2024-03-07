---
title: "☕️[Java] final 변수와 상수 1"
tags:
    - Java
    - Programming Language
date: "2024-03-07"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# final 변수와 상수 1

* `final` 키워드는 이름 그대로 끝! 이라는 뜻 입니다.
* 변수에 `final` 키워드가 붙으면 더는 값을 변경할 수 없습니다.

> 참고로 `final`은 `class`, `method`를 포함한 여러곳에 붙을 수 있습니다.

## final - 지역 변수
```java
package final1;

public class FinalLocalMain {

  public static void main(String[] args) {
    // final 지역 변수1
    final int data1;
    data1 = 10; // 최초 한번만 할당 가능
    // data1 = 20; // 컴파일 오류

    // final 지역 변수2
    final int data2 = 10;
    // data2 = 20; // 컴파일 오류
    method(10);
  }

  static void method(final int parameter) {
    // parameter = 20; // 컴파일 오류
  }
}
```
* `final` 을 지역 변수에 설정할 경우 최초 한번만 할당할 수 있습니다.
    * 이후에 변수의 값을 변경하려면 컴파일 오류가 발생합니다.
* `final`을 지역 변수 선언시 바로 초기화 한 경우 이미 값이 할당되었기 때문에 값을 할당할 수 없습니다.
* 매개변수에 `final`이 붙으면 메서드 내부에서 매개변수의 값을 변셩할 수 없습니다.
    * 따라서 메서드 호출 시점에 사용된 값이 끝까지 사용됩니다.

## final - 필드(맴버변수)
```java
package final1;

public class ConstructInit {

  final int value;

  public ConstructInit(int value) {
    this.value = value;
  }
}
```
* `final` 을 필드에 사용할 경우 해당 필드는 생성자를 통해서 한번만 초기화 될 수 있습니다.

```java
package final1;

public class ConstructInit {

  final int value;

  public ConstructInit(int value) {
    this.value = value;
  }
}
```
* `final` 필드를 필드에서 초기화하면 이미 값이 설정되었기 때문에 생성자를 통해서도 초기화 할 수 없습니다.
    * `value` 필드를 참고합시다.
* 코드에서 보는 것 처럼 `static` 변수에도 `final`을 선언할 수 있습니다.
    * `CONSTANT_VALUE`로 변수 작명 방법이 대문자를 사용하였습니다. 이것은 상수입니다.

```java
package final1;

public class FinalFieldMain {

  public static void main(String[] args) {
    // final 필드 - 생성자 초기화
    System.out.println("생성자 초기화");
    ConstructInit constructInit1 = new ConstructInit(10);
    ConstructInit constructInit2 = new ConstructInit(20);
    System.out.println(constructInit1.value);
    System.out.println(constructInit2.value);

    // final 필드 - 필드 초기화
    System.out.println("필드 초기화");
    FieldInit fieldInit1 = new FieldInit();
    FieldInit fieldInit2 = new FieldInit();
    FieldInit fieldInit3 = new FieldInit();
    System.out.println(fieldInit1.value);
    System.out.println(fieldInit2.value);
    System.out.println(fieldInit3.value);
    
    // 상수
    System.out.println("싱수");
    System.out.println(FieldInit.CONST_VALUE);
  }
}
```

**실행 결과**
```
생성자 초기화
10
20
필드 초기화
10
10
10
싱수
10
```

* `ConstructInit`과 같이 생성자를 사용해서 `final` 필드를 초기화 하는 경우, 각 인스턴스마다 `final` 필드에 다른 값을 할당할 수 있습니다.
    * 물론 `final`을 사용했기 때문에 생성 이후에 이 값을 변경하는 것은 불가능합니다.

<img src="https://github.com/devKobe24/images/blob/main/FieldInin-1.png?raw=true">

* `FieldInit`과 같이 `final` 필드를 필드에서 초기화 하는 경우, 모든 인스턴스가 위 그림의 오른쪽과 같이 같은 값을 가집니다.
    * 여기서는 `FieldInit` 인스턴스의 모든 `value` 값은 `10`이 됩니다.
        * 왜냐하면 생성자 초기화와 다르게 필드 초기화는 필드의 코드에 해당 값이 미리 정해져있기 때문입니다.
* 모든 인스턴스가 같은 값을 사용하기 때문에 결과적으로 메모리를 낭비하게 됩니다.(물론 JVM에 따라서 내부 최적화를 시도할 수 있습니다.)
    * 또 메모리 낭비를 떠나서 같은 값이 계속 생성되는 것은 개발자가 보기에 명확한 중복입니다.
        * 이럴 때 사용하면 좋은 것이 바로 `static` 영역입니다.

**static final**
* `FieldInit.MY_VALUE`는 `static` 영역에 존재합니다.
    * 그리고 `final` 키워드를 사용해서 초기화 값이 변하지 않습니다
* `static` 영역은 단 하나만 존재하는 영역입니다.
    * `MY_VALUE` 변수는 JVM 상에서 하나만 존재하므로 앞서 설명한 중복과 메모리 비효율 문제를 모두 해결할 수 있습니다.

이런 이유로 필드에 `final` + 필드 초기화를 사용하는 경우 `static`을 붙여서 사용하는 것이 효과적입니다.
