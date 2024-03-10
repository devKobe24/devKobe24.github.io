---
title: "☕️[Java] 상속과 접근 제어"
tags:
    - Java
    - Programming Language
date: "2024-03-10"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 상속과 접근 제어.

상속 관계와 접근 제어에 대해 알아보겠습니다.

> 참고로 접근 제어를 자세히 설명하기 위해 부모와 자식의 패키지를 따로 분리하였습니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A9%E1%86%A8%E1%84%80%E1%85%AA%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%80%E1%85%B3%E1%86%AB%E1%84%8C%E1%85%A6%E1%84%8B%E1%85%A5UML.png?raw=true">

* 접근 제어자를 표현하기 위해 UML 표기법을 일부 사용했습니다.
    * `+`: public
    * `#`: protected
    * `~`: default
    * `-`: private

접근 제어자를 잠시 복습해봅시다.

### 접근 제어자의 종류
* `private`: 모든 외부 호출을 막습니다.
* `default(package-private)`: 같은 패키지 안에서 호출은 허용합니다.
* `protected`: 같은 패키지 안에서 호출은 허용합니다. 
    * 패키지가 달라도 상속 관계의 호출은 허용합니다.
* `public`: 모든 외부 호출을 허용합니다.

순서대로 `private`이 가장 많이 차단하고, `public`이 가장 많이 허용합니다.
* `private -> default -> protected -> public`

그림과 같이 다양한 접근 제어자를 사용하도록 코드를 작성해보겠습니다.

**Parent**
```java
package extends1.access.parent;

public class Parent {

  public int publicValue;
  protected int protectedValue;
  int defaultValue;
  private int privateValue;

  public void publicMethod() {
    System.out.println("Parent.publicMethod");
  }

  protected void protectedMethod() {
    System.out.println("Parent.protectedMethod");
  }

  void defaultMethod() {
    System.out.println("Parent.defaultMethod");
  }

  private void privateMethod() {
    System.out.println("Parent.privateMethod");
  }

  public void printParent() {
    System.out.println("==Parent 메서드 안==");
    System.out.println("publicValue = " + publicValue);
    System.out.println("protectedValue = " + protectedValue);
    System.out.println("defaultValue = " + defaultValue);
    System.out.println("privateValue = " + privateValue);


    // 부모 메서드 안에서 모두 접근 가능
    defaultMethod();
    privateMethod();
  }
}
```

**Child**
```java
package extends1.access.child;

import extends1.access.parent.Parent;

public class Child extends Parent {

  public void call() {
    publicValue = 1;
    protectedValue = 1; // 상속 관계 or 같은 패키지
//    defaultValue = 1; // 다른 패키지 접근 불가, 컴파일 오류
//    privateValue = 1; // 접근 불가, 컴파일 오류

    publicMethod();
    protectedMethod(); // 상속 관계 or 같은 패키지
//    defaultMethod(); // 다른 패키지 접근 불가, 컴파일 오류
//    privateMethod(); // 접근 불가, 컴파일 오류

    printParent();
  }
}
```

**ExtendsAccessMain**
```java
package extends1.access;

import extends1.access.child.Child;

public class ExtendsAccessMain {

  public static void main(String[] args) {
    Child child = new Child();
    child.call();
  }
}
```

**Parent와 Child의 패키지가 다르다는 부분에 유의합시다.**

자식 클래스인 `Child`에서 부모 클래스인 `Parent`에 얼마나 접근할 수 있는지 확인해봅시다.
* `publicValue = 1;`: 부모의 `public` 필드에 접근합니다. `public`이므로 접근할 수 있습니다.
* `protectedValue = 1;`: 부모의 `protected` 필드에 접근합니다. 자식과 부모는 다른 패키지이지만, **상속 관계이므로 접근할 수 있습니다.**
* `defaultValue = 1;`: 부모의 `default` 필드에 접근합니다. 자식과 부모가 다른 패키지이므로 접근할 수 없습니다.
* `privateValue = 1;`: 부모의 `private` 필드에 접근합니다. `private`은 모두 외부 접근을 막으므로 자식이라도 호출할 수 없습니다.

메서드의 경우도 앞서 설명한 필드와 동일합니다.

**Child**
```java
package extends1.access.child;

import extends1.access.parent.Parent;

public class Child extends Parent {

  public void call() {
    publicValue = 1;
    protectedValue = 1; // 상속 관계 or 같은 패키지
//    defaultValue = 1; // 다른 패키지 접근 불가, 컴파일 오류
//    privateValue = 1; // 접근 불가, 컴파일 오류

    publicMethod();
    protectedMethod(); // 상속 관계 or 같은 패키지
//    defaultMethod(); // 다른 패키지 접근 불가, 컴파일 오류
//    privateMethod(); // 접근 불가, 컴파일 오류

    printParent();
  }
}
```

* 코드를 실행해보면 `Child.call()` -> `Parent.printParent()` 순서로 호출합니다.
* `Child`는 부모의 `public`, `protexted` 필드나 메서드만 접근할 수 있습니다.
    * 반면에 `Parent.printParent()`의 경우 `Parent` 안에 있는 메서드이기 때문에 `Parent` 자신의 모든 필드와 메서드에 얼마든지 접근할 수 있습니다.

### 접근 제어와 메모리 구조.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%A1%E1%86%BC%E1%84%89%E1%85%A9%E1%86%A8%E1%84%80%E1%85%AA%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%80%E1%85%B3%E1%86%AB%E1%84%8C%E1%85%A6%E1%84%8B%E1%85%A5-%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%80%E1%85%B3%E1%86%AB%E1%84%8C%E1%85%A6%E1%84%8B%E1%85%A5%E1%84%8B%E1%85%AA%E1%84%86%E1%85%A6%E1%84%86%E1%85%A9%E1%84%85%E1%85%B5%E1%84%80%E1%85%AE%E1%84%8C%E1%85%A9.png?raw=true">

* 본인 타입에 없으면 부모 타입에서 기능을 찾는데, 이때 접근 제어자가 영향을 줍니다.
    * 왜냐하면 객체 내부에서는 자식과 부모가 구분되어 있기 때문입니다.
        * 결국 자식 차입에서 부모 타입의 기능을 호출할 때, 부모 입장에서 보면 외부에서 호출한 것과 같습니다.
