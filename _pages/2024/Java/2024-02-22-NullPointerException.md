---
title: "☕️[Java] NullPointerException"
tags:
    - Java
    - Programming Language
date: "2024-02-22"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# NullPointerException
택배를 보낼 때 주소지 없이 택배를 발송하려면 어떤 문제가 발생할까요?
만약 참조값 없이 객체를 찾아가면 어떤 문제가 발생할까요?
* 이 경우 `NullPointerExecption`이라는 예외가 발생하는데, 개발자를 가장 많이 괴롭히는 예외입니다.
* `NullPointerExecption`은 이름 그대로 `null`을 가리리키다(Pointer)인데, 이때 발생하는 예외(Exception)입니다.
* `null`은 없다는 뜻이므로 결국 주소가 없는 곳을 찾아갈 때 발생하는 예외입니다.

* 객체를 참조할 때는 `.(dot)`을 사용합니다.
    * 이렇게 하면 참조값을 사용해서 해당 객체를 찾아갈 수 있습니다.
    * 그런데 참조값이 `null`이라면 값이 없다는 뜻이므로, 찾아갈 수 있는 객체(인스턴스)가 없습니다.
        * `NullPointerExecption`은 이처럼 `null`에 `.(dot)`을 찍었을 때 발생합니다.


예제를 통해서 확인해봅시다.
```java
package ref;

public class NullMain2 {

  public static void main(String[] args) {
    Data data = null;
    data.value = 10; // NullPointerException 예외 발생
    System.out.println("data = " + data.value);
  }
}
```
* `data` 참조형 변수에는 `null` 값이 들어가 있습니다.
    * 그런데 `data.value = 10` 이라고 하면 어떻게 될까요?

```java
data.value = 10;
null.value = 10 // data에는 null 값이 들어있습니다.
```
* 결과적으로 `null` 값은 참조할 주소가 존재하지 않는다는 뜻입니다.
    * 따라서 참조할 객체 인스턴스가 존재하지 않으므로 다음과 같이 `java.lang.NullPointerExecption`이 발생하고, 프로그램이 종료됩니다.
> 참고로 예외가 발생했기 때문에 그 다음 로직은 수행되지 않습니다.

**실행 결과**
```
Exception in thread "main" java.lang.NullPointerException: Cannot assign field "value" because "data" is null at ref.NullMain2.main(NullMain2.java:7)
```

## 멤버 변수와 null
앞선 예제와 같이 지역 변수의 경우에는 `null` 문제를 파악하는 것이 어렵지 않습니다.
* 다음과 같이 멤버 변수가 `null`인 경우에는 주의가 필요합니다.

**Data**
```java
package ref;

public class Data {
  int value;
}
```

**BigData**
```java
package ref;

public class BigData {
  Data data;
  int count;
}
```
* `BigData` 클래스는 `Data data`, `int count` 두 변수를 가집니다.

**NullMain3**
```java
package ref;

public class NullMain3 {

  public static void main(String[] args) {
    BigData bigData = new BigData();
    System.out.println("bigData.count=" + bigData.count);
    System.out.println("bigData.data=" + bigData.data);

    // NullPointerException
    System.out.println("bigData.data.value=" + bigData.data.value);
  }
}
```

**실행 결과**
```
bigData.count=0
bigData.data=null
Exception in thread "main" java.lang.NullPointerException: Cannot read field "value" because "bigData.data" is null at ref.NullMain3.main(NullMain3.java:11)
```

`BigData`를 생성하면 `BigData`의 인스턴스가 생성됩니다.
이때 `BigData` 인스턴스의 멤버 변수에 초기화가 일어나는데, `BigData`의 `data` 멤버 변수는 참조형이므로 `null`로 초기화 됩니다.
`count` 멤버 변수는 숫자이므로 `0`으로 초기화가 됩니다.
* `bigData.count`를 출력하면 `0`이 출력됩니다.
* `bigData.data`를 출력하면 참조값인 `null`이 출력됩니다. 이 변수는 아직 아무것도 참조하고 있지 않습니다.
* `bigData.data.value`를 출력하면 `data`의 값이 `null`이므로 `null`에 `.(dot)`을 찍게 되고, 따라서 참조할 곳이 없으므로 `NullPointerException` 예외가 발생하게 됩니다.

**예외 발생 과정**
```java
bigData.data.value
x001.data.value // bigData는 x001 참조값을 가집니다.
null.value // x001.data는 null 값을 가집니다.
NullPointerExecption // null 값에 .(dot)을 찍으면 예외가 발생합니다.
```

**"이 문제를 해결하려면 `Data` 인스턴스를 만들고 `BigData.data` 멤버 변수에 참조값을 할당하면 됩니다."**

**NullMain4**
```java
package ref;

public class NullMain4 {

  public static void main(String[] args) {
    BigData bigData = new BigData();
    bigData.data = new Data();
    
    System.out.println("bigData.count=" + bigData.count);
    System.out.println("bigData.data=" + bigData.data);

    System.out.println("bigData.data.value=" + bigData.data.value);
  }
}
```

**실행 결과**
```java
bigData.count=0
bigData.data=ref.Data@3a71f4dd
bigData.data.value=0
```

**실행 과정**
```java
bigData.data.value
x001.data.value // bigData는 x001 참조값을 가집니다.
x002.value // x001.data는 x002 값을 가집니다.
0 // 최종 결과
```

> 참고: `xoo1`과 `x002` 는 `sudo code`입니다.

**정리**
* **"`NullPointerException`이 발생하면 `null`값에 `.(dot)`을 찍었다고 생각하면 문제를 쉽게 찾을 수 있습니다."**
