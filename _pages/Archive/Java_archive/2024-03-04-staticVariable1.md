---
title: "☕️[Java] static 변수1"
tags:
    - Java
    - Programming Language
date: "2024-03-04"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# static 변수 1

`sttic` 키워드는 주로 멤버 변수와 메서드에 사용됩니다.
먼저 멤버 변수에 `static` 키워드가 왜 필요한지 이해하기 위해 간단한 예제를 만들어봅시다.

특정 클래스를 통해서 생성된 객체의 수를 세는 단순한 프로그램입니다.

## 인스턴스 내부 변수에 카운트 저장

먼저 생성할 인스턴스 내부에 카운트를 저장하겠습니다.

**Data1**
```java
package static1;

public class Data1 {
  public String name;
  public int count;

  public Data1(String name) {
    this.name = name;
    count++;
  }
}
```

* 생성된 객체의 수를 세어야 합니다.
    * 따라서 객체가 생성될 때 마다 생성자를 통해 인스턴스의 멤버 변수인 `count` 값을 증가시킵니다.

> 참고로 예제를 단순하게 만들기 위해 필드에 `public`을 사용했습니다.

**DataCountMain1**
```java
package static1;

public class DataCountMain1 {

  public static void main(String[] args) {
    Data1 data1 = new Data1("A");
    System.out.println("A count=" + data1.count);

    Data1 data2 = new Data1("B");
    System.out.println("B count=" + data2.count);

    Data1 data3 = new Data1("C");
    System.out.println("C count=" + data3.count);
  }
}
```
* 객체를 생성하고 카운트 값을 출력합니다.

**실행 결과**
```
A count=1
B count=1
C count=1
```
* 이 프로그램은 당연히 기대한 대로 작동하지 않습니다.
    * 객체를 생성할 때 마다 `Data1` 인스턴스는 새로 만들어집니다.
    * 그리고 인스턴스에 포함된 `count` 변수도 새로 만들어지기 때문입니다.
* 처음 `Data1("A")` 인스턴스를 생성하면 `count` 값은 `0`으로 초기화 됩니다.
    * 생성자에서 `count++`을 호출했으므로 `count`의 값은 `1`이 됩니다.
* 다음으로 `Data1("B")` 인스턴스를 생성하면 완전 새로운 인스턴스를 생성합니다.
    * 이 새로운 인스턴스의 `count` 값은 `0`으로 초기화됩니다.
    * 생성자에서 `count++`을 호출했으므로 `count`의 값은 `1`이 됩니다.
* 다음으로 `Data1("C")` 인스턴스를 생성하면 이전 인스턴스는 관계없는 새로운 인스턴스를 생성합니다.
    * 이 새로운 인스턴스의 `count` 값은 `0`으로 초기화 됩니다.
    * 생성자에서 `count++`을 호출했으므로 `count`의 값은 `1`이 됩니다.
* **"인스턴스에 사용되는 멤버 변수 `count`값은 인스턴스끼리 서로 공유되지 않습니다."**
    * 따라서 원하는 답을 구할 수 없습니다.
        * 이 문제를 해결하려면 변수를 서로 공유해야 합니다.

## 외부 인스턴스에 카운트 저장
이번에는 카운트 값을 저장하는 별도의 객체를 만들어보겠습니다.

**Counter**
```java
package static1;

public class Counter {
  public int count;
}
```

**Data2**
```java
package static1;

public class Data2 {
  public String name;

  public Data2(String name, Counter counter) {
    this.name = name;
    counter.count++;
  }
}
```

**DataCountMain2**
```java
package static1;

public class DataCountMain2 {

  public static void main(String[] args) {
    Counter counter = new Counter();
    Data2 data1 = new Data2("A", counter);
    System.out.println("A count=" + counter.count);

    Data2 data2 = new Data2("B", counter);
    System.out.println("B count=" + counter.count);

    Data2 data3 = new Data2("C", counter);
    System.out.println("C count=" + counter.count);
  }
}
```

**실행 결과**
```
A count=1
B count=2
C count=3
```

* `Counter` 인스턴스를 공용으로 사용한 덕분에 객체를 생성할 때 마다 값을 정확하게 증가시킬 수 있습니다.
* `Data2("A")` 인스턴스를 생성하면 생성자를 통해 `Counter` 인스턴스에 있는 `count` 값을 하나 증가시킵니다.
    * `count` 값은 `1`이 됩니다.
* `Data2("B")` 인스턴스를 생성하면 생성자를 통해 `Counter` 인스턴스에 있는 `count` 값을 하나 증가시킵니다.
    * `count` 값은 `2`가 됩니다.
* `Data2("C")` 인스턴스를 생성하면 생성자를 통해 `Counter` 인스턴스에 있는 `count` 값을 하나 증가시킵니다.
    * `count` 값은 `3`이 됩니다.
* 결과적으로 `Data2`의 인스턴스가 3개 생성되고, `count` 값도 인스턴스 숫자와 같은 3으로 정확하게 측정됩니다.
    * 그런데 여기에는 약간 불편한 점들이 있습니다.
        * `Data2` 클래스와 관련된 일인데, `Counter`라는 별도의 클래스를 추가로 사용해야 합니다.
        * 생성자의 매개변수도 추가되고, 생성자가 복잡해집니다. 생성자를 호출하는 부분도 복잡해집니다.
