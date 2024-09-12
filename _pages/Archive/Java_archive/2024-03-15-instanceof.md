---
title: "☕️[Java] instanceof"
tags:
    - Java
    - Programming Language
date: "2024-03-15"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# instanceof

다형성에서 참조형 변수는 이름 그대로 다양한 지식을 대상으로 참조할 수 있습니다.
그런데 참조하는 대상이 다양하기 때문에 어떤 인스턴스를 참조하고 있는지 확인하려면 어떻게 해야할까요?

```java
Parent parent1 = new Parent();
Parent parent2 = new Child()
```

* 여기서 `Parent`는 자신과 같은 `Parent`의 인스턴스도 참조할 수 있고, 자식 타입인 `Child`의 인스턴스도 참조할 수 있습니다.
    * 이때 `parent1`, `parent2` 변수가 참조하는 **인스턴스의 타입을 확인**하고 싶다면 `instanceof` 키워드를 사용하면 됩니다.

예제를 봅시다.

```java
package poly.basic;

public class CastingMain5 {

  public static void main(String[] args) {
    Parent parent1 = new Parent();
    System.out.println("parent1 호출");
    call(parent1);

    Parent parent2 = new Child();
    System.out.println("parent2 호출");
    call(parent2);
  }

  private static void call(Parent parent) {
    parent.parentMethod();

    if (parent instanceof Child) {
      System.out.println("Child 인스턴스 맞음");
      Child child = (Child) parent;
      child.childMethod();
    }
  }
}
```

**실행 결과**
```
parent1 호출
Parent.parentMethod
parent2 호출
Parent.parentMethod
Child 인스턴스 맞음
Child.childMethod
```

`call(Parent parent)` 메서드를 봐봅시다.
* 이 메서드는 매개변수로 넘어온 `parent`가 참조하는 타입에 따라서 다른 명령을 수행합니다.
    * 참고로 지금처럼 다운캐스팅을 수행하기 전에는 먼저 `instanceof`를 사용해서 원하는 타입으로 변경이 가능한지 확인한 다음에 다운캐스팅을 수행하는 것이 안전합니다.

해당 메서드를 처음 호출할 때 `parent`는 `Parent`의 인스턴스를 참조합니다.
```java
parent instanceof Child // parent는 Parent의 인스턴스
new Parent() instanceof Child // false
```

* `parent`는 `Parent`의 인스턴스를 참조하므로 `false`를 반환합니다.

해당 메서드를 다음으로 호출할 때 `parent`는 `Child`의 인스턴스를 참조합니다.
```java
parent instanceof Child // parent는 Child의 인스턴스
new Child() instanceof Child // true
```

참고로 `instanceof` 키워드는 오른쪽 대상의 자식 타입을 왼쪽에서 참조하는 경우에도 `true`를 반환합니다.
```java
parent instanceof Parent // parent는 Child의 인스턴스

new Parent() instanceof Parent // parent가 Parent의 인스턴스를 참조하는 경우: true
new Child() instanceof Parent // parent가 Child의 인스턴스를 참조하는 경우: true
```

* 쉽게 이야기해서 오른쪽에 있는 타입에 왼쪽에 있는 인스턴스의 타입이 들어갈 수 있는지 대입해보면 됩니다.
    * 대입이 가능하면 `true`, 불가능하면 `false`가 됩니다.

```java
new Parent() instanceof Parent
Parent p = new Parent() // 같은 타입 true

new Child() instancof Parent
Parent p = new Child() // 부모는 자식을 담을 수 있다. true

new Parent() instanceof Child
Child c = new Parent() // 자식은 부모를 담을 수 없다. false

new Child() instanceof Child
Child c = new Child() // 같은 타입 true
```

## 자바 16 - Pattern Matching for instanceof

자바 16부터는 `instanceof`를 사용하면서 동시에 변수를 선언할 수 있습니다.

다음 코드를 참고합시다.

```java
package poly.basic;

public class CastingMain6 {

  public static void main(String[] args) {
    Parent parent1 = new Parent();
    System.out.println("parent1 호출");
    call(parent1);

    Parent parent2 = new Child();
    System.out.println("parent2 호출");
    call(parent2);
  }

  private static void call(Parent parent) {
    parent.parentMethod();
    // Child 인스턴스인 경우 childMethod() 실행
    if (parent instanceof Child child) {
      System.out.println("Child 인스턴스 맞음");
      child.childMethod();
    }
  }
}
```

**실행 결과**
```
parent1 호출
Parent.parentMethod
parent2 호출
Parent.parentMethod
Child 인스턴스 맞음
Child.childMethod
```

* 덕분에 인스턴스가 맞는 경우 직접 다운캐스팅 하는 코드를 생략할 수 있습니다.

