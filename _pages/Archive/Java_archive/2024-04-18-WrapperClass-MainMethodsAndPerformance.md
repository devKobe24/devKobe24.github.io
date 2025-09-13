---
title: "☕️[Java] 래퍼 클래스 - 주요 메서드와 성능"
tags:
    - Java
    - Programming Language
date: "2024-04-18"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 래퍼 클래스 - 주요 메서드와 성능.

## 래퍼 클래스 - 주요 메서드.
래퍼 클래스가 제공하는 주요 메서드를 알아봅시다.

```java
package lang.wrapper;

public class WrapperUtilsMain {

  public static void main(String[] args) {
    Integer i1 = Integer.valueOf(10); // 숫자, 래퍼 객체 변환.
    Integer i2 = Integer.valueOf("10"); // 문자열, 래퍼 객체 변환.
    int intValue = Integer.parseInt("10"); // 문자열 전용, 기본형 변환.

    // 비교
    int compareResult = i1.compareTo(20);
    System.out.println("compareResult = " + compareResult);

    // 산술 연산
    System.out.println("sum: " + Integer.sum(10, 20));
    System.out.println("min: " + Integer.min(10, 20));
    System.out.println("max: " + Integer.max(10, 20));
  }
}
```

**실행 결과**
```
compareResult = -1
sum: 30
min: 10
max: 20
```

- `valueOf()`: 래퍼 타입을 반환합니다. 숫자, 문자열을 모두 지원합니다.
- `parseInt()`: 문자열을 기본형으로 변환합니다.
- `compareTo()`: 내 값과 인수로 넘어온 값을 비교합니다. 내 값이 크면 `1`, 같으면 `0`, 내 값이 작으면 `-1`을 반환합니다.
- `Integer.sum()`, `Integer.min()`, `Integer.max()`: `static` 메서드 입니다. 간단한 덧셈, 작은 값, 큰 값 연산을 수행합니다.

**pareInt() vs valueOf()**
원하는 타입에 맞는 메서드를 사용하면 됩니다.
- `valueOf("10")`는 래퍼 타입을 반환합니다.
- `parseInt("10")`는 기본형을 반환합니다.
    - `Long.parseLong()` 처럼 각 타입에 `parseXxx()`가 존재합니다.

## 래퍼 클래스와 성능
래퍼 클래스는 객체이기 때문에 기본형보다 다양한 기능을 제공합니다.
그렇다면 더 좋은 래퍼 클래스만 제공하면 되지 기본형을 제공하는 이유는 무엇일까요?

다음 코드를 실행해서 기본형과, 래퍼 클래스의 성능 차이를 비교해봅시다.

```java
package lang.wrapper;

public class WrapperVsPrimitive {

  public static void main(String[] args) {
    int iterations = 1_000_000_000; // 반복 횟수 설정, 10억
    long startTime, endTime;

    // 기본형 long 사용
    long sumPrimitive = 0;
    startTime = System.currentTimeMillis();
    for (int i = 0; i < iterations; i++) {
      sumPrimitive += i;
    }
    endTime = System.currentTimeMillis();
    System.out.println("sumPrimitive = " + sumPrimitive);
    System.out.println("기본 자료형 long 실행 시간: " + (endTime - startTime) + "ms");

    // 래퍼 클래스 Long 사용
    Long sumWrapper = 0L;
    startTime = System.currentTimeMillis();
    for (int i = 0; i < iterations; i++) {
      sumWrapper += i; // 오토 박싱 발생
    }
    endTime = System.currentTimeMillis();
    System.out.println("sumWrapper = " + sumWrapper);
    System.out.println("래퍼 클래스 Long 실행 시간: " + (endTime - startTime) + "ms");
  }
}
```
- 단순히 값을 반복해서 10억 번 더합니다.
- 기본형 `long`에 더하는 것과 래퍼 클래스 `Long`에 더하는 부분으로 나누어 테스트 합니다. 결과 값은 같습니다.

**실행 결과 - M1 맥북 기준**
```
sumPrimitive = 499999999500000000
기본 자료형 long 실행 시간: 381ms
sumWrapper = 499999999500000000
래퍼 클래스 Long 실행 시간: 1640ms
```
- 기본형 연산이 래퍼 클래스보다 대략 5배 정도 빠른 것을 확인할 수 있습니다. 참고로 계산 결과는 시스템 마다 다릅니다.
- 기본형은 메모리에서 단순히 그 크기만큼의 공간을 차지합니다. 예를 들어 `int` 는 보통 4바이트의 메모리를 사용합니다.
- 래퍼 클래스의 인스턴스는 내부에 필드로 가지고 있는 기본형의 값 뿐만 아니라 자바에서 객체 자체를 다루는데 필요한 객체 메타데이터를 포함하므로 더 많은 메모리를 사용합니다. 자바 버전과 시스템마다 다르지만 대략 8-16바이트의 메모리를 추가로 사용합니다.

**기본형, 래퍼 클래스 어떤 것을 사용?**
- 이 연산은 10억 번의 연산을 수행했을 때 0.3초와, 1.5초의 차이입니다.
- 기본형이든 래퍼 클래스든 이것을 1회로 환산하면 둘다 매우 빠르게 연산이 수행됩니다.
    - 0.3초 나누기 10억, 1.5초 나누기 10억이다.
- 일반적인 애플리케이션을 만드는 관점에서 보면 이런 부분을 최적화해도 사막의 모래알 하나 정도의 차이가 날 뿐입니다.
- CPU 연산을 아주 많이 수행하는 특수한 경우이거나, 수만 ~ 수십만 이상 연속해서 연산을 수행해야 하는 경우라면 기본형을 사용해서 최적화를 고려합시다.
- 그렇지 않은 일반적인 경우라면 코드를 유지보수하기 더 나은 것을 선택하면 됩니다.

**유지보수 vs 최적화**
유지보수 vs 최적화를 고려해야 하는 상황이라면 유지보수하기 좋은 코드를 먼저 고민해야 합니다.
특히 최신 컴퓨터는 매우 빠르기 때문에 메모리 상에서 발생하는 연산을 몇 번 줄인다고해도 실질적인 도움이 되지 않는 경우가 많습니다.
- 코드 변경 없이 최적화를 하면 가장 좋겠지만, 성능 최적화는 대부분 단순함 보다는 복잡함을 요구하고, 더 많은 코드들을 추가로 만들어야 합니다. 최적화를 위해 유지보수 해야 하는 코드가 더 늘어나는 것입니다. 그런데 진짜 문제는 최적화를 한다고 했지만 전체 애플리케이션의 성능 관점에서 보면 불필요한 최적화를 할 가능성이 있습니다.
- 특히 웹 애플리케이션의 경우 메모리 안에서 발생하는 연산 하나보다 네트워크 호출 한 번이 많게는 수십만배 더 오래 걸립니다. 자바 메모리 내부에서 발생하는 연산을 수천번에서 한 번으로 줄이는 것 보다, 네트워크 호출 한 번을 더 줄이는 것이 더 효과적인 경우가 많습니다.
- 권장하는 방법은 개발 이후에 성능 테스트를 해보고 정말 문제가 되는 부분을 찾아서 최적화 하는 것입니다.
