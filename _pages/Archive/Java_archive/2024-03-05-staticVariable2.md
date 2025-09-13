---
title: "☕️[Java] static 변수2"
tags:
    - Java
    - Programming Language
date: "2024-03-05"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# static 변수 2

## static 변수 사용

특정 클래스에서 공용으로 함께 사용할 수 있는 변수를 만들 수 있다면 편리할 것입니다.
* `static` 키워드를 사용하면 공용으로 함께 사용하는 변수를 만들 수 있습니다

**Data3**
```java
package static1;

public class Data3 {
  public String name;
  public static int count; // static

  public Data3(String name) {
    this.name = name;
    count++;
  }
}
```

* 기존 코드를 유지하기 위해 새로운 클래스 `Data3`을 만들었습니다.
* `static int count` 부분을 봐봅시다.
    * 변수 타입(`int`) 앞에 `static` 키워드가 붙어있습니다.
* 이렇게 멤버 변수에 `static`을 붙이게 되면 `static` 변수, 정적 변수 또는 클래스 변수라 합니다.
* 객체가 생성되면 생성자에서 정적 변수 `count`의 값을 하나 증가시킵니다.

**DataCountMain3**
```java
package static1;

public class DataCountMain3 {

  public static void main(String[] args) {
    Data3 data1 = new Data3("A");
    System.out.println("A coutn=" + Data3.count);

    Data3 data2 = new Data3("B");
    System.out.println("B coutn=" + Data3.count);

    Data3 data3 = new Data3("C");
    System.out.println("C coutn=" + Data3.count);
  }
}
```

* 코드를 보면 `count` 정적 변수에 접근하는 방법이 조금 특이한데 `Data3.count` 와 같이 클래스명에 `.(dot)`을 사용합니다.
    * 마치 클래스에 직접 접근하는 것 처럼 느껴집니다.

**실행 결과**
```
A coutn=1
B coutn=2
C coutn=3
```

* `static` 이 붙은 멤버 변수는 메서드 영역에서 관리합니다.
    * `static` 이 붙은 멤버 변수 `count`는 인스턴스 영역에 생성되지 않습니다. 대신 메서드 영역에서 이 변수를 관리합니다.
* `Data3("A")` 인스턴스를 생성하면 생성자가 호출됩니다.
* 생성자에는 `count++` 코드가 있습니다. `count`는 `static`이 붙은 정적 변수입니다. 정적 변수는 인스턴스 영역이 아니라 메서드 영역에서 관리합니다. 따라서 이 경우 메서드 영역에 있는 `count`의 값이 하나 증가됩니다.
* `Data3("B")` 인스턴스를 생성하면 생성자가 호출됩니다.
* `count++` 코드가 있습니다.
    * `count`는 `static`이 붙은 정적 변수입니다
    * 메서드 영역에 있는 `count` 변수의 값이 하나 증가됩니다.
* `Data3("C")` 인스턴스를 생성하면 생성자가 호출됩니다.
* `count++` 코드가 있습니다. `count`는 `static`이 붙은 정적 변수입니다.
    * 메서드 영역에 있는 `count` 변수의 값이 하나 증가됩니다.

최종적으로 메서드 영역에 있는 `count` 변수의 값은 3이 됩니다.
* `static`이 붙은 정적 변수에 접근하려면 `Data3.count`와 같이 클래스명 + `.(dot)` + 변수명으로 접근하면 됩니다.

> 참고로 `Data3`의 생성자와 같이 자신의 클래스에 있는 정적 변수라면 클래스 명을 생략할 수 있습니다.

`static` 변수를 사용한 덕분에 공용 변수를 사용해서 편리하게 문제를 해결할 수 있었습니다.

## 정리
* `static` 변수는 쉽게 이야기해서 클래스인 붕어빵 틀이 특별히 관리하는 변수입니다.
* 붕어빨 틀은 1개이므로 클래스 변수도 하나만 존재합니다.
* 반면에 인스턴스인 붕어빵은 인스턴스의 수 만큼 변수가 존재합니다.
