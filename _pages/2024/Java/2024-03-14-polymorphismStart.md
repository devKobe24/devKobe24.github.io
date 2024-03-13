---
title: "☕️[Java] 다형성(Polymorphism) 시작"
tags:
    - Java
    - Programming Language
date: "2024-03-14"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 다형성(Polymorphism) 시작.

**"객체지향 프로그래밍의 대표적인 특징으로는 캡슐화, 상속, 다형성이 있습니다."**
* 그 중에서 **"다형성"** 은 객체지향 프로그래밍의 **꽃**이라고 불립니다.

앞서 학습한 캡슐화나 상속은 직관적으로 이해하기 쉽습니다.
* 반면에 **"다형성은 제대로 이해하기도 어렵고, 잘 활용하기는 더 어렵습니다."**
    * 하지만 좋은 개발자기 되기 위해서는 **"다형성에 대한 이해가 필수"** 입니다.

**"다형성(Polymorphism)"** 은 이름 그대로 **"다양한 형태", "여러 형태"** 를 뜻합니다.
* 프로그래밍에서 **"다형성은 한 객체가 여러 타입의 객체로 취급될 수 있는 능력을 뜻합니다"**

보통 하나의 객체는 하나의 타입으로 고정되어 있습니다.
* 그런데 **"다형성을 사용하면 하나의 객체가 다른 타입으로 사용될 수 있다는 뜻입니다."**

## 본격적인 다형성 학습.

다형성을 이해하기 위해서는 크게 **"2가지 핵심 이론"** 을 알아야 합니다.
1. **다형적 참조**
2. **메서드 오버라이딩**

먼저 "다형적 참조"라 불리는 개념에 대해 알아봅시다.

### 다형적 참조.

다형적 참조를 이해하기 위해 다음과 같은 간단한 상속 관계를 코드로 만들어보겠습니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%E1%84%8E%E1%85%A1%E1%86%B7%E1%84%8C%E1%85%A9UML.png?raw=true" width=163 height=254>

* 부모와 자식이 있고, 각각 다른 메서드를 가집니다.

**Parent**
```java
package poly.basic;

public class Parent {

  public void parentMethod() {
    System.out.println("Parent.parentMethod");
  }
}
```

**Child**
```java
package poly.basic;

public class Child extends Parent {

  public void childMethod() {
    System.out.println("Child.childMethod");
  }
}
```

**PolyMain**
```java
/*
 * 다형적 참조: 부모는 자식을 품을 수 있다. 
 */

package poly.basic;

public class PolyMain {

  public static void main(String[] args) {
    // 부모 변수가 부모 인스턴스 참조
    System.out.println("Parent -> Parent");
    Parent parent = new Parent();
    parent.parentMethod();

    // 자식 변수가 자식 인스턴스 참조
    System.out.println("Child -> Child");
    Child child = new Child();
    child.parentMethod();
    child.childMethod();

    // 부모 변수가 자신 인스턴스 참조(다형적 참조)
    System.out.println("Parent -> Child");
    Parent poly = new Child();
    poly.parentMethod();
  }
}
```

```
Parent -> Parent
Parent.parentMethod
Child -> Child
Parent.parentMethod
Child.childMethod
Parent -> Child
Parent.parentMethod
```

그림을 통해 코드를 하나씩 분석해봅시다.

**부모 타입의 변수가 부모 인스턴스 참조**
<img src="https://github.com/devKobe24/images/blob/main/%E1%84%87%E1%85%AE%E1%84%86%E1%85%A9%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%8B%E1%85%B4%E1%84%87%E1%85%A7%E1%86%AB%E1%84%89%E1%85%AE%E1%84%80%E1%85%A1%E1%84%87%E1%85%AE%E1%84%86%E1%85%A9%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%89%E1%85%B3%E1%84%90%E1%85%A5%E1%86%AB%E1%84%89%E1%85%B3%E1%84%8E%E1%85%A1%E1%86%B7%E1%84%8C%E1%85%A9.png?raw=true">

* 부모 타입의 변수가 부모 인스턴스를 참조합니다.
* `Parent parent = new Parent()`
* `Parent` 인스턴스를 만들었습니다. 이 경우 부모 타입인 `Parent`를 생성했기 때문에 메모리 상에 `Parent`만 생성됩니다.(자식은 생성되지 않습니다.)
* 생성된 참조값을 `Parent` 타입의 변수인 `parent`에 담아둡빈다.
* `parent.parentMethod()`를 호출하면 인스턴스의 `Parent` 클래스에 있는 `parentMethod()`가 호출됩니다.

**자식 타입의 변수가 자식 인스턴스 참조**
<img src="https://github.com/devKobe24/images/blob/main/%E1%84%8C%E1%85%A1%E1%84%89%E1%85%B5%E1%86%A8%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%8B%E1%85%B4%E1%84%87%E1%85%A7%E1%86%AB%E1%84%89%E1%85%AE%E1%84%80%E1%85%A1%E1%84%8C%E1%85%A1%E1%84%89%E1%85%B5%E1%86%A8%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%89%E1%85%B3%E1%84%90%E1%85%A5%E1%86%AB%E1%84%89%E1%85%B3%E1%84%8E%E1%85%A1%E1%86%B7%E1%84%8C%E1%85%A9.png?raw=true">

* 자식 타입의 변수가 자식 인스턴스를 참조합니다.
* `Child child = new Child()`
* `Child` 인스턴스를 만들었습니다. 이 경우 자식 타입인 `Child`를 생성했기 때문에 메모리 상에 `Child`와 `Parent`가 모두 생성 됩니다.
* 생성된 참조값을 `Child` 타입의 변수인 `child`에 답아둡니다.
* `child.childMethod()`를 호출하면 인스턴스의 `Child` 클래스에 있는 `childMethod()`가 호출됩니다.

여기까지는 지금까지 배운 내용이므로 이해하는데 어려움은 없을 것입니다. 이제부터가 중요합니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%E1%84%8E%E1%85%A1%E1%86%B7%E1%84%8C%E1%85%A9%E1%84%87%E1%85%AE%E1%84%86%E1%85%A9%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%8B%E1%85%B4%E1%84%87%E1%85%A7%E1%86%AB%E1%84%89%E1%85%AE%E1%84%80%E1%85%A1%E1%84%8C%E1%85%A1%E1%84%89%E1%85%B5%E1%86%A8%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%89%E1%85%B3%E1%84%90%E1%85%A5%E1%86%AB%E1%84%89%E1%85%B3%E1%84%8E%E1%85%A1%E1%86%B7%E1%84%8C%E1%85%A9.png?raw=true">

* 부모 타입의 변수가 자식 인스턴스를 참조합니다.
* `Parent poly = new Child()`
* `Child` 인스턴스를 만들었습니다. 이 경우 자식 타입인 `Child`를 생성했기 때문에 메모리 상에 `Child`와 `Parent`가 모두 생성됩니다.
* 생성된 참조값을 `Parent` 타입의 변수인 `poly`에 담아둡니다.

**부모는 자식을 담을 수 있습니다.**
* 부모 타입은 자식 타입을 담을 수 있습니다.
* `Parent poly`는 부모 타입입니다. `new Child()`를 통해 생성된 결과는 `Child` 타입입니다. 자바에서 부모 타입은 자식 타입을 담을 수 있습니다.
    * `Parent poly = new Child()` : 성공
* 반대로 자식 타입은 부모 타입을 담을 수 없습니다.
    * `Child child1 = new Parent()` : 컴파일 오류 발생

**다형적 참조**
지금까지 학습한 내용을 떠올려보면 항상 같은 타입에 참조를 대입했습니다.
그래서 보통 한 가지 형태만 참조할 수 있습니다.

* `Parent parent = new Parent()`
* `Child child = new Child()`

그런데 `Parent` 타입의 변수는 다음과 같이 자신인 `Parent`는 물론이고, 자식 타입까지 참조할 수 있습니다. 만약 손자가 있다면 손자도 그 하위 타입도 참조할 수 있습니다.

* `Parent poly = new Parent()`
* `Parent poly = new Child()`
* `Parent poly = new Grandson()` : Child 하위에 손자가 있다면 가능

자바에서 부모 타입은 물론이고, 자신을 기준으로 모든 자식 타입을 참조할 수 있습니다.
**"이것을 바로 다양한 형태를 탐조할수 있다고 해서 다형적 참조하고 합니다."**

**다형적 참조와 인스턴스 실행**
앞의 그림을 참고합시다.
* `poly.parentMethod()`를 호출하면 먼저 참조값을 사용해서 인스턴스를 찾습니다.
    * 그리고 다음으로 인스턴스 안에서 실행할 타입도 찾아야 합니다.
* `poly`는 `Parent` 타입입니다.
    * 따라서 `Parent` 클래스부터 시작해서 필요한 기능을 찾습니다.
* 인스턴스의 `Parent` 클래스에 `parentMethod()`가 있습니다.
    * 따라서 해당 메스드가 호출됩니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%E1%84%8E%E1%85%A1%E1%86%B7%E1%84%8C%E1%85%A9%E1%84%8B%E1%85%B4%E1%84%92%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A8.png?raw=true">

`Parent poly = new Child()` 이렇게 자식을 참조한 상황에서 `poly`가 자식 타입인 `Child`에 있는 `childMethod()`를 호출하면 어떻게 될까요?
* `poly.childMethod()`를 실행하면 먼저 참조값을 통해 인스턴스를 찾습니다.
    * 그리고 다음으로 인스턴스 안에서 실행할 타입을 찾아야 합니다.
        * 호출자인 `poly`는 `Parent`타입입니다. 
            * 따라서 `Parent` 클래스부터 시작해서 필요한 기능을 찾습니다.
**"그런데 상속 관계는 부코 방향으로 찾아 올라갈 수는 있지만 자식 방향으로 찾아 내려갈 수는 없습니다."**
* `Parent`는 부모 타입이고 상위에 부모가 없습니다.
    * 따라서 `childMethod()`를 찾을 수 없으므로 컴파일 오류가 발생합니다.

이런경우 `childMethod()`를 호출하고 싶으면 어떻게 해야할까요?
* **"바로 캐스팅이 필요합니다."**

**다형적 참조의 핵심은 부모는 자식을 품을 수 있다는 것입니다.**
그런데 이런 **"다형적 참조가 왜 필요하지?"** 라는 의문이 들 수 있습니다.
* 이 부분은 다형성의 다른 이론들도 함께 알아야 이해할 수 있습니다.
    * 지금은 우선 다형성의 문법과 이론을 익히는데 집중합시다.
