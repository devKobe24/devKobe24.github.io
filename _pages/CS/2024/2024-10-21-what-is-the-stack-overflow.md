---
title: "💾 [CS] 스택 오버플로우(Stack Overflow)란 무엇일까요?"
tags:
    - CS
date: "2024-10-21"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 스택 오버플로우(Stack Overflow)란 무엇일까요?
- **스택 오버플로우(Stack Overflow)는 프로그램이 사용 가능한 스택 메모리(Stack Memory) 영역을 초과하여 더 이상 데이터를 쌓을 수 없게 되는 현상을 말합니다.**
- 이로 인해 프로그램이 **비정상적으로 종료**되거나 **시스템 에러가** 발생하게 됩니다.

## 1️⃣ 스택(Stack)이란?
- **스택(Stack)은** 프로그램 실행 시 **함수 호출, 로컬 변수 등을** 저장하는 메모리 영역입니다.
    - 스택(Stack)은 **후입선축(LIFO: Last In, First Out)** 방식으로 작동하며, 함수가 호출될 때마다 해당 함수의 정보(예: 매개변수, 로컬 변수, 반환 주소 등)가 스택(Stack)에 **"쌓이고(push)"**, 함수가 졸요되면 스택(Stack)에서 **"제거(pop)"됩니다.**

## 2️⃣ 스택 오버플로우(Stack Overflow)의 원인.

### 1️⃣ 무한 재귀 호출(Recursive Call)
- **스택 오버플로우(Stack Overflow)가** 가장 흔히 발생하는 경우는 **재귀 함수**가 종료되지 않고 무한히 호출될 때입니다.
- 재귀 함수는 자기 자신을 호출하면서 매번 새로운 **스택 프레임(Stack frame)을** 쌓게 되는데, 종료 조건 없이 계속 호출되면 스택에 계속 쌓이게 되어 **스택 메모리(Stack Memory)를 초과하게 됩니다.**

### 👉 예시
```java
public class StackOverflowExample {
    public static void recursiveMethod() {
        // 재귀 호출 - 종료 조건이 없어 무한 호출됨
        recursiveMethod();
    }
    
    public static void main(String[] args) {
        recursiveMethod();
    }
}
```

- 위 코드에서는 `recursiveMethod()`가 자기 자신을 계속해서 호출하기 때문에 **스택 오버플로우(Stack Overflow)가** 발생하게 됩니다.

### 2️⃣ 지나치게 깊은 함수 호출 체인.
- 여러 함수가 서로를 호출하는 상황에서도 스택 오버플로우(Stack Overflow)가 발생할 수 있습니다.
- 예를 들어, `A()` 함수가 `B()`를 호출하고, `B()`가 `C()`를 호출하고, ... 이 과정을 매우 깊게 반복하면 스택 메모리(Stack Memory)가 초과될 수 있습니다.

> 📝 스택 메모리(Stack Memory)
> 
> **프로그램 실행 중에 함수 호출과 관련된 임시 데이터를 저장하는 메모리 영역**입니다.
> 주로 **지역 변수, 함수 매개변수, 리턴 주소 등**의 데이터를 저장하며, 함수가 호출될 때마다 **새로운 스택 프레임(Stack Frame)이** 생성되고, 함수가 종료되면 해당 스텍 프레임이 제거됩니다.
> 
> 스택 메모리(Stack Memory)는 **LIFO(Last In, First Out, 후입선출)** 구조로 작동합니다.
> 즉, **마지막에 저장된 데이터가 먼저 제거**되며, 함수 호출이 중첩될수록 스택(Stack)의 깊이가 깊어집니다

### 3️⃣ 너무 큰 로컬 변수 할당.
- 함수 내부에서 **너무 큰 크기의 배열이나 객체를 로컬 변수로 선언**할 때도 스택 오버플로우(Stack Overflow)가 발생할 수 있습니다.
- 스택(Stack)은 보통 **크기가 제한된 메모리 영역**이기 때문에, 너무 많은 메모리를 사용하는 로컬 변수를 선언하면 스택 메모리(Stack Memory)를 초과하게 됩니다.

## 3️⃣ 스택 오버플로우의 결과.
- 스택 오버플로우(Stack Overflow)가 발생하면 **프로그램이 비정상적으로 종료**되거나, **JVM(Java Virtual Machine)에서 `StackOverflowError`와** 같은 에러가 발생합니다.
- **재귀 함수 호출**이나 **과도한 함수 호출 체인**을 사용할 때, 이런 문제가 발셍힐 수 있음을 인지하고 예방하는 것이 중요합니다.

## 4️⃣ 스택 오버플로우(StackOverflow)를 방지하는 방법.

### 1️⃣ 재귀 함수의 종료 조건 확인.
- 재귀 함수를 사용할 때는 반드시 **종료 조건(Base case, 베이스 케이스)을** 명확히 정의해야 합니다.
    - 종료 조건(Base case, 베이스 케이스)이 없다면 무한히 호출되기 때문에 스택 오버플로우(StackOverflow)를 유발할 수 있습니다.

### 👉 예시
```java
public class FactorialExample {
    public static int factorial(int n) {
        if (n <= 1) {
            return 1; // 종료 조건: n이 1 이하일 때 재귀 호출 종료
        }
        return n * factorial(n - 1);
    }
    
    public static void main(String[] args) {
        System.out.println(factorial(5)); // 정상적으로 120 출력
    }
}
```

### 2️⃣ 재귀 대신 반복문 사용.
- **가능한 경우 재귀 함수** 대신 **반복문(loop)을** 사용하여 **스택 메모리(Stack Memory) 사용을 줄일 수 있습니다.**
- 반복문(loop)은 **스택 메모리(Stack Memory)** 대신 **힙 메모리(Heap Memory)나 CPU 레지스터(CPU Register)를** 사용하므로, 깊은 재귀 호출을 반복문으로 변경하면 스택 오버플로우(StackOverflow)를 방지할 수 있습니다.

### 3️⃣ 꼬리 재귀 최적화(Tail Recursion Optimization)
- 일부 언어는 **꼬리 재귀(tail recursion)를** 최적화하여 재귀 호출을 반복문처럼 처리할 수 있습니다.
    - 하지만 Java는 기본적으로 꼬리 재귀(tail recursion) 최적화를 지원하지 않기 때문에, 주의가 필요합니다.
- 꼬리 재귀(Tail Recursion)는 **재귀 호출이 함수의 마지막 작업**으로 실행될 때, 컴파일러가 **스택 메모리(Stack Memory)를** 재사용하여 스택 오버플로우(Stack Overflow)를 방지하는 방식입니다.

> 📝 꼬리 재귀(Tail Recursion)
> 
> **재귀 함수의 한 형태로, 함수의 마지막(꼬리) 동작이 자기 자신을 호출하는 것을** 말합니다.
> **즉, 재귀 호출 뒤에 추가적인 연산을 수행하지 않는** 재귀 방식입니다.
> 꼬리 재귀(Tail Recursion)는 **재귀 호출이 끝날 때 더 이상 처리할 작업이 남아 있지 않은 경우**에 해당합니다.

### 4️⃣ 적절한 스택 크기 설정.
- Java에서는 JVM(Java Virtual Machine)의 **스택 크기를** 설정하여 스택 오버 플로우(Stack Overflow)를 예방할 수 있습니다.
    - `-Xss` 옵션을 사용하여 스택 크기를 조정할 수 있습니다.
- 그러나, 스택 크기를 무작정 늘리는 것은 근본적인 해결책이 아니기 때문에 **재귀 호출**이나 **함수 설계를** 개선하는 것이 더 좋습니다.

### 👉 예시 
```bash
java -Xss2m MyProgram
```

- 위 명령은 Java 프로그램의 스택 크기를 **2MB로** 설정합니다.

## 5️⃣ 요약
- **스택 오버 플로우(Stack Overflow)는 함수 호출이 스택 메모리(Stack Memory)의 한계를 초과했을 때 발생하며, 주로 재귀 함수의 무한 호출**이나 **깊은 함수 호출 체인**에서 발생합니다.
- 이를 방지하기 위해 **재귀 함수의 종료 조건을 명확히 설정하고,** 필요한 경우 **반복문을** 사용하거나 **JVM(Java Virtual Machine)의 스택 크기**를 조정할 수 있습니다.
- 스택 오버플로우(Stack Overflow)를 피하기 위해서는 프로그램의 **메모리 사용**과 **함수 호출** 구조에 대한 철저한 이해와 관리가 필요합니다.
