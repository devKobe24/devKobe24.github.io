---
title: "☕️[Java] static 메서드 1"
tags:
    - Java
    - Programming Language
date: "2024-03-05"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# static 메서드 1
이번에는 `static`이 붙은 메서드에 대해 알아보겠습니다.
이해를 돕기 위해 간단한 예제를 만들어보겠습니다.

특정 문자열을 꾸며주는 간단한 기능을 만들어보겠습니다.
예를 들어서 `"hello"`라는 문자를 꾸미면 앞 뒤에 `*`을 붙여서 `*hello*`와 같이 꾸며주는 기능입니다.

## 인스턴스 메서드

**DecoUtil1**
```java
package static2;

public class DecoUtil1 {

  public String deco(String str) {
    String result = "*" + str + "*"
    return result;
  }
}
```
* `deco()`는 문자열을 꾸미는 기능을 제공합니다.
    * 문자열이 들어오면 앞 뒤에 `*`을 붙여서 반환합니다.

**DecoMain1**
```java
package static2;

public class DecoMain1 {

  public static void main(String[] args) {
    String s = "hello java";
    DecoUtil1 utils = new DecoUtil1();
    String deco = utils.deco(s);

    System.out.println("before: " + s);
    System.out.println("after: " + deco);
  }
}
```

**실행 결과**
```
before: hello java
after: *hello java*
```

* 앞서 개발한 `deco()` 메서드를 호출하기 위해서는 `DecoUtil1`의 인스턴스를 먼저 생성해야 합니다.
    * 그런데 `deco()` 라는 기능은 멤버 변수도 없고, 단순히 기능만 제공할 뿐입니다.
        * 인스턴스가 필요한 이유는 멤버 변수(인스턴스 변수)등을 사용하는 목적이 큰데, 이 메서드는 사용하는 인스턴스 변수도 없고 단순히 기능만 제공합니다.

## static 메서드
먼저 예제를 만들어서 실행해봅시다.

**DecoUtil2**
```java
package static2;

public class DecoUtil2 {

  public static String deco(String str) {
    String result = "*" + str + "*";
    return result;
  }
}
```

* `DecoUtil2`는 앞선 예제와 비슷합니다.
    * 하지만 메서드 앞에 `static`이 붙어있습니다. 이 부분에 주의합시다.
        * 이렇게 하면 정적 메서드를 만들 수 있습니다.
            * 그리고 이 정적 메서드는 정적 변수처럼 인스턴스 생성 없이 클래스 명을 통해서 바로 호출할 수 있습니다.

**DecoMain2**
```java
package static2;

public class DecoMain2 {

  public static void main(String[] args) {
    String s = "hello java";
    String deco = DecoUtil2.deco(s);

    System.out.println("before: " + s);
    System.out.println("after: " + deco);
  }
}
```

**실행 결과**
```
before: hello java
after: *hello java*
```

* `DecoUtil2.deco(s);` 코드를 봅시다.
    * `static`이 붙은 정적 메서드는 객체 생성 없이 클래스명 + `.(dot)` + 메서드 명으로 바로 호출할 수 있습니다.
        * 정적 메서드 덕분에 불필요한 객체 생성 없이 편리하게 메서드를 사용했습니다.

**클래스 메서드**
* 메서드 앞에서 `static`을 붙일 수 있습니다.
    * 이것을 **정적 메서드** 또는 **클래스 메서드**라 합니다.
        * 정적 메서드라는 용어는 `static`이 정적이라는 뜻이기 때문입니다.
        * 클래스 메서드라는 용어는 인스턴스 생성 없이 마치 클래스에 있는 메서드를 바로 호출하는 것 처럼 느껴지기 때문입니다.

**인스턴스 메서드**
* `static`이 붙지 않은 메서드는 인스턴스를 생성해야 호출할 수 있습니다.
    * 이것을 **인스턴스 메서드**라 합니다.
