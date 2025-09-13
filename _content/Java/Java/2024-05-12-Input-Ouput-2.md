---
title: "☕️[Java] 입출력(2)"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-05-12"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 1️⃣ 입출력(2)

## 1. 파일 출력.
자바 프로그래밍에서 파일 출력은 프로그램이 데이터를 쓰는 과정을 말합니다.
이 과정을 통해 프로그램은 실행 결과를 저장하거나, 사용자가 입력한 정보를 파일에 기록하고, 다른 프로그램이나 나중에 프로그램 자체가 다시 사용할 수 있는 형태로 데이터를 출력할 수 있습니다.

## 2. 파일 출력을 수행하기 위한 기본 방법들.
- **1. FileOutputStream 사용**
    - **'FileOutputStream'** 클래스는 바이트 단위의 출력을 파일에 직접 쓸 때 사용됩니다.
    - 이 클래스를 사용하면 이미지, 비디오 파일, 이진 데이터 등을 파일로 저장할 수 있습니다.
```java
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputExample {
    public static void main(String[] args) {
        String data = "Hello, this is a test.";
        try (FileOutputStream out = new FileOutputStream("output.txt")) {
            out.write(data.getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- **2. PrintWriter 사용**
    - **'PrintWriter'** 는 문자 데이터를 출력할 때 사용됩니다.
    - 이 클래스는 파일에 텍스트를 쓸 때 편리하며, 자동 플러싱 기능, 줄 단위 출력 등의 메소드를 제공합니다.
```java
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

public class PrintWriteExample {
    public static void main(String[] args) {
        try (PrintWriter writer = new PrintWriter(new FileWriter("output.txt", true))) {
            writer.println("Hello, this is a test.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- **3. FileWriter 사용**
    - **'FileWriter'** 는 자바에서 파일에 텍스트 데이터를 쓰기 위한 간편한 방법 중 하나입니다.
    - 이 클래스는 내부적으로 문자 데이터를 파일에 쓸 수 있도록 **'OutputStreamWriter'** 를 사용하여 바이트 스트림을 문자 스트림으로 변환합니다.
    - **'FileWriter'** 는 텍스트 파일을 쉽게 작성할 수 있도록 해주며, 생성자를 통해 다양한 방식으로 파일을 열 수 있습니다.
```java
import java.io.FileWriter;
import java.io.IOException;

public class FileWriterExample {
    pulbic static void main(String[] args) {
        try (FileWriter writer = new FileWriter("output.txt", true)) {
            writer.write("Hello, this is a test.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- **4. BufferedWriter 사용**
    - **'BufferedWrite'** 는 버퍼링을 통해 효율적으로 파일에 문자 데이터를 쓸 수 있도록 합니다.
    - **'FileWriter'** 와 함께 사용되어, 더 큰 데이터를 처리할 때 성능을 개선합니다.
```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class BufferedWriterExample {
    public static void main(String[] args) {
        String content = "Hello, this is a test.";
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"))) {
            writer.write(content);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
---

## 2. 파일 입력.

자바 프로그래밍에서 파일 입력은 프로그램이 파일로부터 데이터를 읽어들이는 과정을 말합니다.

이 데이터는 텍스트나 바이너리 형태일 수 있으며, 파일에서 데이터를 읽어 프로그램 내에서 사용할 수 있도록 만드는 것이 목적입니다.

파일 입력을 위해 자바는 다양한 입출력 클래스를 제공합니다.

## 2.1 주로 사용되는 파일 입력 방법.

- **1. FileInputStream 사용**
    - **'FileInputStream'** 은 바이트 단위로 파일에서 데이터를 읽는 데 사용됩니다.
    - 이 클래스는 이미지, 비디오 파일, 실행 파일등의 이진 데이터 처리에 주로 사용됩니다.
```java
import java.io.FileInputStream;
import java.io.IOException;

public class FileInputStreamExample {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("input.dat")) {
            int content;
            while ((content = fis.read()) != -1) {
                // content 변수에 한 바이트씩 읽어들인 데이터를 저장
                System.out.print((char) content);
            }
        } catch (IOExecption e) {
            e.printStackTrace();
        }
    }
}
```

- **2. BufferedRead** 와 **FileReader 사용**
    - **'BufferedReader'** 와 **'FileReader'** 는 텍스트 데이터를 효과적으로 읽기 위해 함께 사용됩니다.
    - **'FileReader'** 는 파일에서 문자 데이터를 읽어들이며, **'BufferedReader'** 는 버퍼링을 통해 읽기 성능을 향상 시킵니다.
```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class BufferedReaderExample {
    public static void main(String[] args) {
        try (BufferedReader br new BufferedReader(new FileReader("input.txt"))) {
            String line;
            while ((line = br.readline()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- **3. Scanner 사용**
    - **'Scanner'** 클래스는 텍스트 파일을 읽을 때 유용하며, 특히 토큰화(tokenizing)된 데이터를 처리할 때 편리합니다.
    - **'Scanner'** 는 정규식을 사용하여 입력을 구분자로 분리하고, 다양한 타입으로 데이터를 읽어들일 수 있습니다.
```java
import java.io.File;
import java.util.Scanner;

public class ScannerExample {
    public static void main(String[] args) {
        try (Scanner scanner = new Scanner(new File("input.txt"))) {
            while (scanner.hasNextLine()) {
                System.out.println(scanner.nextLine());
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## 2.2 📝 정리.
이렇게 다양한 방법을 통해 파일로부터 데이터를 읽을 수 있으며, 각 방법은 사용하는 데이터 타입과 처리할 데이터의 양에 따라 선택할 수 있습니다.
파일에서 데이터를 읽는 것은 데이터를 처리하거나, 설정 정보를 불러오거나, 사용자 데이터를 읽는 등 다양한 목적으로 활용됩니다.
