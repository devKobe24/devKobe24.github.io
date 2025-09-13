---
title: "☕️[Java] 입출력(1)"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-05-11"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 1️⃣ 입출력(1)

콘솔 입출력 방법에 대해 직접 구현

## 1. 콘솔 입력
자바에서 콘솔 입력을 받는 방법은 여러 가지가 있습니다.

주로 사용되는 몇 가지 방법들을 소개하겠습니다.

- **1. Scanner 클래스 사용**
    - **'Scanner'** 클래스는 자바의 **'java.util'** 패키지에 포함되어 있으며, 다양한 타입의 입력을 콘솔에서 받기 위해 널리 사용됩니다.
    - **'System.in'** 을 **'Scanner'** 객체에 연결하여 사용자로부터 입력을 받을 수 있습니다.
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Enter your name: ");
        String name = scanner.nextLine();
        System.out.println("Hello, " + name);
        scanner.close();
    }
}
```

- **2. BufferedReader** 와 **InputStreamReader 사용**
    - 이 방법은 **'java.io'** 패키지를 사용합니다.
    - **'InputStreamReader'** 는 바이트 스트림을 문자 스트림으로 변환하는데 사용되고, **'BufferedReader'** 는 텍스트 읽기를 효율적으로 할 수 있게 해 줍니다.
        - 이 방법은 예외 처리를 필요로하며, 라인 단위로 입력을 받습니다.
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new InputStreamReader(System.in))) {
            System.out.println("Enter your name: ");
            String name = reader.readLine();
            System.out.println("Hello, " + name);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- **3. Console 클래스 사용**
    - **'Console'** 클래스는 자바 6 이상에서 사용할 수 있으며, 콘솔에서 비밀번호와 같은 민감한 데이터를 읽을 때 유용합니다.
    - **'System.console()'** 메소드를 통해 **'Console'** 객체를 얻을 수 있으나, 이 방법은 IDE에서는 작동하지 않을 수 있습니다.(콘솔 환경에서만 사용 가능.)
```java
import java.io.Console;

public class Main {
    public static void main(String[] args) {
        Console console = System.console();
        if (console != null) {
            String name = console.readLine("Enter your name: ");
            char[] password = console.readPassword("Enter your password: ");
            console.printf("Hello, %s\n", name);
        } else {
            System.out.println("No console available");
        }
    }
}
```
