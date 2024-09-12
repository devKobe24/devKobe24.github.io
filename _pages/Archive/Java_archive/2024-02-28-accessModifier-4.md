---
title: "☕️[Java] 접근 제어자의 사용 - 필드, 메서드"
tags:
    - Java
    - Programming Language
date: "2024-02-28"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 접근 제어자의 사용 - 필드, 메서드
다양한 상황에 따른 접근 제어자를 확인해봅시다.
**"주의! 지금부터는 패키지 위치가 매우 중요합니다. 패키지 위치에 주의하세요."**

## 필드, 메서드 레벨의 접근 제어자
**AccessData**
```java
package access.a;

public class AccessData {

  public int publicField;
  int defaultField;
  private int privateField;

  public void publicMethod() {
    System.out.println("publicMethod 호출 " + publicField);
  }

  void defaultMethod() {
    System.out.println("defaultMethod 호출 " + defaultField);
  }

  private void privateMethod() {
    System.out.println("privateMethod 호출 " + privateField);
  }

  public void innerAccess() {
    System.out.println("내부 호출");
    publicField = 100;
    defaultField = 200;
    privateField = 300;
    publicMethod();
    defaultMethod();
    privateMethod();
  }
}
```

* 패키지 위치는 `package access.a`입니다. 패키지 위치를 꼭 맞추어야 합니다. 주의합시다.
* 순서대로 `public`, `default`, `private`을 필드와 메서드에 사용했습니다.
* 마지막에 `innerAccess()`가 있는데, 이 메서드는 내부 호출을 보여줍니다.
    * 내부 호출은 자기 자신에게 접근하는 것입니다.
        * 따라서 `private`을 포함함 모든 곳에 접근할 수 있습니다.

이제 외부에서 이 클래스에 접근해 봅시다.

**AccessInnerMain**
```java
package access.a;

public class AccessInnerMain {

  public static void main(String[] args) {
    AccessData data = new AccessData();
    // public 호출 가능
    data.publicField = 1;
    data.publicMethod();

    // 같은 패키지 default 호출 가능
    data.defaultField = 2;
    data.defaultMethod();

    // private 호출 불가
    //data.privateField = 3;
    //data.privateMethod();

    data.innerAccess();
  }
}
```
* 패키지 위치는 `package access.a`입니다. 패키지 위치를 꼭 맞춰야 합니다. 주의합시다.
* `public`은 모든 접근을 허용하기 때문에 필드, 메서드 모두 접근 가능합니다.
* `defaul`는 같은 패키지에서 접근할 수 있습니다. `AccessInnerMain`은 `AccessData`와 같은 패키지입니다.
    * 따라서 `default` 접근 제어자에 접근할 수 있습니다.
* `private`은 `AccessData` 내분에서만 접근할 수 있습니다. 
    * 따라서 호출 불가입니다.
* `AccessData.innerAccess()` 메서드는 `public`입니다.
    * 따라서 외부에서 호출할 수 있습니다.
        * `innerAccess()` 메서드는 외부에서 호출되었지만 `innerAccess()` 메서드는 `AccessData`에 포함되어 있습니다.
            * 이 메서드는 자신의 `private` 필드와 메서드에 모두 접근할 수 있습니다.

**실행 결과**
```
publicMethod 호출 1
defaultMethod 호출 2
내부 호출
publicMethod 호출 100
defaultMethod 호출 200
privateMethod 호출 300
```

**AccessOuterMain**
```java
package access.b;

import access.a.AccessData;

public class AccessOuterMain {
  public static void main(String[] args) {
    AccessData data = new AccessData();
    // public 호출 가능
    data.publicField = 1;
    data.publicMethod();

    // 다른 패키지 default 호출 가능
    //data.defaultField = 2;
    //data.defaultMethod();

    // private 호출 불가
    //data.privateField = 3;
    //data.privateMethod();

    data.innerAccess();
  }
}
```
* 패키지 위치는 `package access.b` 입니다. 패키지 위치를 꼭 맞추어야 합니다. 주의합시다.
* `public`은 모든 접근을 허용하기 때문에 필드, 메서드 모두 접근할 수 있습니다.
* `default`는 같은 패키지에서 접근할 수 있습니다.
    * `access.b.AccessOuterMain`은 `access.a.AccessData`와 다른 패키지 입니다.
        * 따라서 `default` 접근 제어자에 접근할 수 없습니다.
* `private`은 `AccessData` 내부에서만 접근할 수 있습니다. 따라서 호출 불가입니다.
* `AccessData.innerAccess()` 메서드는 `public`입니다.
    * 따라서 외부에서 호출할 수 있습니다.
        * `innerAccess()` 메서드는 외부에서 호출되었지만 해당 메서드 안에서는 자신의 `private` 필드와 메서드에 접근할 수 있습니다.

**실행 결과**
```
publicMethod 호출 1
내부 호출
publicMethod 호출 100
defaultMethod 호출 200
privateMethod 호출 300
```

> 참고로 생성자도 접근 제어자 관점에서 메서드와 같습니다.
