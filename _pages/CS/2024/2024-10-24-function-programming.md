---
title: "💾 [CS] 함수형 프로그래밍(Function Programming, FP)이란 무엇일까요?"
tags:
    - CS
date: "2024-10-24"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 함수형 프로그래밍(Function Programming, FP)이란 무엇일까요?
- **함수형 프로그래밍(Function Programming, FP)은 함수를 기본 구성 요소로 사용하는 프로그래밍 패러다임입니다.** 
- 함수형 프로그래밍(Function Programming, FP)에서는 **순수 함수**와 **데이터의 불변성**을 강조하며, **부작용(Side Effect)을** 피하는 것을 목표로 합니다.
- 이를 통해 **코드의 가독성, 유지보수성, 재사용성**을 높이고, **병렬 처리**와 같은 상황에서도 **안정적이고 예측 가능한 코드**를 작성할 수 있습니다.

## 1️⃣ 함수형 프로그래밍(Function Programming, FP)의 핵심 개념.

### 1️⃣ 순수 함수(Pure Function)
- 순수 함수(Pure Function)는 **입력값이 같으면 항상 같은 결과를 반환**하는 함수입니다.
    - 함수의 출력이 외부 상태에 의존하지 않고, 함수 내부에서 외부 상태를 변경하지도 않습니다.
- 순수 함수(Pure Function)는 **부작용(Side Effect)이** 없기 때문에, 테스트하기 쉽고, 함수의 동작을 예측할 수 있습니다.

### 👉 예시.
```java
// 순수 함수 예시
public int add(int a, int b) {
    return a + b; // 입력값이 같으면 항상 같은 결과를 반환
}
```

### 2️⃣ 불변성(Immutability)
- 함수형 프로그래밍에서는 데이터의 **불변성**을 중요하게 생각합니다.
    - 즉, 데이터가 생성된 후에는 **변경되지 않아야 하며, 필요하면 새로운 데이터를 생성합니다.**
- 불변성은 코드의 안정성을 높이고, **동시성 문제를** 피할 수 있게 해줍니다.

### 👉 예시.
```java
// Java에서의 불변 객체
final String name = "Kobe"; // name은 변경할 수 없는 값
```

> 📝 동시성 문제(Concurrency Issue)
> 
> **여러 작업이 동시에 실행될 때 발생할 수 있는 오류나 비정상적인 동작을 말합니다.** 
> 프로그램에서 **여러 스레드나 프로세스가 동시에 동일한 데이터나 자원에 접근하고 수정하려고 할 때** 동시성 문제(Concurrency Issue)가 발생할 수 있습니다.
> 특히 **데이터의 일관성과 안정성을 유지하는 것이** 어려워지며, 이는 **프로그램의 버그, 충돌, 예측하지 못한 동작**을 초래할 수 있습니다.

> 📝 스레드(Thread)
> 
> **프로세스 내에서 실행되는 가장 작은 단위의 작업 흐름**을 의미합니다.
> 프로세스가 **메모리, 파일 핸들, 네트워크 연결**과 같은 리소스를 포함하는 **독립적인 실행 단위**라면, 스레드(Thread)는 **프로세스 내부에서 실제로 코드가 실행되는 실행 흐름**입니다.
> **하나의 프로세스는 여러 개의 스레드(Thread)를 포함할 수 있으며,** 각 스레드(Thread)는 **독립적으로 실행**될 수 있습니다.

> 📝 프로세스(Process)
> 
> **컴퓨터에서 실행 중인 프로그램의 인스턴스**를 말합니다.
> 간단히 말해, **프로그램이 메모리에서 실행될 때 프로세스(Process)가 됩니다.**
> 컴퓨터에서 실행되는 모든 응용 프로그램(예: 웹 브라우저, 텍스트 편집기, 백그라운드 서비스 등)은 각각 하나의 프로세스(Process)입니다.

### 3️⃣ 고차 함수(Higher-Order Function)
- 고차 함수(Higher-Order Function)는 **다른 함수를 인자로 받거나, 결과로 함수를 반환**하는 함수입니다.
- 이를 통해 **함수를 조합하거나, 동적으로 함수를 생성**할 수 있으며, 코드의 재사용성을 높일 수 있습니다.

### 👉 예시.
```java
public static void main(String[] args) {
    Function<Integer, Integer> square = X -> X * X;
    System.out.println(applyFunction(square, 5)); // 25 출력
}

public static int applyFunction(Function<Integer, Integer> func, int value) {
    return func.apply(value);
}
```

### 4️⃣ 일급 시민(First-Class Citizen)
- 함수형 프로그래밍에서는 **함수가 일급 시민(First-Class Citizen)으로** 취급됩니다.
    - 이는 함수를 **변수에 할당하거나, 함수의 인자로 전달하거나, 함수의 반환값으로 사용할 수 있다**는 것을 의미합니다.
- 함수가 다른 데이터 타입(숫자, 문자열 등)처럼 취급되기 때문에 **동적으로 함수의 행동을 변경**할 수 있습니다.

> 📝 일급 시민(First-Class Citizen, 또는 First-Class Object)
> 
> 프로그래밍 언어에서 **변수나 함수가 다른 데이터 타입과 동일하게 취급되고, 자유롭게 사용할 수 있는 객체를 의미합니다.**
> 일급 시민(First-Class Citizen 또는 First-Class Object)으로 간주되는 개체는 **변수에 할당되거나, 함수의 인자(Arguments)로 전달되거나, 함수의 반환 값으로 사용할 수 있는 등 다양한 방식으로 활용될 수 있습니다.**
> 
> 이 개념은 주로 **함수형 프로그래밍(Function Programming, FP)에서** 사용되며, 특히 **함수를 일급 시민(First-Class Citizen 또는 First-Class Object)으로 취급할 수 있는지 여부가 그 언어의 함수형 프로그래밍(Function Programming, FP) 지원 수준을** 결정하는 중요한 요소가 됩니다.

### 5️⃣ 부작용(Side Effects) 없는 코드.
- 함수형 프로그래밍(Function Programming, FP)에서는 함수가 **부작용(Side Effect)을** 일으키지 않도록 작성합니다.
    - 부작용(Side Effects)이란 함수가 **외부 상태를 변경**하거나, **입출력 작업**을 수행하는 것을 말합니다.
- 부작용(Side Effects)이 없기 때문에, 코드의 예측 가능성과 재사용성이 높아집니다.

### 6️⃣ 함수 합성(Function Composition)
- 함수 합성(Function Composition)은 **여러 함수를 결합**하여 새로운 함수를 만드는 것을 말합니다.
    - 작은 함수들을 조합하여 **복잡한 연산을 처리**할 수 있게 합니다.
- 이는 재사용 가능한 작은 함수를 만들어, 더 큰 기능을 구현할 때 유용하게 사용할 수 있습니다.

## 2️⃣ 함수형 프로그래밍(Function Programming, FP)의 특징.

### 1️⃣ 코드의 가독성과 유지보수성.
- 함수형 프로그래밍(Function Programming, FP)에서는 **짧고 간결한 코드를** 작성할 수 있으며, 코드가 **명확하고 읽기 쉬워집니다.**
    - 각 함수가 독립적으로 동작하기 때문에, **모듈화(Modularization)와 테스트**가 용이합니다.

> 🙋‍♂️ [모듈화(Modularization)란 무엇일까요?](https://www.devkobe24.com/CS/2024/2024-10-23-modularization.html)

### 2️⃣ 병렬 처리와 동시성.
- 함수형 프로그래밍(Function Programmin, FP)의 **불변성** 덕분에, 여러 스레드(Thread)에서 **동시 실행**하더라도 데이터의 일관성이 유지됩니다.
- 따라서 병렬 처리를 할 때 **데이터 경합(Race Condition)** 문제가 발생하지 않으며, 안전하게 실행할 수 있습니다.

> 📝 데이터 경합 또는 레이스 컨디션(Race Condition).
> 
> **두 개 이상의 스레드(Thread) 또는 프로세스(Process)가 동시에 공유 자원에 접근하고, 그 결과가 실행 순서에 따라 달라지는 상황을 말합니다.**
> 즉, **동시성 프로그래밍(Concurrent Programming)에서** 스레드들이 **경쟁적으로 자원에 접근할 때** 발생하는 문제로, 프로그램의 **의도치 않은 결과**를 초래할 수 있습니다.
> 
> 레이스 컨디션(Race Condition)은 특히 **공유 자원(변수, 데이터 구조, 파일 등)에 대해 읽기와 쓰기 작업이 동시에 이루어지는 경우**에 발생하며, 동기화가 제대로 이루어지지 않으면 데이터의 일관성이 깨지게 됩니다.

### 3️⃣ 테스트 용이성.
- 순수 함수(Pure Function)는 **입력값과 출력값의 관계가 명확**하기 때문에 테스트하기 쉽습니다.
    - 또한, 부작용이 없기 때문에 독립적으로 테스트할 수 있어 **단위 테스트(Unit Testing)에** 적합합니다.

> 📝 단위 테스트(Unit Testing)
> 
> **소프트웨어의 개별 구성 요소(함수, 메서드, 클래스 등)가 올바르게 동작하는지 검증하는 테스트 방법입니다.**
> 단위 테스트(Unit Testing)의 목적은 **프로그램의 가장 작은 단위(단위, 유닛)가 예상대로 작동하는지 확인하는 것입니다.**
> 이를 통해 **각 구성 요소가 독립적으로 정확하게 동작**하는지를 보장할 수 있습니다.

## 3️⃣ 함수형 프로그래밍의 예시 (Java)
- Java 8부터는 함수형 프로그래밍(Function Programmig, FP) 스타일을 지원하기 위해 **람다 표현식(Lambda Expression)과 스트림(Stream) API를** 도입했습니다.

> 📝 람다 표현식(Lambda Expression)
> 
> **익명 함수(Anonymous Function)로, 이름이 없는 간단한 함수를 의미합니다.**
> 람다 표현식(Lambda Expression)을 사용하면 **함수를 보다 간결하고 직관적으로 정의할 수 있으며, 코드의 가독성을 높이고** 간단한 작업을 수행하는 데 유용합니다.
> 람다 표현식(Lambda Function)은 주로 **함수형 프로그래밍(Function Programming, FP)** 스타일을 지원하기 위해 도입되었으며, **Java 8부처 Java에서도 사용할 수 있게 되었습니다.**

> 🙋‍♂️ [Java Stream이란 무엇일까요?](https://www.devkobe24.com/Java/Java/2024-10-19-stream-api.html)

### 1️⃣ 람다 표현식(Lambda Expression)
- 람다 표현식(Lambda Expression)을 사용하면 **간결하게 함수**를 정의할 수 있습니다.

```java
// 기존 방식
public int add(int a, int b) {
    return a + b;
}

// 람다 표현식
BinaryOperator<Integer> add = (a, b) -> a + b;
System.out.println(add.apply(3,4)); // 7 출력
```

### 2️⃣ 스트림(Stream) API
- 스트림은 **컬렉션의 데이터를 필터링, 변환, 정렬, 집계할 수 있는 함수형 프로그래밍 스타일의 API입니다.**

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<Integer> nnumbers = Arrays.asList(1, 2, 3, 4, 5);
        
        // 짝수만 필터링하고, 각 숫자에 2를 곱한 리스트 생성
        List<Integer> result = numbers.stream()
            .filter(n -> n % 2 == 0)
            .map(n -> n * 2)
            .collect(Collectors.toList());
            
            System.out.println(result); // [4, 8] 출력
    }
}
```

## 4️⃣ 함수형 프로그래밍의 장점.

### 1️⃣ 코드의 간결성.
- 함수형 프로그래밍에서는 코드가 **짧고 직관적**이기 때문에, 읽기 쉽고 유지보수가 용이합니다.

### 2️⃣ 재사용성과 확장성.
- **작고 재사용 가능한 함수를** 작성하여, 다른 프로그램이나 프로젝트에서 **재사용할 수 있습니다.**

### 3️⃣ 병렬 처리의 용이성.
- **불변성 덕분에 병렬 처리 환경에서도** 안전하게 실행할 수 있으며, 효율적인 병렬 처리가 가능합니다.

### 4️⃣ 디버깅과 테스트의 용이성
- 순수 함수는 **독립적으로 테스트할 수 있어, 디버깅과 단위 테스트가 쉽습니다.**

## 5️⃣ 함수형 프로그래밍의 단점.

### 1️⃣ 러닝 커브.
- 함수형 프로그래밍의 개념에 익숙하지 않은 개발자에게는 **러닝 커브가 있을 수 있습니다.**
    - 특히 **람다 표현식(Lambda Expression)과 고차 함수(Higher-Order Function)에** 익숙해지는 데 시간이 필요할 수 있습니다.

### 2️⃣ 퍼포먼스 이슈.
- **불변성** 때문에 새로운 객체를 매번 생성해야 하는 경우 **메모리 사용량이 증가할 수 있으며,** 성능에 영향을 미칠 수 있습니다.
    - 따라서 상황에 따라 **성능 최적화를 고려해야 합니다.**

## 6️⃣ 요약.
- **함수형 프로그래밍**은 **순수 함수, 불변성, 부작용 없는 코드를 강조하여, 가독성과 유지보수성, 재사용성이 높은 코드를 작성하는 프로그래밍 패러다임입니다.**
- Java 8 이후의 람다 표현식(Lambda Expression)과 스트림(Stream) API는 함수형 프로그래밍(Function Programming ,FP) 스타일을 도입하여, 개발자가 더욱 간결하고 효율적인 코드를 작성할 수 있게 해줍니다.
- 함수형 프로그래밍은 **병렬 처리와 동시성 문제에 강점을 가지며, 작은 함수들을 조합하여 모듈화(Modularization)된 코드를 작성할 수 있어, 코드의 재사용성과 확장성을 높이는 데 유리합니다.**
