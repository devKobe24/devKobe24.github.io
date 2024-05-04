---
title: "☕️[Java] 여러가지 연산자(1)"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-05-05"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 1️⃣ 각각의 연산자에 대한 이해

## 1. 항과 연산자.

## 1.1 단항 연산자(Unary Operator).
자바 프로그래밍에서 단항 연산자(Unary Operator)는 오직 한 개의 피연산자(operand)를 가지고 연산을 수행하는 연산자를 말합니다.

이들은 변수나 값에 직접 적용되며, 표현식의 결과를 반환합니다.

단항 연산자는 특히 간단한 수학 연산, 값의 부정, 또는 값의 증감 등에서 유용하게 사용됩니다.

## 1.2 자바에서 사용되는 주요 단항 연산자.
- **1. 부정 연산자(`'!'`)**
    - 불리언 값을 반전시킵니다.
        - 예를 들어, **`'!true'`** 는 **`'false'`** 가 됩니다.

- **2. 단항 플러스 및 마이너스 연산자(`'+', '-'`)**
    - **`'+'`** 는 일반적으로 숫자의 부호를 그대로 두지만, 명시적으로 사용할 수 있습니다.
    - **`'-'`** 는 숫자의 부호를 반전시킵니다.
        - 예를 들어, **`'-5'`** 는 양수 **`'5'`** 를 음수로 변환합니다.

- **3. 증가 및 감소 연산자(`'++', '--'`)**
    - **`'++'`** 연산자는 변수의 값을 1만큼 증가시킵니다.
    - **`'--'`** 연산자는 변수의 값을 1만틈 감소시킵니다.
    - 이 연산자들은 전위(prefix) 형태(예: **`'++x'`**)와 후위(postfix) 형태(예: **`'x++'`**)로 사용될 수 있습니다.
        - 전위 형태는 변수를 증가시키고 표현식의 값을 반환하기 전에 증가된 값을 사용하고, 후위 형태는 표현식의 값을 반환한 후 변수를 증가시킵니다.


- **4. 비트 반전 연산자(`'~'`)**
    - 정수형 변수는 모든 비트를 반전시킵니다.
        - 예를 들어, **`'~00000000'`** 은 **`'11111111'`** 이 됩니다.
            - 이는 정수에 대해 비트 단위 NOT 연산을 수행합니다.

## 1.3 예제 사용.

```java
public class UnaryDemo {
    public static void main(String[] args) {
        boolean a = false;
        System.out.println(!a); // true

        int num = 5;
        System.out.println(-num); // -5
        System.out.println(++num); // 6
        System.out.println(num++); // 6, 그리고 num이 7이 됨
        System.out.println(--num); // 6
        System.out.println(num--); // 6, 그리고 num이 5가 됨

        int b = 0b00000000; // 이진수로 0
        System.out.println(~b); // 모든 비트가 1로 반전
    }
}
```

- 이 예제에서는 다양한 단항 연산자들의 사용 방법과 그 효과를 보여줍니다.
    - 단항 연산자들은 자바 프로그래밍에서 변수를 조작하거나 특정 연산을 더 간결하게 수행하는데 매우 유용합니다.

---

## 2.1 이항 연산자(Binary Operator)
자바 프로그래밍에서 이항 연산자(Binary Operator)는 두 개의 피연산자(operand)를 취해 연산을 수행하고 결과를 반환하는 연산자를 말합니다.

이항 연산자는 수학적 계산, 논리 비교, 값의 할당 등 다양한 작업에 사용됩니다.

## 2.2 자바에서 사용되는 주요 이항 연산자의 종류.
- **1. 산술 연산자.**
    - **'+'** (덧셈)
    - **'-'** (뺄셈)
    - **'*'** (곱셈)
    - **'/'** (나눗셈)
    - **'%'** (나머지)

- **2. 비교 연산자.**
    - **'=='** (동등)
    - **'!='** (부등)
    - **'>'** (크다)
    - **'<'** (작다)
    - **'>='** (크거나 같다)
    - **'<='** (작거나 같다)

- **3. 논리 연산자.**
    - **'&&'** (논리적 AND)
    - **'||'** (논리적 OR)

- **4. 비트 연산자**
    - **'&'** (비트 AND)
    - **'|'** (비트 OR)
    - **'^'** (비트 XOR)

- **5. 할당 연산자**
    - **'='** (기본 할당)
    - **'+=', '-=', '*=', '/=', '%='** (복합 할당 연산자)
    - **'&=', '|=', '^=', '<<=', '>>=', '>>>='** (비트 복합 할당 연산자)

- **6. 시프트 연산자**
    - **'<<'** (왼쪽 시프트)
    - **'>>'** (오른쪽 시프트, 부호 유지)
    - **'>>>'** (오른쪽 시프트, 부호 비트 없음)

## 2.3 예제 코드

```java
public class BinaryOperatorsExample {
    public static void main(String[] args) {
        int a = 10, b = 5;
        int sum = a + b; // 15
        int difference = a - b; // 5
        boolean isEqual = (a == b); // false
        boolean isGreater = (a > b); // true

        int bitAnd = a & b; // 비트 AND 연산
        int shiftedLeft = a << 2; // 왼쪽으로 2 비트 시프트

        System.out.println("Sum: " + sum);
        System.out.println("Difference: " + difference);
        System.out.println("Is Equal: " + isEqual);
        System.out.println("Is Greater: " + isGreater);
        System.out.println("Bitwise AND: " + bitAnd);
        System.out.println("Left Shifted: " + shiftedLeft);
    }
}
```
- 이항 연산자들은 기본적인 산술 연산부터 복잡한 논리 연산에 이르기까지 프로그래밍에서 광범위하게 사용됩니다.
    - 이를 통해 효과적으로 데이터를 조작하고, 조건을 평가하며, 복잡한 문제를 해결할 수 있습니다.

---

## 3.1 삼항 연산자(Ternary Operator).
자바 프로그래밍에서 삼항 연산자(Ternary Operator), 또는 조건 연산자(Conditional Operator)라고 불리는 **'?:'** 는 세 개의 피연산자를 사용하는 유일한 연산자입니다.

이 연산자는 간결한 조건문을 구현할 때 사용되며, 간단한 조건식을 기반으로 두 가지 선택지 중 하나를 반환합니다.

## 3.2 삼항 연산자의 구조.

```
조건식 ? 값1 : 값2
```

- **1. 조건식 :** 이 부분은 **'true'** 또는 **'false'** 를 반환하는 불리언 식입니다.
- **2. 값2 :** 조건식이 **'true'** 일 때 반환됩니다.
- **3. 값3 :** 조건식이 **'false'** 일 때 반환됩니다.

조건식의 평가 결과에 따라 **'값1'** 또는 **값2** 중 하나가 결과값으로 선택되어 반환됩니다.

이 연산자는 일반적으로 간단한 조건에 따라 변수에 값을 할당하거나 특정 표현식의 결과를 결정할 때 유용하게 사용됩니다.

## 3.3 예제 사용.

```java
public class TernaryExample {
    public static void main(String[] args) {
        int a = 10, b = 5;
        int max = (a > b) ? a : b; // a와 b 중 큰 값을 max에 할당.
        System.out.println("Max value: " + max);
        
        String response = (a > b) ? "a is greater than b" : " b is greater or equal to a";
        System.out.println(response);
    }
}
```
- 이 예제에서 삼항 연산자를 사용하여 두 숫자 중 더 큰 숫자를 결정하고, 문자열 메시지도 조건에 따라 선택합니다.
    - 이러한 사용은 코드를 더 간결하게 만들고, **'if-else'** 구조를 보다 간단하게 대체할 수 있게 해 줍니다.
- 삼항 연산자는 그 효율성과 간결함 때문에 자바 프로그래밍에서 자주 사용되는 유용한 도구입니다.
    - 그러나 복잡한 로직이나 여러 조건이 연속적으로 필요한 경우에는 가독성을 위해 전통적인 **'if-else'** 문을 사용하는 것이 더 나을 수 있습니다.
