---
title: "☕️[Java] 불변 객체 - 도입"
tags:
    - Java
    - Programming Language
date: "2024-04-03"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 불변 객체 - 도입
지금까지 발생한 문제를 잘 생각해보면 공유하면 안되는 객체를 여러 변수에서 공유했기 때문에 발생한 문제입니다.
- 하지만 앞서 살펴보았듯이 객체의 공유를 막을 수 있는 방법은 없습니다.
    - 그런데 사이드 이펙트의 더 근본적인 원인을 고려해보면, 객체를 공유하는 것 자체는 문제가 아닙니다.
    - 객체를 공유한다고 바로 사이드 이펙트가 발생하지 않습니다.
        - **문제의 직접적인 원인은 공유된 객체의 값을 변경한 것에 있습니다.**

앞의 예를 떠올려보면 `a`, `b`는 처음 시점에는 둘 다 `"서울"`이라는 주소를 사용해야 합니다.
- 그리고 이후에 `b`의 주소를 `"부산"`으로 변경해야 합니다.
```java
Address a = new Address("서울");
Address b = a;
```

따라서 처음에는 `b = a`와 같이 `"서울"`이라는 `Address` 인스턴스를 `a`, `b`가 함께 사용하는 것이, 다음 코드와 서로 다른 인스턴스를 사용하는 것 보다 메모리와 성능상 더 효율적입니다.
- 인스턴스가 하나이니 메모리가 절약되고, 인스턴스를 하나 생성하지 않아도 되니 생성 시간이 줄어서 성능상 효율적입니다.

```java
Address a = new Address("서울");
Address b = new Address("서울");
```

여기까지는 `Address b = a`와 같이 공유 참조를 사용해도 아무런 문제가 없습니다. 오히려 더 효율적입니다.

**진짜 문제는 이후에 b가 공유 참조하는 인스턴스의 값을 변경하기 때문에 발생합니다.**
```java
b.setValue("부산"); // b의 값을 부산으로 변경해야 합니다.
System.out.println("부산 -> b");
System.out.println("a = " + a); // 사이드 이펙트 발생
System.out.println("b = " + b);
```

자바에서 여러 참조형 변수가 하나의 객체(인스턴스)를 참조하는 공유 참조 문제는 피할 수 없습니다.
- 기본형과 다르게 참조형인 객체는 처음부터 처음부터 여러 참조형 변수에서 공유될 수 있도록 설계되었습니다.
    - 따라서 이것은 문제가 아닙니다.
        - **문제의 직접적인 원인은 공유될 수 있는 `Address` 객체의 값을 더이선가 변경했기 때문입니다.**

만약 `Address` 객체의 값을 변경하지 못하게 설계했다면 이런 사이드 이펙트 자체가 발생하지 않을 것입니다.

## 불변 객체 도입
객체의 상태(객체 내부의 값, 필드, 멤버 변수)가 변하지 않는 객체를 불변 객체(Immutable Object)라 합니다.

앞서 만들었던 `Address` 클래스를 상태가 변하지 않는 불변 클래스로 다시 만들어 봅시다.

```java
package lang.immutable.address;

public class ImmutableAddress {
  private final String value;

  public ImmutableAddress(String value) {
    this.value = value;
  }

  public String getValue() {
    return value;
  }

  @Override
  public String toString() {
    return "Address{" +
        "value='" + value + '\'' +
        '}';
  }
}
```
- 내부 값이 변경되면 안됩니다.
    - 따라서 `value`의 필드를 `final`로 선언했습니다.
- 값을 변경할 수 있는 `setValue()`를 제거했습니다.
- 이 클래스는 생성자를 통해서만 값을 설정할 수 있고, 이후에는 값을 변경하는 것이 불가능합니다.

불변 클래스를 만드는 방법은 아주 단순합니다.
- 어떻게든 필드 값을 변경할 수 없게 클래스를 설계하면 됩니다.

```java
package lang.immutable.address;

public class RefMain2 {

  public static void main(String[] args) {
    // 참조형 변수는 하나의 인스턴스를 공유할 수 있습니다.
    ImmutableAddress a = new ImmutableAddress("서울");
    ImmutableAddress b = a; // 참조값 대입을 막을 수 있는 방법이 없다.

    System.out.println("a = " + a);
    System.out.println("b = " + b);

    // b.setValue("부산"); // 컴파일 오류 발생
    b = new ImmutableAddress("부산");
    System.out.println("부산 -> b");
    System.out.println("a = " + a); // 사이드 이펙트 발생
    System.out.println("b = " + b);
  }
}
```
- `ImmutableAddress`의 경우 값을 변경할 수 있는 `b.setValue()` 메서드 자체가 제거되었습니다.
- 이제 `ImmutableAddress` 인스턴스의 값을 변경할 수 있는 방법은 없습니다.
- `ImmutableAddress`를 사용하는 개발자는 값을 변경하려고 시도하다가, 값을 변경하는 것이 불가능하다는 사실을 알고, 이 객체가 불변 객체인 사실을 깨닫습니다.
    - 예를 들어 `b.setValue("부산")`을 호출하려고 했는데, 해당 메서드가 없다는 사실을 컴파일 오류를 통해 인지한다.
        - 따라서 어쩔 수 없이 새로운 `ImmutableAddress("부산")` 인스턴스를 생성해서 `b`에 대입한다.
- 결과적으로 `a`, `b`는 서로 다른 인스턴스를 참조하고, `a`가 참조하던 `ImmutableAddress`는 그대로 유지됩니다.

**실행 결과**
```
a = Address{value='서울'}
b = Address{value='서울'}
부산 -> b
a = Address{value='서울'}
b = Address{value='부산'}
```

실행 결과를 보면 `a`의 값은 그대로 유지되는 것을 확인할 수 있습니다.
<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%87%E1%85%AE%E1%86%AF%E1%84%87%E1%85%A7%E1%86%AB%E1%84%80%E1%85%A2%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%83%E1%85%A9%E1%84%8B%E1%85%B5%E1%86%B8-1.png?raw=true">
- 자바에서 객체의 공유 참조는 막을 수 없습니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%87%E1%85%AE%E1%86%AF%E1%84%87%E1%85%A7%E1%86%AB%E1%84%80%E1%85%A2%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%83%E1%85%A9%E1%84%8B%E1%85%B5%E1%86%B8-2.png?raw=true">
- `ImmutableAddress`는 불변 객체입니다. 따라서 값을 변경할 수 없습니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%87%E1%85%AE%E1%86%AF%E1%84%87%E1%85%A7%E1%86%AB%E1%84%80%E1%85%A2%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%83%E1%85%A9%E1%84%8B%E1%85%B5%E1%86%B8-3.png?raw=true">
- `ImmutableAddress`은 불변 객체이므로 `b`가 참조하는 인스턴스의 값을 서울에서 부산으로 변경하려면 새로운 인스턴스를 생성해서 할당해야 합니다.

## 정리
**불변이라는 단순한 제약을 사용해서 사이드 이펙트라는 큰 문제를 막을 수 있습니다.**
- 객체의 공유 참조는 막을 수 없습니다.
    - 그래서 객체의 값을 변경하면 다른 곳에서 참조하는 변수의 값도 함께 변경되는 사이드 이펙트가 발생합니다.
    - 사이드 이펙트가 발생하면 안되는 상황이라면 불변 객체를 만들어 사용하면 됩니다.
    - 불변 객체는 값을 변경할 수 없기 때문에 사이드 이펙트가 원천 차단됩니다.
- 불변 객체는 값을 변경할 수 없습니다.
    - 따라서 불변 객체의 값을 변경하고 싶다면 변경하고 싶은 값으로 새로운 불변 객체를 생성해야 합니다.
        - 이렇게 하면 기존 변수들이 참조하는 값에는 영향을 주지 않습니다.

> **참고 - 가변(Mutable) 객체 VS 불변(Immutable) 객체**
> 가변은 이름 그대로 처음 만든 이후 상태가 변할 수 있다는 뜻입니다.(사전적으로 사물의 모양이나 성질이 달라질 수 있다는 뜻입니다.)
> 불변은 이름 그대로 처음 만든 이후 상태가 변하지 않는다는 뜻입니다.(사전적으로 사물의 모양이나 성질이 달라질 수 없다는 뜻입니다.)
>
> `Address` 는 가변 클래스입니다. 이 클래스로 객체를 생성하면 가변 객체가 됩니다.
> `ImmutableAddress`는 불변 클래스입니다. 이 클래스로 객체를 생성하면 불변 객체가 됩니다.
