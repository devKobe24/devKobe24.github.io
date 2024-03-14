---
title: "☕️[Java] 다형성과 캐스팅"
tags:
    - Java
    - Programming Language
date: "2024-03-14"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 다형성과 캐스팅.

`Parent poly = new Child()`와 같이 부모 타입의 변수를 사용하게 되면 `poly.childMethod()`와 같이 자식 타입에 있는 기능을 호출할 수 없습니다.

```java
package poly.basic;

public class CastingMain1 {

  public static void main(String[] args) {
    // 부모 변수가 자식 인스턴스 참조(다형적 참조)
    Parent poly = new Child();

    // 단 자식의 기능은 호출할 수 없습니다.(컴파일 오류 발생)
    // poly.childMethod();

    // 다운 캐스팅(부모 타입 -> 자식 타입)
    Child child = (Child) poly; // x001
    child.childMethod();
  }
}
```

**실행 결과**
```
Child.childMethod
```

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%E1%84%80%E1%85%AA%E1%84%8F%E1%85%A2%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC.png?raw=true">

* `poly.childMethod()`를 호출하면 먼저 참조값을 사용해서 인스턴스를 찾습니다.
* 인스턴스 안에서 사용할 타입을 찾아야 합니다.
    * `poly`는 `Parent` 타입입니다.
* `Parent`는 최상위 부모입니다.
    * 상속 관계는 부모로만 찾아서 올라갈 수 있습니다.
        * `childMethod`는 자식 타입에 있으므로 호출할 수 없습니다.
            * 따라서 컴파일 오류가 발생합니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%8F%E1%85%A2%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC.png?raw=true">

이럴때는 어떻게 하면 될까요?
* 호출하는 타입을 자식인 `Child` 타입으로 변경하면 인스턴스의 `Child`에 있는 `childMethod()`를 호출할 수 있습니다. 하지만 다음과 같은 문제에 봉착합니다.

**부모는 자식을 담을 수 있지만 자식은 부모를 담을 수 없습니다.**
* `Parent parent = new Child()` : 부모는 자식을 담을 수 있습니다.
* `Parent parent = child // Child child 변수` : 부모는 자식을 담을 수 있습니다.

반면에 다음과 같이 자식은 부모를 담을 수 없습니다.
```java
Child child = poly // Parent poly 변수
```

* 부모 타입을 사용하는 변수를 자식 타입에 대입하려고 하면 컴파일 오류가 발생합니다. 자식은 부모를 담을 수 없습니다.
    * 이때는 다운캐스팅이라는 기능을 사용해서 부모 타입을 잠깐 자식 타입으로 변경하면 됩니다.

다음 코드를 분석해봅시다.
```java
Child child = (Child) poly // Parent poly
```
* `(타입)` 처럼 괄호와 그 사이에 타입을 지정하면 참조 대상을 특정 타입으로 변경할 수 있습니다.
    * **이렇게 특정 타입으로 변경하는 것을 "캐스팅"** 이라고 합니다.
* 오른쪽에 있는 `(Child) poly` 코드를 먼저 봅시다.
    * `poly`는 `Parent` 타입입니다.
        * 이 타입을 `(Child)`를 사용해서 일시적으로 자식 타입인 `Child` 타입으로 변경합니다.
            * 그리고 나서 왼쪽에 있는 `Child child`에 대입합니다.

**실행 순서**
```
Child child = (Child) poly // 다운캐스팅을 통해 부모타입을 자식 타입으로 변환한 다음에 대입 시도
Child child = (Child) x001 // 참조값을 읽은 다음 자식 타입으로 지정
Child child = x001 // 최종 결과
```

> 참고로 캐스팅을 한다고 해서 `Parent poly`의 타입이 변하는 것은 아닙니다.
> 해당 참조값을 꺼내고 참조값이 `Child` 타입이 되는 것입니다.
> 따라서 `poly`의 타입은 `Parent`로 기존과 같이 유지됩니다.

### 캐스팅
* 업캐스팅(upcasting): 부모 타입으로 변경
* 다운캐스팅(downcasting): 자식 타입으로 변경

> **캐스팅 용어**
> "캐스팅"은 영어 단어 "cast"에서 유래되었습니다.
> "cast"는 금속이나 다른 물질을 녹여서 특정한 형태나 모양으로 만드는 과정을 의미합니다.
> 
> `Child child = (Child) poly` 경우 `Parent poly`라는 부모 타입을 `Child`라는 자식 타입으로 변경했습니다.
> 부모 타입을 자식 타입으로 변경하는 것을 "다운캐스팅"이라 합니다. (부모 -> 자식)
> 반대로 부모 타입으로 변경하는 것은 "업캐스팅"이라 합니다. (자식 -> 부모)

**다운캐스팅과 실행**
```java
// 다운캐스팅(부모 타입 -> 자식 타입)
Child child = (Child) poly;
child.childMethod();
```
* 다운캐스팅 덕분에 `child.childMethod()`를 호출할 수 있게 되었습니다.
    * `childMethod()`를 호출하기 위해 해당 인스턴스를 찾아간 다음 `Child` 타입을 찾습니다.
        * `Child` 타입에는 `childMethod()`가 있으므로 해당 기능을 호출할 수 있습니다.
            * 앞의 그림을 참고합시다.
