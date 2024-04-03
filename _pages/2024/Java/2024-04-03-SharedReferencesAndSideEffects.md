---
title: "☕️[Java] 공유 참조와 사이드 이펙트"
tags:
    - Java
    - Programming Language
date: "2024-04-03"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 공유 참조와 사이드 이펙트.

사이드 이펙트(Side Effect)는 프로그래밍에서 어떤 계산이 주된 작업 외에 추가적인 부수 효과를 일으키는 것을 말합니다.

앞서 `b`의 값을 부산으로 변경한 코드를 다시 분석해 봅시다.

```java
b.setValue("부산"); //b의 값을 부산으로 변경해야함
System.out.println("부산 -> b");
System.out.println("a = " + a); // 사이드 이펙트 발생
System.out.println("b = " + b);
```

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8B%E1%85%B2%E1%84%8E%E1%85%A1%E1%86%B7%E1%84%8C%E1%85%A9%E1%84%8B%E1%85%AA%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%83%E1%85%B3%E1%84%8B%E1%85%B5%E1%84%91%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3-1.png?raw=true">

- 개발자는 `b`의 주소값을 서울에서 부산으로 변경할 의도로 값 변경을 시도했습니다.
- 하지만 `a`, `b`는 같은 인스턴스를 참조합니다. 따라서 `a`의 값도 함께 부산으로 변경되어 버립니다.

이렇게 주된 작업 외에 추가적인 부수 효과를 일으키는 것을 사이드 이펙트라고 합니다.
프로그래밍에서 사이드 이펙트는 보통 부정적인 의미로 사용되는데, 사이드 이펙트는 프로그램의 특정 부분에서 발생한 변경이 의도치 않게 다른 부분에 영향을 미치는 경우에 발생합니다.
이로 인해 디버깅이 어려워지고 코드의 안정성이 저하될 수 있습니다.

## 사이드 이펙트 해결방안
생각해보면 문제의 해결방안은 아주 단순합니다.

다음과 같이 `a`와 `b`가 처음부터 서로 다른 인스턴스를 참조하면 됩니다.
```java
Address a = new Address("서울");
Address b = new Address("서울");
```

코드를 작성해봅시다.

```java
package lang.immutable.address;

public class RefMain1_2 {

  public static void main(String[] args) {
    // 참조형 변수는 하나의 인스턴스를 공유할 수 있습니다.
    Address a = new Address("서울");
    Address b = new Address("서울");

    System.out.println("a = " + a);
    System.out.println("b = " + b);

    b.setValue("부산");
    System.out.println("부산 -> b");
    System.out.println("a = " + a);
    System.out.println("b = " + b);
  }
}
```

**실행 결과**
```
a = Address{value='서울'}
b = Address{value='서울'}
부산 -> b
a = Address{value='서울'}
b = Address{value='부산'}
```

실행 결과를 보면 `b`의 주소값만 부산으로 변경된 것을 확인할 수 있습니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8B%E1%85%B2%E1%84%8E%E1%85%A1%E1%86%B7%E1%84%8C%E1%85%A9%E1%84%8B%E1%85%AA%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%83%E1%85%B3%E1%84%8B%E1%85%B5%E1%84%91%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3-2.png?raw=true">

- `a`와 `b`는 서로 다른 `Address` 인스턴스를 참조합니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8B%E1%85%B2%E1%84%8E%E1%85%A1%E1%86%B7%E1%84%8C%E1%85%A9%E1%84%8B%E1%85%AA%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%83%E1%85%B3%E1%84%8B%E1%85%B5%E1%84%91%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3-3.png?raw=true">

- `a`와 `b`는 서로 다른 인스턴스를 참조합니다.
    - 따라서 `b`가 참조하는 인스턴스의 값을 변경해도 `a`에는 영향을 주지 않습니다.

## 여러 변수가 하나의 객체를 공유하는 것을 막을 방법은 없다
지금까지 발생한 모든 문제는 같은 객체(인스턴스)를 변수 `a`, `b`가 함께 공유하기 때문에 발생했습니다.
- 따라서 객체를 공유하지 않으면 문제가 해결됩니다.
- 여기서 변수 `a`,`b`가 서로 각각 다른 주소지로 변경할 수 있어야 합니다.
    - 이렇게 하려면 서로 다른 객체를 참조하면 됩니다.

**객체를 공유**
```java
Address a = new Address("서울");
Address b = a;
```
- 이 경우 `a`, `b` 둘 다 같은 `Address` 인스턴스를 바라보기 때문에 한쪽의 주소만 부산으로 변경하는 것이 불가능합니다.

**객체를 공유 하지 않음**
```java
Address a = new Address("서울");
Address b = new Address("서울");
```
- 이 경우 `a`, `b`는 서로 다른 `Address` 인스턴스를 바라보기 때문에 한쪽의 주소만 부산으로 변경하는 것이 가능합니다.

이처럼 단순하게 서로 다른 객체를 참조해서, 같은 객체를 공유하지 않으면 문제가 해결됩니다.

쉽게 이야기해서 여러 변수가 하나의 객체를 공유하지 않으면 지금까지 설명한 문제들이 발생하지 않습니다.
- 그런데 여기에 문제가 있습니다.
    - 하나의 객체를 여러 변수가 공유하지 않도록 강제로 막을 수 있는 방법이 없다는 것입니다

다음 예시를 봅시다.

**참조값의 공유를 막을 수 있는 방법이 없습니다.**
```java
Address a = new Address("서울");
Address b = a; // 참조값 대입을 막을 수 있는 방법이 없습니다.
```

`b = a`와 같은 코드를 작성하지 않도록 해서, 여러 변수가 하나의 참조값을 공유하지 않으면 문제가 해결될 것 같습니다.
- 하지만 `Address`를 사용하는 개발자 입장에서 실수로 `b = a`라고 해도 아무런 오류가 발생하지 않습니다.
    - 왜냐하면 자바 문법상 `Address b = a`와 같은 참조형 변수의 대입은 아무런 문제가 없기 때문입니다.

다음과 같이 새로운 객체를 참조형 변수에 대입하든, 또는 기존 객체를 참조형 변수에 대입하든, 다음 두 코드 모두 자바 문법상 정상인 코드입니다.
```java
Address b = new Address("서울"); // 새로운 객체 참조
Address b = a // 기존 객체 공유 참조
```
참조값을 다른 변수에 대입하는 순간 여러 변수가 하나의 객체를 공유하게 됩니다.
- 쉽게 이야기해서 **객체의 공유를 막을 수 있는 방법이 없습니다!**

기본형은 항상 값을 복사해서 대입하기 때문에 값이 절대로 공유되지 않습니다.
- 하지만 참조형의 경우 참조값을 복사해서 대입하기 때문에 여러 변수에서 얼마든지 같은 객체를 공유할 수 있습니다.
- 객체의 공유가 꼭 필요할 때도 있지만, 때로는 공유하는 것이 지금과 같은 사이드 이펙트를 만드는 경우도 있습니다.

물론 개발자가 눈을 크게 잘 뜨고! 집중해서 코드를 잘 작성하면서 사이드 이펙트 문제를 일으키지 않을 수 있습니다.
- 하지만 실제로는 훨씬 더 복잡한 상황에서 이런 문제가 발생합니다.

다음 코드를 봅시다.

```java
package lang.immutable.address;

public class RefMain1_3 {

  public static void main(String[] args) {
    // 참조형 변수는 하나의 인스턴스를 공유할 수 있습니다.
    Address a = new Address("서울");
    Address b = a;

    System.out.println("a = " + a);
    System.out.println("b = " + b);

    change(b, "부산");
    System.out.println("a = " + a);
    System.out.println("b = " + b);
  }

  private static void change(Address address, String changeAddress) {
    System.out.println("주소 값을 변경합니다 -> " + changeAddress);
    address.setValue(changeAddress);
  }
}
```

- 앞서 작성한 코드와 같은 코드입니다.
    - 단순히 `change()` 메서드만 하나 추가되었습니다.
    - 그리고 `change()` 메서드에서 `Address` 인스턴스에 있는 `value` 값을 변경합니다.
- `main()` 메서드만 보면 `a`의 값이 함께 부산으로 변경된 이류를 찾기가 더 어렵습니다.

**실행 결과**
```
a = Address{value='서울'}
b = Address{value='서울'}
주소 값을 변경합니다 -> 부산
a = Address{value='부산'}
b = Address{value='부산'}
```

여러 변수가 하나의 객체를 참조하는 공유 참조를 막을 수 있는 방법은 없습니다.
- 그럼 공유 참조로 인해 발생하는 문제를 어떻게 해결할 수 있을까요?
    - 단순히 개발자가 공유 참조 문제가 발생하지 않도록 조심해서 코드를 작성해야 할까요?

