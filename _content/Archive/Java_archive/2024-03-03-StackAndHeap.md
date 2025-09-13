---
title: "☕️[Java] 스택 영역과 힙 영역"
tags:
    - Java
    - Programming Language
date: "2024-03-03"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 스택 영역과 힙 영역.
이번에는 스택 영역과 힙 영역이 함께 사용되는 경우를 알아봅시다.

**Data**
```java
package memory;

public class JavaMemoryMain2 {

  public static void main(String[] args) {
    System.out.println("main start");
    method1();
    System.out.println("main end");
  }

  static void method1() {
    System.out.println("method1 start");
    Data data1 = new Data(10);
    method2(data1);
    System.out.println("method1 end");
  }

  static void method2(Data data2) {
    System.out.println("method2 start");
    System.out.println("data.value=" + data2.getValue());
    System.out.println("method2 end");
  }
}
```

* `main()` -> `method1()` -> `method2()` 순서로 호출하는 단순한 코드입니다.
* `method1()`에서 `Data` 클래스의 인스턴스를 생성합니다.
* `method1()`에서 `method2()`를 호출할 때 매개변수에 `Data` 인스턴스의 참조값을 전달합니다.

**실행 결과**
```
main start
method1 start
method2 start
data.value=10
method2 end
method1 end
main end
```

그림을 통해 순서대로 알아봅시다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%B3%E1%84%90%E1%85%A2%E1%86%A8%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%80%E1%85%AA%E1%84%92%E1%85%B5%E1%86%B8%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B71.png?raw=true">

* 처음 `main()` 메서드를 실행합니다.
    * `main()` 스택 프레임이 생성됩니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%B3%E1%84%90%E1%85%A2%E1%86%A8%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%80%E1%85%AA%E1%84%92%E1%85%B5%E1%86%B8%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B72.png?raw=true">

* `main()`에서 `method1()`을 실행합니다.
    * `method1()` 스택 프레임이 생성됩니다.
* `method1()`은 지역 변수로 `Data data1`을 가지고 있습니다.
    * 이 지역 변수도 스택 프레임에 포함됩니다.
* `method1()`은 `new Data(10)`를 사용해서 힙 영역에 `Data` 인스턴스를 생성합니다.
    * 그리고 참조값을 `data1`에 보관합니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%B3%E1%84%90%E1%85%A2%E1%86%A8%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%80%E1%85%AA%E1%84%92%E1%85%B5%E1%86%B8%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B73.png?raw=true">

* `method1()`은 `method2()`를 호출하면서 `Data data2` 매개변수에 `xoo1` 참조값을 넘깁니다.
    * 이제 `method1()`에 있는 `data1`과 `method2()`에 있는 `data2` 지역 변수(매개변수 포함)는 둘다 같은 `x001` 인스턴스를 참조합니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%B3%E1%84%90%E1%85%A2%E1%86%A8%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%80%E1%85%AA%E1%84%92%E1%85%B5%E1%86%B8%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B74.png?raw=true">

* `method2()`가 종료됩니다.
    * `method2()`의 스택 프레임이 제거되면서 매개변수 `data2`도 함께 제거됩니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%B3%E1%84%90%E1%85%A2%E1%86%A8%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%80%E1%85%AA%E1%84%92%E1%85%B5%E1%86%B8%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B75.png?raw=true">

* `method1()1`이 종료됩니다.
    * `method1()`의 스택 프레임이 제거되면서 지역 변수 `data1`도 함께 제거됩니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%B3%E1%84%90%E1%85%A2%E1%86%A8%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%80%E1%85%AA%E1%84%92%E1%85%B5%E1%86%B8%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B76.png?raw=true">

* `method1()`이 종료된 직후의 상태를 봅시다.
    * `method1()`의 스택 프레임이 제거되고 지역 변수 `data1`도 함께 제거되었습니다.
* 이제 `x001` 참조값을 가진 `Data` 인스턴스를 참조하는 곳이 더는 없습니다.
    * 참조하는 곳이 없으므로 사용되는 곳도 없습니다.
        * 결과적으로 프로그램에서 더는 사용하지 않는 객체인 것입니다.
            * 이런 객체는 메모리만 차지하게 됩니다.
                * GC(가비지 컬렉션)은 이렇게 참조가 모두 사라진 인스턴스를 찾아서 메모리에서 제거합니다.

> **참고:** 힙 영역 외부가 아닌, 힙 영역 안에서만 인스턴스끼리 서로 참조하는 경우에도 GC의 대상이 됩니다.

## 정리
* 지역 변수는 스택 영역에, 객체(인스턴스)는 힙 영역에 관리되는 것을 확인했습니다.
    * 이제 나머지 하나가 남았습니다. 바로 메서드 영역입니다.
        * 메서드 영역이 관리하는 변수도 있습니다.
            * 이것을 이해하기 위해서는 먼저 `static` 키워드를 알아야합니다.
                * `static` 키워드는 메서드 영역과 밀접한 연관이 있습니다.
