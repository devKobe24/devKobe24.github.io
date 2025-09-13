---
title: "☕️[Java] 래퍼 클래스 - 오토 박싱과 언박싱"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-09-17"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] 래퍼 클래스 - 오토 박싱과 언박싱
- 자바에서 **오토 박싱(Auto-Boxing)** 과 **오토 언박싱(Auto-unboxing)** 은 기본 데이터 타입(Primitive type)과 래퍼 클래스(Wrapper Class) 간의 자동 변환을 의미합니다.
- 자바 5에서 도입된 이 기능은 코드의 가독성과 편리성을 높여줍니다.

## 1️⃣ 오토 박싱(Auto-boxing)
- 오토 박싱(Auto-boxing)은 기본 데이터 타입(Primitive type)이 자동으로 해당하는 래퍼 클래스(Wrapper Class)의 객체로 변환되는 과정을 말합니다.
    - 예를 들어, `int` 타입이 `Integer` 객체로 자동 변환되는 상황을 생각해볼 수 있습니다.

### 예시.
```java
int num = 10;
Integer boxedNum = num; // 오토 박싱(Auto-boxing)
```

- 위의 코드에서 `int` 타입 변수 `num`이 자동으로 `Integer` 객체로 변환됩니다.
- 개발자는 명시적으로 `Integer.valueOf(num)` 같은 메서드를 호출할 필요 없이, 기본 타입을 직접 객체로 사용할 수 있습니다.

## 2️⃣ 오토 언박싱(Auto-Unboxing)
- 오토 언박싱(Auto-Unboxing)은 래퍼 클래스(Wrapper Class)의 객체가 자동으로 해당하는 기본 데이터 타입(Primitive type)으로 변환되는 과정을 말합니다.
    - 예를 들어, `Integer` 객체가 `int` 타입으로 변환되는 상황을 생각해볼 수 있습니다.

### 예시.
```java
Integer boxedNum = 20;
int num = boxedNum; // 오토 언박싱(Auto-Unboxing)
```

- 위의 코드에서 `Integer` 객체 `boxedNum`이 자동으로 `int` 타입으로 변환됩니다.
- 개발자는 명시적으로 `boxedNum.intValue()` 같은 메서드를 호출할 필요 없이, 객체를 직접 기본 타입으로 사용할 수 있습니다.

## 3️⃣ 오토 박싱과 오토 언박싱의 활용 예.
- 오토 박싱과 오토 언방식은 주로 컬렉션 프레임워크와 함께 사용됩니다.
    - 예를 들어, `List<Integer>`에 값을 추가하거나 꺼낼 때 이러한 기능이 매우 유용합니다.

### 예시
```java
import java.util.ArrayList;
import java.util.List;

public class AutoBoxingUnboxingExample {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>();
        
        // 오토 박싱: int 타입이 Integer 객체로 변환되어 리스트에 추가됩니다.
        numbers.add(10);
        numbers.add(20);
        
        // 오토 언박싱: Integer 객체가 int 타입으로 변환되어 사용됩니다.
        int firstNum = numbers.get(0);
        int secondNum = numbers.get(1);
        
        System.out.println("First number: " + firstNum);
        System.out.println("Second number: " + sceondNum);
    }
}
```

- 이 예제에서 `numbers.add(10);`는 `10`이라는 `int` 값을 `Integer` 객체로 자동으로 박싱한 뒤 리스트에 추가합니다.
    - 또한, `numbers.get(0)`은 `Integer` 객체를 `int`로 자동 언박싱하여 변수 `firstNum`에 할당합니다.

## 4️⃣ 주의 사항.
- **성능 이슈.**
    - 오토 박싱과 오토 언박싱은 성능에 영향을 줄 수 있습니다.
        - 많은 오토 박싱/언박싱이 발생하면 메모리 사용량이 증가하거나 성능이 저하될 수 있습니다.
- **NullPointerException**
    - 오토 언박싱 과정에서 래퍼 클래스 객체가 `null`이면 `nullPointerException`이 발생합니다.

### 예시.
```java
Integer nullInteger = null;
int num = nullInteger; // NullPointerException 발생
```

## 5️⃣ 결론.
- 오토 박싱과 오토 언박싱은 기본 데이터 타입과 객체 타입 간의 변환을 자동으로 처리하여 개발자가 코드를 간결하고 명확하게 작성할 수 있도록 도와줍니다.
- 하지만 이러한 자동 변환히 발생하는 곳에서는 성능과 `null` 처리에 대한 주의를 기울여랴 합니다.
