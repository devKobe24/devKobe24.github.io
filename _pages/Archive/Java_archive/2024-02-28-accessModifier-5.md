---
title: "☕️[Java] 접근 제어자의 사용 - 클래스 레벨"
tags:
    - Java
    - Programming Language
date: "2024-02-28"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 접근 제어자의 사용 - 클래스 레벨
**클래스 레벨의 접근 제어자 규칙**
* 클래스 레벨의 접근 제어자는 `public`, `default`만 사용할 수 있습니다.
    * `private`, `protected`는 사용할 수 없습니다.
* `public` 클래스는 반드시 파일명과 이름이 같아야 합니다.
    * 하나의 자바 파일에 `public` 클래스는 하나만 등장할 수 있습니다.
    * 하나의 자바 파일에 `default` 접근 제어자를 사용하는 클래스는 무한정 만들 수 있습니다.

**PublicClass.java 파일**
```java
package access.a;

public class PublicClass {
    pulbic static void main(String[] args) {
        PublicClass publicClass = new PublicClass();
        DefaultClass1 class1 = new DefaultClass1();
        DefaultClass2 class2 = new DefaultClass2();
    }
}

class DefaultClass1 {
}

class DefaultClass2 {
}
```
* 패키지 위치는 `package access.a`입니다. 패키지 위치를 꼭 맞추어야 합니다. 주의합시다.
* `PublicClass`라는 이름의 클래스를 만들었습니다. 이 클래스는 `public` 접근 제어자입니다.
    * 따라서 파일명과 이 클래스의 이름이 반드시 같아야 합니다.
        * 이 클래스는 `public`이기 때문에 외부에서 접근할 수 있습니다.
* `DefaultClass1`, `DefaultClass2`는 `default` 접근 제어자입니다.
    * 이 클래스는 `default`이기 때문에 같은 패키지 내부에서만 접근할 수 있습니다.
* `PublicClass`의 `main()`을 보면 각각의 클래스를 사용하는 예를 보여줍니다.
    * `PublicClass`의 `main()`을 보면 각각의 클래스를 사용하는 예를 보여줍니다.
        * `PublicClass`는 `public` 접근 제어입니다.
            * 따라서 어디서든 사용할 수 있습니다.
                * `DefaultClass1`, `DefaultClass2`와는 같은 패키지에 있으므로 사용할 수 있습니다.

**PublicClassInnerMain**
```java
package access.a;

public class PublicClassInnerMain {

  public static void main(String[] args) {
    PublicClass publicClass = new PublicClass();
    DefaultClass1 class1 = new DefaultClass1();
    DefaultClass2 class2 = new DefaultClass2();
  }
}
```
* 패키지 위치는 `package access.a`입니다. 패키지 위치를 꼭 맞춰줘야합니다.
* `PublicClass`는 `public` 클래스입니다.
    * 따라서 외부에서 접근할 수 있습니다.
* `PublicClassInnerMain`와 `DefaultClass1`, `DefaultClass2`는 같은 패키지입니다.
    * 따라서 접근할 수 있습니다.

**PublicClassOuterMain**
```java
package access.b;

//import access.a.DefaultClass1;
//import access.a.DefaultClass2;
import access.a.PublicClass;

public class PublicClassOuterMain {

  public static void main(String[] args) {
    PublicClass publicClass = new PublicClass();

    // 다른 패키지 접근 불가.
    //DefaultClass1 class1 = new DefaultClass1();
    //DefaultClass2 class2 = new DefaultClass2();
  }
}
```
* 패키지 위치는 `package accesss.b` 입니다. 패키지 위치를 꼭 맞춰야합니다.
* `PublicClass`는 `public` 클래스입니다.
    * 따라서 외부에서 접근할 수 있습니다.
* `PublicClassOuterMain`와 `DefaultClass1`, `DefaultClass2`는 다른 패키지입니다.
    * 따라서 접근할 수 없습니다.
