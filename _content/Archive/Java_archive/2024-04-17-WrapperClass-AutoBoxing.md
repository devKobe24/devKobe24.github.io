---
title: "☕️[Java] 래퍼 클래스 - 오토 박싱"
tags:
    - Java
    - Programming Language
date: "2024-04-17"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 래퍼 클래스 - 오토 박싱.

## 오토 박싱 - Autoboxing

자바에서 `int`를 `Integer`로 변환하거나, `Integer`를 `int`로 변환하는 부분을 정리해봅시다.
다음과 같이 `valueOf()`, `intValue()` 메서드를 사용하면 됩니다.

```java
package lang.wrapper;

public class AutoboxingMain1 {

  public static void main(String[] args) {
    // Primitive -> Wrapper
    int value = 7;
    Integer boxedValue = Integer.valueOf(value);

    // Wrapper -> Primitive
    int unboxedValue = boxedValue.intValue();

    System.out.println("boxedValue = " + boxedValue);
    System.out.println("unboxedValue = " + unboxedValue);
  }
}
```

**실행 결과**
```
boxedValue = 7
unboxedValue = 7
```

- 박싱: `valueOf()`
- 언박싱: `xxxValue()` (예: `intValue()`, `doubleValue()`)

개발자들이 오랜기간 개발을 하다 보니 기본형을 래퍼 클래스로 변환하거나 또는 래퍼 클래스를 기본형으로 변환하는 일이 자주 발생했습니다.
그래서 많은 개발자들이 불편함을 호소했습니다.
- 자바는 이런 문제를 해결하기 위해 자바 1.5부터 오토 박싱(Auto-boxing), 오토 언박싱(Auto-unboxing)을 지원합니다.

## 오토 박싱, 언박싱
```java
package lang.wrapper;

public class AutoboxingMain2 {

  public static void main(String[] args) {
    // Primitive -> Wrapper
    int value = 7;
    Integer boxedValue = value; // 오토 박싱(Auto-boxing)

    // Wrapper -> Primitive
    int unboxedValue = boxedValue; // 오토 언박싱(Auto-Unboxing)

    System.out.println("boxedValue = " + boxedValue);
    System.out.println("unboxedValue = " + unboxedValue);
  }
}
```

**실행 결과**
```
boxedValue = 7
unboxedValue = 7
```

오토 박싱과 오토 언박싱은 컴파일러가 개발자 대신 `valueOf`, `xxxValue()` 등의 코드를 추가해주는 기능입니다.
덕분에 기본형과 래퍼형을 서로 편리하게 변환할 수 있습니다.
따라서 `AutoboxingMain1`과 `AutoboxingMain2`는 동일하게 작동합니다.

```java
Integer boxedValue = value; // 오토 박싱(Auto-boxing)
Integer boxedValue = Integer.valueOf(value); // 컴파일 단계에서 추가

int unboxedValue = boxedValue; // 오토 언박싱(Auto-Unboxing)
int unboxedValue = boxedValue.intValue(); // 컴파일 단계에서 추가
```
