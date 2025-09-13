---
title: "☕️[Java] final 변수와 참조"
tags:
    - Java
    - Programming Language
date: "2024-03-07"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# final 변수와 참조.

`final`은 변수의 값을 변경하지 못하게 막습니다. 그런데 여기서 변수의 값이라는 것은 무엇일까요?
* 변수는 크게 기본형 변수와 참조형 변수가 있습니다.
* 기본형 변수는 `10`, `20` 같은 값을 보관하고, 참조형 변수는 객체의 참조값을 보관합니다.
    * `final`을 기본형 변수에 사용하면 값을 변경할 수 없습니다.
    * `final`을 참조형 변수에 사용하면 참조값을 변경할 수 없습니다.

여기까지는 이해하는데 어려움이 없을 것입니다.

이번에는 약간 복잡한 예제를 만들어 봅시다.

```java
package final1;

public class Data {
  public int value;
}
```

```java
package final1;

public class FinalRefMain {

  public static void main(String[] args) {
   final Data data = new Data();
   //data = new Data(); // final 변경 불가 컴파일 오류

    // 참조 대상의 값은 변경 가능
    data.value = 10;
    System.out.println(data.value);
    data.value = 20;
    System.out.println(data.value);
  }
}
```

```java
final Data data = new Data();
// data = new Data(); // final 변경 불가 컴파일 오류
```
* 참조형 변수 `data`에 `final`이 붙었습니다.
    * 변수 선언 시점에 참조값을 할당했으므로 더는 참조값을 변경할 수 없습니다.

```java
data.value = 10;
data.value = 20;
```
* 그러나 참조 대상의 객체 값은 변경할 수 있습니다.
    * 참조형 변수 `data`에 `final`이 붙었습니다.
        * 이 경우 참조형 변수에 들어있는 참조값을 다른 값으로 변경하지 못합니다.
            * 쉽게 이야기해서 이제 다른 객체를 참조할 수 없습니다.
            * 그런데 이것의 정확한 뜻을 잘 이해해야 합니다.
                * 참조값만 변경하지 못한다는 뜻입니다.
                    * 이 변수 이외에 다른 곳에 영향을 주는 것이 아닙니다.
* `Data.value`는 `final`이 아닙니다.
    * 따라서 값을 변경할 수 있습니다.

<img src="https://github.com/devKobe24/images/blob/main/final%E1%84%87%E1%85%A7%E1%86%AB%E1%84%89%E1%85%AE%E1%84%8B%E1%85%AA%E1%84%8E%E1%85%A1%E1%86%B7%E1%84%8C%E1%85%A9-1.png?raw=true">

* 정리하면 참조형 변수에 `final`이 붙으면 참조 대상 자체를 다른 대상으로 변경하지 못하는 것이지, 참조하는 대상의 값은 변경할 수 있습니다.
