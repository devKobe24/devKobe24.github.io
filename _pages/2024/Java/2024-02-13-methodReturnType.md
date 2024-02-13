---
title: "☕️[Java] 반환타입."
tags:
    - Java
    - Programming Language
date: "2024-02-13"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 반환 타입.

## 반환 타입이 있으면 반드시 값을 반환해야 합니다.
반환 타입이 있는 메서드는 반드시 `return`을 사용해서 값을 반환해야 합니다.
이 부분은 특히 조건문과 함께 사용할 때 주의해야 합니다.

**MethodReturn1**
```java
package method;

public class MethodReturn1 {

  public static void main(String[] args) {
    boolean result = odd(2);
    System.out.println(result);
  }

  public static boolean odd(int i) {
    if (i % 2 == 1) {
      return true;
    }
  }
}
```

위 코드에서 `if` 조건이 만족할 때는 `true`가 반환됩니다.
하지만 조건을 만족하지 않는 경우에는 `return`문이 실행되지 않습니다.
따라서 위 코드를 실행하면 `return` 문을 누락했다는 컴파일 오류가 발생합니다.

**컴파일 오류**
`java: missing return statement`

**MethodReturn1 - 수정코드**
```java
package method;

public class MethodReturn1 {

  public static void main(String[] args) {
    boolean result = odd(2);
    System.out.println(result);
  }

  public static boolean odd(int i) {
    if (i % 2 == 1) {
      return true;
    } else {
      return false;
    }
  }
}
```

위와 같이 수정하면 `ìf` 조건을 만족하지 않아도 `else`를 통해 `return`문이 실행됩니다.

## return 문을 만나면 그 즉시 메서드를 빠져나갑니다.
`return` 문을 만나면 그 즉시 해당 메서드를 빠져나갑니다.

다음 로직을 수행하는 메서드를 만들어보겠습니다.
* 18세 미만의 경우: 미성년자는 출입이 불가합니다.
* 18세 이상의 경우: 입장하세요.

**MethodReturn2**
```java
package method;

public class MethodReturn2 {

  public static void main(String[] args) {
//    checkAge(10);
    checkAge(18);
  }

  public static void checkAge(int age) {
    if (age < 18) {
      System.out.println(age + "세, 미성년자는 출입이 불가합니다.");
      return;
    }
    System.out.println(age + "세, 입장하세요.");
  }
}
```
* 18세 미만의 경우, "미성년자는 출입이 불가능합니다."를 출력하고 바로 `return`문이 수행됩니다.
    * 따라서 다음 로직을 수행하지 않고, 해당 메서드를 빠져나옵니다.
* 18세 이상의 경우, "입장하세요."를 출력하고, 메서드가 종료됩니다.
    * 참고로 반환 타입이 없는 `void`형이기 때문에 마지막 줄의 `return`은 생략할 수 있습니다.

### 반환 값 무시.
반환 타입이 있는 메서드를 호출했는데 만약 반환 값이 필요없다면 사용하지 않아도 됩니다.

* 예시 1) `int sum = add(1,2)`
    * 반환된 값을 받아서 'sum'에 저장했습니다.
* 예시 2) `add(1,2)`
    * 반환된 값을 사용하지 않고 버립니다.
        * 여기서는 '예시 1'과 같이 호출 결과를 변수에 담지 않았습니다. 단순히 메서드만 호출했습니다.
