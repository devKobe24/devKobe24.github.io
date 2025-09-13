---
title: "💾 [CS] 스택 트레이스(Stack Trace)란 무엇일까요?"
tags:
    - CS
date: "2024-10-22"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 스택 트레이스(Stack Trace)란 무엇일까요?
- **스택 트레이스(Stack Trace)는 프로그램 실행 중 예외(Exception)가 발생했을 때, 예외가 발생한 지점과 그 예외로 이어진 함수 호출 경로를 보여주는 디버깅 정보입니다.**
- 이를 통해 개발자는 **어떤 예외가 어디서 발생했는지, 그리고 그 예외가 어떻게 발생했는지를** 파악할 수 있습니다.

## 1️⃣ 스택 트레이스(Stack Trace)의 역할.

### 1️⃣ 디버깅.
- 스택 트레이스(Stack Trace)를 통해 예외가 발생한 위치를 정확히 찾아내고, 문제를 신속하게 해결할 수  있습니다.

### 2️⃣ 문제의 원인 파악.
- 스택 트레이스(Stack Trace)는 예외가 발생하기 전까지 **어떤 함수가 호출되었는지를** 순서대로 보여주기 때문에 문제와 원인을 추적할 수 있는 중요한 단서를 제공합니다.

### 4️⃣ 호출 경로 이해.
- 프로그램에서 **함수 호출 경로**를 이해하는 데 도움을 주어, 코드의 **흐름을 파악하고 수정할 수 있게 합니다.**

## 2️⃣ 스택 트레이스의 구조.
- 스택 트레이스(Stack Trace)는 일반적으로 **예외의 종류와 메시지,** 그리고 **함수 호출 스택의 목록**으로 구성됩니다.
    - 각 항목은 다음 정보를 포함합니다.
        - 1. 예외의 종류와 메시지
        - 2. 호출된 메서드의 목록.

### 1️⃣ 예외의 종류와 메시지.
- 예외의 유형과 메시지를 통해 예외의 원인에 대한 기본적인 정보를 제공합니다.

### 2️⃣ 호출된 메서드의 목록.
- 예외가 발생한 시점까지의 함수 호출 경로를 위에서부터 아래로 표시합니다.
    - 가장 위의 항목은 **예외가 실제로 발생한 메서드이며, 그 아래는 예외가 발생하기까지 호출된 메서드들이 표시됩니다.**

## 3️⃣ 스택 트레이스 예시 (Java)
- 아래의 코드는 **Java** 프로그램에서 `NullPointerException`이 발생했을 때의 스택 트레이스 예시입니다.

```java
public class StackTraceExample {
    public static void main(String[] args) {
        firstMethod();
    }
    
    public static void firstMethod() {
        secondMethod();
    }
    
    public static void secondMethod() {
        String str = null;
        str.length(); // 여기에서 NullPointerException 발생
    }
}
```

### 👉 출력되는 스택 트레이스.
```php
Excption in thread "main" java.lang.NullPointerException
    at StackTraceExample.secondMethod(StackTraceExample.java:14)
    at StackTraceExample.firstMethod(StackTraceExample.javee:9)
    at StackTraceExample.main(StackTraceExample.java:5)
```

## 4️⃣ 스택 트레이스의 구성 요소.

### 1️⃣ 예외의 종류와 메시지.
- `Exception in thread "main" java.lang.NullPointerException`
    - 프로그램의 메인 스레드에서 `NullPointerException`이 발생했다는 것을 알려줍니다.

### 2️⃣ 호출된 메서드의 목록.
- `at StackTraceExample.secondMethod(StackTraceExample.java:14)`
    - `secondMethod` 메서드의 **14번째 줄**에서 예외가 발생했습니다.
- `at StackTraceExample.firstMethod(StackTraceExample.java:9)`
    - `firstMethod`가 `secondMethod`를 호출했습니다.
- `at StackTraceExample.main(StackTraceExample.java:5)`
    - `main` 메서드가 `firstMethod`를 호출했습니다.
- 스택 트레이스(Stack Trace)는 가장 **최근에 호출된 메서드(예외가 발생한 메서드)부터** 시작하여, 예외가 발생하기 전에 호출된 메서드들을 순차적으로 표시합니다.

## 5️⃣ 스택 트레이스의 중요성.

### 1️⃣ 오류의 원인 파악.
- 스택 트레이스를 보면, **예외가 발생한 정확한 위치**와 **어떤 함수에서 호출된 것인지**를 알 수 있습니다.
    - 이를 통해 오류의 원인을 신속하게 파악할 수 있습니다.
- 예를 들어, 위의 스택 트레이스(Stack Trace)에서는 `secondMethod`의 `str.length()` 호출에서 `NullPointerException`이 발생했음을 알 수 있습니다.
    - 그 이후에는 `firstMethod`가 이 메서드를 호출했고, 최종적으로 `main` 메서드에서 `firstMethod`가 호출되었습니다.

### 2️⃣ 디버깅 시간 단축.
- 스택 트레이스(Stack Trace)는 예외의 원인을 정확하게 지목하기 때문에, 개발자가 코드를 수정하고 디버깅하는 시간을 단축할 수 있습니다.
    - **디버거**를 사용하지 않고도 문제의 근본 원인을 빠르게 찾을 수 있습니다.

### 3️⃣ 호출 경로 이해.
- 코드의 흐름을 이해하는 데 두움을 줍니다.
    - 특히, **복잡한 프로그램**이나 **타사의 코드**를 다룰 때 스택 트레이스(Stack Trace)는 코드가 어떻게 작동하는지에 대한 중요한 단서를 제공합니다.

## 6️⃣ 스택 트레이스 읽는 방법.

### 1️⃣ 예외 메시지를 확인.
- 예외 메시지는 문제의 종류와 원인에 대해 첫 번째 힌트를 제공합니다.
    - 예를 들어, `NullPointException`은 **널 객체에 접근하려고 했다는 것**을 의미합니다.

### 2️⃣ 가장 위의 호출 위치 찾기.
- 스택 트레이스(Stack Trace)에서 가장 위의 호출 위치는 **예외가 발생한 정확한 위치**를 나타냅니다.
    - 이 부분부터 문제를 추적하기 시작해야 합니다.

### 3️⃣ 호출 순서대로 확인.
- 예외가 발생한 코드의 흐름을 이해하려면 호출 순서대로 각 메서드가 어떤 역할을 하는지 분석합니다.
    - 이 과정을 통해 문제가 있는 부분을 빠르게 찾을 수 있습니다.

## 7️⃣ 스텍 트레이스를 유용하게 사용하는 방법.

### 1️⃣ 로깅(logging)과 함꼐 사용.
- 실제로 배포된 소프트웨어에서는 오류가 발생하면 **로그에 스택 트레이스(Stack Trace)를** 기록하도록 설정하는 것이 중요합니다.
    - 이를 통해 사용자가 오류를 보고했을 때, 개발자가 쉽게 문제의 원인을 파악할 수 있습니다.
- Java에서는 예외를 잡을 때 `e.printStackTrace()`를 사용하여 스택 트레이스(Stack Trace)를 출력하거나, **로깅 프레임워크(logging framework)를** 사용하여 로그 파일에 기록할 수 있습니다.

### 2️⃣ 예외 처리에서 스택 트레이스 활용.
- 예외를 잡아서 처리할 때, 스택 트레이스를 출력함으로써 **디버깅**에 도움이 될 수 있습니다.
    - 특히, **예상하지 못한 예외**를 디버깅할 때 유용합니다.

## 8️⃣ 요약.
- **스택 트레이스(Stack Trace)는 프로그램에서 예외가 발생했을 때 예외의 원인과 호출 경로를 보여주는 디버깅 정보**입니다.
- 예외의 종류와 메시지, 그리고 함수 호출 스택 목록으로 구성되어 있으며, 문제를 추적하고 해결하는 데 큰 도움이 됩니다.
- 이를 통해 개발자는 코드의 흐름을 이해하고, 예외가 발생한 정확한 위치와 원인을 파악하여 **디버깅 시간을 단축할 수 있습니다.**
- 스택 트레이스(Stack Trace)는 특히 **로깅(logging)과 결합**하여 사용하면, 배포된 소프트웨어의 문제를 진단하는 데 유용합니다.
