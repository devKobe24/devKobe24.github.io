---
title: "☕️[Java] 래퍼 클래스 - 자바 래퍼 클래스"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-09-17"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] 래퍼 클래스 - 자바 래퍼 클래스.

## 1️⃣ 자바 래퍼 클래스.
- 래퍼 클래스는 기본형을 객체로 감싸서 더 편리하게 사용하도록 도와주기 때문에 상당히 유용하다.
    - 즉, 래퍼 클래스는 기본형의 객체 버전입니다.
- 자바의 래퍼 클래스(Wrapper Class)는 기본 데이터 타입(primitive type)을 객체(object)로 다루기 위해 제공되는 클래스입니다.
- 자바에는 8개의 기본 데이터 타입이 있으며, 각각에 대응하는 래퍼 클래스가 있습니다.
- 래퍼 클래스는 기본 타입을 객체로 다뤄야 할 때 유용하게 사용됩니다.

### 기본형에 대응하는 래퍼 클래스.
- byte -> Byte
- short -> Short
- int -> Integer
- long -> Long
- float -> Float
- double -> Double
- char -> Character
- boolean -> Boolean

### 기본 재퍼 클래스의 특징.
- 불변이다.
- `equals`로 비교해야 합니다.

### 자바가 제공하는 래퍼 클래스의 사용법.
```java
package langReview.wrapper;

public class WrapperClassMain {
    public static void main(String[] args) {
        Integer newInteger = new Integer(10); // 미래에 삭제 예정, 대신에 valueOf()를 사용
        Integer integerObj = Integer.valueOf(10); // -128 ~ 127 자주 사용하는 숫자 값 재사용, 불변

        Long longObj = Long.valueOf(100);

        Double doubleObj = Double.valueOf(10.5);

        System.out.println("newInteger = " + newInteger);
        System.out.println("integerObj = " + integerObj);
        System.out.println("longObj = " + longObj);
        System.out.println("doubleObj = " + doubleObj);

        System.out.println("===================내부 값 읽기===================");
        int intValue = integerObj.intValue();
        System.out.println("intValue = " + intValue);
        long longValue = longObj.longValue();
        System.out.println("longValue = " + longValue);

        System.out.println("===================비교===================");
        System.out.println("== : " + (newInteger == integerObj));
        System.out.println("equals : " + (newInteger.equals(integerObj)));
    }
}
```

**실행 결과**
```bash
newInteger = 10
integerObj = 10
longObj = 100
doubleObj = 10.5
===================내부 값 읽기===================
intValue = 10
longValue = 100
===================비교===================
== : false
equals : true
```

### 1️⃣ 래퍼 클래스 생성 - 박싱(Boxing)
- 기본형을 래퍼 클래스로 변경하는 것을 마치 박스에 물건을 넣은 것 같다고 해서 **박싱(Boxing)** 이라고 합니다.
- `new Integer(10)`은 직접 사용하면 안됩니다.
    - 작동은 하지만, 향후 자바에서 제거될 예정입니다.
    - 대신에 `Integer.valueOf(10)`을 사용하면 됩니다.
        - 내부에서 `new Integer(10)`을 사용해서 객체를 생성하고 돌려줍니다.
            - 추가로 `Integer.valueOf()`에는 성능 최적화 기능이 있습니다.
                - 개발자들이 일반적으로 자주 사용하는 `-128 ~ 127` 범위의 `Integer` 클래스를 미리 생성해줍니다.
                    - 해당 범위의 값을 조회하면 미리 생성된 `Integer` 객체를 반환합니다.
                    - 해당 범위의 값이 없으면 `new Integer()`를 호출합니다.
                        - 마치 문자열 풀과 비슷하게 자주 사용하는 숫자를 미리 생성해두고 재사용합니다.
                        - 참고로 이런 최적화 방식은 미래에 더 나은 방식으로 변경될 수 있습니다.

### 2️⃣ intValue() - 언박싱(Unboxing)
- 래퍼 클래스는 객체이기 때문에 `==` 비교를 하면 인스턴스의 참조값(Reference)을 비교합니다.
- 래퍼 클래스는 내부의 값을 비교하도록 `equals()`를 재정의 해두었습니다.
    - 따라서 값을 비교하려면 `equals()`를 사용해야 합니다.

> 참고로 래퍼 클래스는 객체를 그대로 출력해도 내부에 있는 값을 문자로 출력하도록 `toString()`을 재정의 해두었습니다.


## 2️⃣ 래퍼 클래스의 주요 사용 목적.
- **컬렉션과의 호환성.**
    - 자바의 컬렉션 프레임워크(List, Set, Map 등)는 객체만 저장할 수 있습니다.
        - 따라서 기본 데이터 타입(primitive type)을 컬렉션에 저장하려면 해당 값을 래퍼 클래스의 객체로 변환해야 합니다.
- **유틸리티 메서드 제공.**
    - 래퍼 클래스는 기본 데이터 타입을 문자열로 변환하거나, 문자열을 기본 데이터 타입(primitive type)으로 변환하는 등의 유용한 유틸리티 메서드를 제공합니다.
- **상수 정의.**
    - 래퍼 클래스는 `MAX_VALUE`. `MIN_VALUE` 등과 같은 상수를 제공합니다.
        - 이 상수들은 해당 타입의 최대값, 최소값 등을 표현합니다.

## 3️⃣ 래퍼 클래스의 주요 메서드.
- `valueOf(String s)` : 문자열을 기본 데이터 타입의 래퍼 객체로 변환합니다.
- `parseInt(String s)` : 문자열을 `int`로 변환합니다.
- `toString()` : 객체의 값을 문자열로 변환합니다.
- `compareTo(T another)` : 두 래퍼 객체를 비교하여 정수 값을 반환합니다.

## 4️⃣ 예시.
```java
public class WrapperClassExample {
    public static void main(String[] args) {
        // 기본 타입을 래퍼 클래스로 변환 (박싱, Boxing)
        Integer intObject = Integer.valueOf(42);
        Double doubleObject = Double.valueOf(3.14);
        Boolean booleanObject = Boolean.valueOf(true);
        
        // 래퍼 클래스를 기본 타입으로 변환(언박싱, Unboxing)
        int intValue = intObject.intValue();
        double doubleValue = doubleObject.doubleValue();
        boolean = booleanValue = booleanObject.booleanValue();
        
        
        System.out.println("Integer object: " + intObject);
        System.out.println("Double object: " + doubleObject);
        System.out.println("Boolean object: " + booleanObject);

        System.out.println("Unboxed int: " + intValue);
        System.out.println("Unboxed double: " + doubleValue);
        System.out.println("Unboxed boolean: " + booleanValue);
    }
}
```
- 이 코드는 기본 데이터 타입과 래퍼 클래스 간의 변환을 보여주며, 박싱과 언박싱이 어떻게 동작하는지를 설명합니다.
- 이러한 기능들은 자바에서 기본 타입을 객체로 다루어야 하는 상황에서 매우 유용하게 사용됩니다.
