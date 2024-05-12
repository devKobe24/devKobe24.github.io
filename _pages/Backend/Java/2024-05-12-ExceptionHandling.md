---
title: "☕️[Java] 예외 처리"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-05-12"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 1️⃣ 예외 처리

예외 처리가 무엇인지 이해하고, 예외 처리 방법에 대해 직접 구현

## 1. 예외(Exception)
자바 프로그래밍에서 **"예외(Exception)"** 란 프로그램 실행 중에 발생하는 비정상적인 조건 또는 오류를 의미합니다.
이는 프로그램의 정상적인 흐름을 방해하며, 적절히 처리하지 않으면 프로그램이 비정상적으로 종료될 수 있습니다.
자바에서는 이러한 예외를 효과적으로 처리하기 위해 강력한 예외 처리 메커니즘을 제공합니다.

## 1.2 예외의 유형.
자바에서 예외는 크게 두 가지 유형으로 나눌 수 있습니다.

- **1. Checked Execptions**
    - 컴파일 시간에 체크되는 예외로, 컴파일러가 해당 예외를 처리하도록 요구합니다.
    - 이 예외들은 주로 외부 시스템과의 상호 작요(파일 입출력, 네트워크 통신 등)에서 발생하며, 프로그래머가 이를 적절히 처리하도록 강제합니다.

- **2. Unchecked Exceptions**
    - 런타임에 발생하는 예외로, 주로 프로그래머의 실수로 인해 발생합니다.(예: 배령의 범위를 벗어나는 접근, null 참조 등.)
    - 이러한 예외는 컴파일러가 체크하지 않으므로, 개발자가 예측하고 적절히 처리할 필요가 있습니다.

---

## 2. 예외 처리(Exception Handling)
자바 프로그래밍에서 예외 처리는 프로그램 실행 중에 발생할 수 있는 예외적인 상황, 즉 오류나 문제를 안전하게 관리하고 대처하는 방법을 말합니다.
예외 처리를 통해 프로그램의 비정상적인 종료를 막고, 오류 발생 시 적절한 대응을 할 수 있도록 합니다.
이는 프로그램의 안정성과 신뢰성을 높이는 데 중요한 역할을 합니다.

## 2.1 예외 처리의 주요 구성 요소.

- **1. try 블록**
    - 예외가 발생할 가능성이 있는 코드를 이 블록 안에 넣습니다.
    - 만약 블록 안의 코드 실행 중에 예외가 발생하면, 즉시 해당 블록의 실행을 중단하고 **'catch'** 블록으로 제어를 넘깁니다.

- **2. catch 블록**
    - **'try'** 블록에서 발생한 특정 유형의 예외를 처리합니다.
    - 프로그램이 예외를 안전하게 처리할 수 있도록 적절한 로직을 구현할 수 있습니다.
    - 하나의 **'try'** 블록에 여러 **'catch'** 블록을 사용하여 다양한 종류의 예외를 각각 다르게 처리할 수 있습니다.

- **3. finally 블록**
    - 이 블록은 예외의 발생 여부롸 관계없이 실행되는 코드를 포함합니다.
    - 주로 사용되는 목적은 자원 해제와 같은 정리 작업을 수행하기 위함입니다.
    - 예를 들어 파일이나 네트워크 자원을 닫거나 해제할 때 사용됩니다.

- **4. throws 키워드**
    - 메소드 선언 시 사용되며, 해당 메소드가 예외를 직접 처리하지 않고 호출한 메소드로 예외를 전파하겠다는 것을 나타냅니다.
    - 이를 통해 예외 처리의 책임을 메소드 호출자에게 넘길 수 있습니다.

## 2.2 예외 처리 예제.
```java
public class ExceptionHandlingExample {
    public static void main(String[] args) {
        try {
            int result = divide(10, 0);
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            System.err.println("Arithmetic Exception: Division by zero is not allowed.");
        } finally {
            System.out.println("This block is always executed.");
        }
    }
    
    public static int divide(int numerator, in denominator) {
        return numerator / denominator; // This ca throw ArithmeticException if denominator is zero.
    }
}
```

- 이 예제에서 **'divide'** 메소드는 분모가 0일 때 **ArithmeticException** 을 발생시킬 수 있습니다.
    - **'try'** 블록 안에서 이 메소드를 호출하고, 예외가 발생하면 **'catch'** 블록에서 이를 잡아서 적절한 오류 메시지를 출력합니다.
    - 또한, **'finally'** 블록은 예외 발생 여부와 상관없이 실행되어 어떤 상황에서도 실행될 필요가 있는 코드를 포함할 수 있습니다.

---

## 3. throw 키워드.
자바 프로그래밍에서 **'throw'** 키워드는 개발자가 의도적으로 예외를 발생시키기 위해 사용합니다.
이를 통해 특정 상황에서 프로그램의 흐름을 제어하거나, 특정 조건에서 오류를 발생시켜 예외 처리 메커니즘을 테스트하거나 강제할 수 있습니다.
**'throw'** 는 예외 객체를 생성하고 이를 던집니다(throw)
즉, 프로그램의 정상적인 실행 흐름을 중단하고 예외 처리 루틴으로 제어를 이동시킵니다.

## 3.1 'throw' 사용법.
**'throw'** 를 사용할 때는 예외 객체를 생성해야 합니다.
이 객체는 **'Throwable'** 클래스 또는 그 하위 클래스의 인스턴스여야 합니다.
자바에서는 대부분 **'Exception'** 클래스 또는 그 서브클래스를 사용하여 예외를 생성하고 던집니다.

### 예제.
```java
public class Main {
    public static void main(String[] args) {
        try {
            checkAge(17);
        } catch (Exception e) {
            System.out.println("Exception caught: " + e.getMessage());
        }
    }
    
    static void checkAge(int age) throws Execption {
        if (age < 18) {
            throw new Exception("Access denied - You must be at least 18 years old.");
        }
        System.out.println("Access granted - You are old enough!");
    }
}
```
- 이 예제에서 **'checkAge'** 메소드는 나이를 확인하고, 18세 미만인 경우 예외를 던집니다.
    - 이 예외는 **'throw new Exception(...)'** 을 통해 생성되고 던져집니다.
    - 메인 메소드에서는 이 메소드를 **'try'** 블록 안에서 호출하고, **'catch'** 블록을 통해 예외를 잡아서 처리합니다.
        - 결과적으로, 사용자가 18세 미만이면 "Access denided" 메시지를 포함하는 예외가 출력됩니다.

## 3.2 'throw'와 'throws'의 차이
- **'throw' :** 예외를 실제로 발생시키는 행위입니다. 이는 메소드 내부에서 특정 조건에서 예외를 발생시킬 때 사용됩니다.
- **'throws' :** 메소드 선언에 사용되며, 해당 메소드가 실행되는 동안 발생할 수 있는 예외를 명시적으로 선언합니다. 이는 호출자에게 해당 메소드를 사용할 때 적절한 예외 처리가 필요하다는 것을 알립니다.

---

## 4. throws 키워드.
자바 프로그래밍에서 **'throws'** 키워드는 메소드 선언에 사용되며, 해당 메소드가 실행 도중 발생할 수 있는 특정 유형의 예외를 명시적으로 선언하는 데 사용됩니다.
**'throws'** 는 메소드가 예외를 직접 처리하지 않고, 대신 이를 호출한 메소드로 예외를 "던져"(전파하는) 사실을 알립니다.
이를 통해 예외 처리 책임을 메소드 호출자에게 넘기는 것입니다.

## 4.1 'throws' 사용의 목적.
- **명시성**
    - 메소드가 발생시킬 수 있는 예외를 명시함으로써, 이 메소드를 사용하는 다른 개발자들에게 해당 메소드를 사용할 때 어떤 예외들을 처리해야 하는지 명확하게 알릴 수 있습니다.

- **강제 예외 처리**
    - **'throws'** 로 선언된 예외는 대부분 "checked exception" 이며, 이는 메소드를 호출하는 코드가 반드시 이 예외들을 처리하도록 강제합니다(try-catch 블록을 사용하거나, 또 다시 **'throws'** 로 예외를 전파하도록 함).

## 4.2 'throws' 사용법 예제.
아래 예제에서는 **'throws'** 를 사용하여 **'IOException'** 을 처리하는 방법을 보여줍니다.
이 예외는 파일 입출력 작업에서 자주 발생합니다.

```java
import java.io.*;

public class Main {
    public static void main(String[] args) {
        try {
            readFile("example.txt");
        } catch (IOExecption e) {
            System.out.println("An error occurred: " + e.getMessage());
        }
    }
    
    public static void readFile(String filename) throws IOException {
        File file = new File(filename);
        FileInputStream fis = null;
        try {
            fis = new FileInputStream(file);
            int content;
            while ((content = fis.read()) != -1) {
                // Process the content
                System.out.print((char) content);
            }
        } finally {
            if (fis != null) {
                fis.close();
            }
        }
    }
}
```

- 이 예제에서 **'readFile'** 메소드는 파일을 읽을 때 발생할 수 있는 **IOException** 을 처리하지 않고, 대신 **'throws'** 키워드를 사용하여 이 예외를 메소드를 호출한 **'main'** 메소드로 전달합니다.
    - **'main'** 메소드는 이 예외를 **'catch'** 블록을 통해 처리합니다.
