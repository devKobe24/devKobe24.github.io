---
title: "☕️[Java] 기본형과 참조형(2) - 변수 대입"
tags:
    - Java
    - Programming Language
date: "2024-02-20"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 기본형과 참조형(2) - 변수 대입
**"대원칙: 자바는 항상 변수의 값을 복사해서 대입합니다."**
* 자바에서 변수에 값을 대입하는 것은 변수에 들어 있는 값을 복사해서 대입하는 것입니다.
    * 기본형, 참조형 모두 항상 변수에 있는 값을 복사해서 대입합니다.
    * **"기본형"** 이면 변수에 들어 있는 "실제 사용하는 값"을 복사해서 대입하고, **"참조형"** 이면 변수에 들어 있는 "참조값"을 복사해서 대입합니다.

이 대원칙을 이해하면 복잡한 상황에도 코드를 단순하게 이해할 수 있습니다.

**기본형 대입**
```java
int a = 10;
int b = a;
```

**참조형 대입**
```java
Student s1 = new Student();
Student s2 = s1;
```

기본형은 변수에 값을 대입하더라도 실제 사용하는 값이 변수에 바로 들어 있기 때문에 값만 복사해서 대입한다고 생각하면 쉽게 이해할 수 있습니다.

그러나 **참조형의 경우 실제 사용하는 객체가 아니라 객체의 위치를 가르키는 참조값만 복사**가 됩니다.
* 쉽게 이야기해서 실제 건물이 복사가 되는 것이 아니라 건물의 위치인 주소만 복사되는 것입니다.
    * 따라서 같은 건물을 찾아갈 수 있는 방법이 하나 늘어날 뿐입니다.

**"구체적인 예시를 통해서 변수 대입시 기본형과 참조형의 차이를 알아봅시다."**

**VarChange1**
```java
package ref;

public class VarChange1 {

  public static void main(String[] args) {
    int a = 10;
    int b = a;
    System.out.println("a = " + a);
    System.out.println("b = " + b);

    // a 변경
    a = 20;
    System.out.println("변경 a = 20");
    System.out.println("a = " + a);
    System.out.println("b = " + b);

    // b 변경
    b = 30;
    System.out.println("변경 b = 30");
    System.out.println("a = " + a);
    System.out.println("b = " + b);
  }
}
```

**실행 결과**
```
a = 10
b = 10
변경 a = 20
a = 20
b = 10
변경 b = 30
a = 20
b = 30
```

**"코드를 순서대로 알아봅시다."**

```java
int a = 10;
int b = a;
```

**실행 결과**
```
a = 10
b = 10
```

* **"변수의 대입은 변수에 들어있는 값을 복사해서 대입합니다."**
    * 여기에서 변수 `a`에 들어있는 값 `10`을 복사해서 변수 `b`에 대입합니다.
        * 변수 `a` 자체를 `b`에 대입하는 것이 아닙니다.

```java
a = 20;
```

**실행 결과**
```
a = 10
b = 20
```

* 변수 `a`에 값 `20`을 대입했습니다.
    * 따라서 변수 `a`의 값이 `10`에서 `20`으로 변경되었습니다.
        * **"당연한 이야기지만 변수 `b`에는 아무런 영향을 주지 않습니다."**

```java
b = 30;
```

**실행 결과**
```
a = 20
b = 30
````

* 변수 `b`에 값 `30`을 대입했습니다.
    * 변수 `b`의 값이 `10`에서 `30`으로 변경되었습니다.
        * **"당연한 이야기지만 변수 `a`에는 아무런 영향을 주지 않습니다."**

**여기서 핵심은 `int b = a`라고 했을 때 변수에 들어있는 값을 복사해서 전달한다는 점입니다.**
* 따라서 `a = 20`, `b = 30`이라고 했을 때 각각 본인의 값만 변경되는 것을 확인할 수 있습니다.

## 참조형과 변수 대입

아래의 예시 코드를 보고 참조형과 변수 대입에 대하여 알아봅시다.

**Data**
```java
package ref;

public class Data {
  int value;
}
```

**VarChange2**
```java
package ref;

public class VarChange2 {

  public static void main(String[] args) {
    Data dataA = new Data();
    dataA.value = 10;
    Data dataB = dataA;

    System.out.println("dataA 참조값 = " + dataA);
    System.out.println("dataB 참조값 = " + dataB);
    System.out.println("dataA.value = " + dataA.value);
    System.out.println("dataB.value = " + dataB.value);

    // dataA 변경.
    dataA.value = 20;
    System.out.println("변경 dataA.value = 20");
    System.out.println("dataA.value = " + dataA.value);
    System.out.println("dataB.value = " + dataB.value);

    // dataB 변경.
    dataB.value = 30;
    System.out.println("변경 dataA.value = 30");
    System.out.println("dataA.value = " + dataA.value);
    System.out.println("dataB.value = " + dataB.value);
  }
}
```

**실행 결과**
```
dataA 참조값 = ref.Data@30f39991
dataB 참조값 = ref.Data@30f39991
dataA.value = 10
dataB.value = 10
변경 dataA.value = 20
dataA.value = 20
dataB.value = 20
변경 dataA.value = 30
dataA.value = 30
dataB.value = 30
```

**"코드를 한 줄씩 읽어가며 이해 해봅시다."**
```java
Data dataA = new Data();
dataA.value = 10;
```

* `dataA` 변수는 `Data` 클래스를 통해서 만들었기 때문에 **"참조형"**입니다.
    * 이 변수는 **"`Data` 형 객체의 참조값을 저장"** 합니다.
* `Data` 객체를 생성하고, 참조값을 `dataA`에 저장합니다.
    * 그리고 객체의 `value` 변수에 값 `10`을 저장했습니다.

```java
Data dataB = dataA;
```

**실행 코드**
```java
Data dataA = new Data();
dataA.value = 10;
Data dataB = dataA;

System.out.println("dataA 참조값 = " + dataA);
System.out.println("dataB 참조값 = " + dataB);
System.out.println("dataA.value = " + dataA.value);
System.out.println("dataB.value = " + dataB.value);
```

**출력 결과**
```
dataA 참조값 = ref.Data@30f39991
dataB 참조값 = ref.Data@30f39991
dataA.value = 10
dataB.value = 10
```

* 변수의 대입은 변수에 들어있는 **"값을 복사해서 대입"** 합니다.
    * 변수 `dataA`에는 참조값 `30f39991`이 들어있습니다.
        * 여기서는 변수 `dataA`에 들어있는 참조값 `30f39991`을 복사해서 변수 `dataB`에 대입합니다.
            * 이제 `dataA`와 `dataB`에 들어있는 참조값은 같습니다.
                * 따라서 둘 다 같은 `30f39991` 인스턴스를 가리킵니다.

> 참고로 변수 `dataA`가 가르키는 인스턴스를 복사하는 것이 아닙니다!
> **"변수에 들어있는 참조값만 복사해서 전달하는 것 입니다."**

```java
dataA.value = 20;
```

**실행 코드**
```java
dataA.value = 20;
System.out.println("dataA.value = " + dataA.value);
System.out.println("dataB.value = " + dataB.value);
```

**출력 결과**
```java
dataA.value = 20
dataB.value = 20
```

* `dataA.value = 20` 코드를 실행하면 `dataA`가 가르키는 `30f39991` 인스턴스의 `value` 값을 `10`에서 `20`으로 변경합니다.
    * 그러나 `dataA`와 `dataB`는 같은 `30f39991` 인스턴스를 참조하기 때문에 `dataA.value`와 `dataB.value`는 둘 다 같은 값인 `20`을 출력합니다.

```java
dataB.value = 30;
```

**실행 코드**
```java
dataB.value = 30;
System.out.println("dataA.value = " + dataA.value);
System.out.println("dataB.value = " + dataB.value);
```

**출력 결과**
```
dataA.value = 30
dataB.value = 30
```
* `dataB.value = 30` 코드를 실행하면 `dataB`가 가르키는 `30f39991` 인스턴스의 `value` 값을 `20`에서 `30`으로 변경합니다.
    * 그러나 `dataA`와 `dataB`는 같은 `30f39991` 인스턴스를 참조하기 때문에 `dataA.value`와 `dataB.value`는 같은 값인 `30`을 출력합니다.

"여기서 핵심은 `Data dataB = dataA` 라고 했을 때 **변수에 들어있는 값**을 복사해서 사용한다는 점입니다."
* 그런데 그 값이 참조값입니다.
    * 따라서 `dataA`와 `dataB`는 같은 참조값을 가지게 되고, 두 변수는 같은 객체 인스턴스를 참조하게 됩니다.
