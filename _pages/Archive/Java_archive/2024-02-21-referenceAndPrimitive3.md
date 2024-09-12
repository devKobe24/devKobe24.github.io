---
title: "☕️[Java] 기본형과 참조형(3) - 메서드 호출"
tags:
    - Java
    - Programming Language
date: "2024-02-21"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 기본형과 참조형(3) - 메서드 호출

**대원칙: 자바는 항상 변수의 값을 복사해서 대입합니다.**
* 자바에서 변수에 값을 대입하는 것은 변수에 들어 있는 값을 복사해서 대입하는 것입니다.
* 기본형, 참조형 모두 항상 변수에 있는 값을 복사해서 대입합니다.
    * 기본형이면 변수에 들어 있는 실제 사용하는 값을 복사해서 대입합니다.
    * 참조형이면 변수에 들어 있는 참조값을 복사해서 대입합니다.

**메서드 호출도 마찬가지입니다. 메서드를 호출할 때 사용하는 매개변수(parameter)도 결국 변수일 뿐 입니다.**
* 따라서 메서드를 호출할 때 매개변수(parameter)에 값을 전달하는 것도 앞서 설명한 내용과 같이 값을 복사해서 전달합니다.

**다음 예시 코드를 봐봅시다.**

## 기본형과 메서드 호출
```java
package ref;

public class MethodChange1 {

  public static void main(String[] args) {
    int a = 10;
    System.out.println("메서드 호출 전: a = " + a);
    changePrimitive(a);
    System.out.println("메서드 호출 후: a = " + a);
  }

  static void changePrimitive(int x) {
   x = 20;
  }
}
```

**실행 결과**
```java
메서드 호출 전: a = 10
메서드 호출 후: a = 10
```

**1. 메서드 호출**
* 메서드를 호출할 때 매개변수 `x`에 변수 `a`의 값을 전달합니다.
```java
int x = a
```
* 이 코드는 다음과 같이 해석할 수 있습니다.
    * 자바에서 변수에 값을 대입하는 것은 항상 값을 복사해서 대입합니다 따라서 변수 `a`,`x` 각각 숫자 `10`을 가지고 있습니다.

**2. 메서드 안에서 값을 변경**
```java
int a = 10;
changePrimitive(a);
```
* 메서드 안에서 `x = 20`으로 새로운 값을 대입합니다.
    * 결과적으로 `x`의 값만 `20`으로 변경되고, `a`의 값은 `10`으로 유지됩니다.

**3. 메서드 종료**
```java
int a = 10;
changePrimitive(a);
```
* 메서드 종료후 값을 확인해보면 `a`는 `10`이 출력되는 것을 확인할 수 있습니다. 
    * 참고로 메서드가 종료되면 매개변수 `x`는 제거됩니다.

## 참조형과 메서드 호출
```java
package ref;

public class MethodChange2 {

  public static void main(String[] args) {
    Data dataA = new Data();
    dataA.value = 10;
    System.out.println("메서드 호출 전: dataA.value = " + dataA.value);
    changeReference(dataA);
    System.out.println("메서드 호출 전: dataA.value = " + dataA.value);
  }

  static void changeReference(Data dataX) {
    dataX.value = 20;
  }
}
```

**실행 결과**
```java
메서드 호출 전: dataA.value = 10
메서드 호출 전: dataA.value = 20
```

**1. 메서드 호출**

* 메서드를 호출할 때 매개변수 `dataX`에 변수 `dataA`의 값을 전달합니다.
    * 이 코드는 다음과 같이 해석할 수 있습니다.
```java
int dataX = dataA;
```
* 자바에서 변수에 값을 대입하는 것은 항상 값을 복사해서 대입합니다.
    * 변수 `dataA`는 참조값 `x001`을 가지고 있다는 가정하에 `dataA`는 참조값을 복사해서 전달합니다.
        * 따라서 변수 `dataA`, `dataX` 둘 다 같은 참조값인 `x001`을 가지게 됩니다.
            * 이제 `dataX`를 통해서도 `x001`에 있는 `Data` 인스턴스에 접근할 수 있습니다.

**2. 메서드 안에서 값을 변경**
```java
static void changeReference(Data dataX) {
    dataX.value = 20;
}
```
* 메서드 안에서 `dataX.value = 20`으로 새로운 값을 대입합니다.
    * 참조값을 통해 `x001` 인스턴스에 접근하고 그 안에 있는 `value`의 값을 `20`으로 변경했습니다.
        * `dataA`, `dataX` 모두 같은 `x001` 인스턴스를 참조하기 때문에 `dataA.value`와 `dataX.value`는 둘다 `20`이라는 값을 가집니다.

**3. 메서드 종료**
* 메서드 종료후 `dataA.value`의 값을 확인해보면 다음과 같이 `20`으로 변경된 것을 확인할 수 있습니다.
```
메서드 호출 전: dataA.value = 10
메서드 호출 전: dataA.value = 20
```

**기본형과 참조형의 메서드 호출**

자바에서 메서드의 매개변서(Parameter)는 항상 값에 의해 전달됩니다.
그러나 이 값이 실제 값이냐, 참조(메모리 주소)값이냐에 따라 동작이 달라집니다.
* **기본형**: 메서드로 기본형 데이터를 전달하면, 해당 값이 복사되어 전달됩니다. 이 경우, 메서드 내부에서 매개변수(Parameter)의 값을 변경해도, 호출자의 변수 값에는 영향이 없습니다.
* **참조형:** 메서드로 참조형 데이터를 전달하면, 참조값이 복사되어 전달됩니다. 이 경우, 메서드 내부에서 매개변수(Parameter)로 전달된 객체의 멤버 변수를 변경하면, 호출자의 객체도 변경됩니다.
