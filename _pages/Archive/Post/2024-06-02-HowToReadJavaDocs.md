---
title: "📝[blog post] Java Docs 보는 방법."
tags:
    - Java
    - Programming Language
    - Backend
    - blogging
    - Documentation
date: "2024-06-02"
thumbnail: "/assets/img/thumbnail/blog.jpeg"
---

# 📝 Java Docs를 읽는 능력이 필요한 이유. :)

저는 Documentation이 그 어떤 유명 테크 블로거의 글 보다 중요하고 심도있게 읽어야 한다는 개인적인 의견이 있습니다.

그 이유는 Java를 개발한 개발자분들이 직접 만든 설명서나 다름 없기 때문입니다.

우리가 레고를 생각해 봅시다.

내가 좋아하는 레고를 사서 집에서 조립할 때 무엇을 보나요? 🤔

맞습니다!

레고 패키지 안에 들어있는 "설명서"를 기반으로 레고를 조립합니다.

레고를 디자인하고 만드신 분이 직접 "이렇게 순서대로 만들면 당신이 원하는 멋진 레고 완성품을 얻을 수 있습니다!" 라는 것을 직.간접적으로 보여주는 아주 자세한 설명이 들어있죠 📝

설명서는 직접 디자인하고 설계한 사람의 철학과 그들이 왜 그렇게 만들었는지 그리고 어떻게 쓰여야하는지 정확, 명료하게 명시되어 있습니다.

또한 다른 구성품과 맞춰볼 수 있는 것도 제안하거나 보여주기도 합니다.

그래서 Documentation을 보고 제대로 활용할 줄 아는 것이 개발자에게는 중요한 능력 중 하나가 아닐까 하는 생각을 합니다 🙋‍♂️

# 1️⃣ Java Documentation 보기.

## 1. 온라인 문서.

- Java SE Documentation은 [Oracle 공식 사이트](https://docs.oracle.com/en/java/javase/11/docs/api/index.html)에서 제공됩니다.
    - Java 버전에 따라 다른 문서가 제공되니, 사용하는 Java 버전에 맞는 문서를 선택해야 합니다.

## 2. IDE 내장 문서.

- 많은 통합 개발 환경(IDE)에는 JavaDoc을 쉽게 볼 수 있는 기능이 내장되어 있습니다. InteillJ IDEA, Eclipes, NetBeans 등에서 코드 작성 시 JavaDocs를 볼 수 있습니다.
    - 예를 들어, IntelliJ IDEA에서 클래스나 메소드 이름 위에 커서를 올리면 해당 클래스나 메소드의 JavaDoc이 팝업으로 표시됩니다.

## 3. 로컬 문서.

- Java JDK를 설치할 때, JavaDoc을 로컬에 다운로드할 수 있습니다. 이를 통해 인터넷 연결 없이도 문서를 참조할 수 있습니다.
- JDK 설치 경로 아래의 **`docs`** 폴더에 HTML 형식의 문서가 저장되어 있습니다.

# 2️⃣ Java Documentation 활용 방법

Java Documentation을 효과적으로 활용하는 방법을 알아봅시다.🤩

## 1. 클래스 및 메소드 탐색.

- API 문서에서 패키지, 클래스, 메소드, 필드 등의 세부 정보를 탐색할 수 있습니다.
    - 예를 들어, **`java.util`** 패키지에 어떤 클래스가 포함되어 있는지, **`ArrayList`** 클래스에 어떤 메소드가 있는지 등을 확인할 수 있습니다.

## 2. 사용 예제 찾기.

- 각 클래스와 메소드에는 사용 예제가 포함되어 있을 수 있습니다. 이러한 예제는 해당 API를 올바르게 사용하는 방법을 이해하는 데 도움이 됩니다.

## 3. 메소드 시그니처 및 설명.

- 메소드의 매개변수, 반환값, 예외 등을 설명하는 시그니처와 설명을 통해 메소드의 사용법을 정확히 알 수 있습니다.
    - 예를 들어, **`String`** 클래스의 **`substring`** 메소드의 시그니처와 설명을 보면, 매개변수로 전달해야 할 값과 반환되는 값에 대한 정보를 얻을 수 있습니다.

## 4. 상속 구조 및 인터페이스.

- 클래스가 구현하는 인터페이스와 상속받는 클래스에 대한 정보를 확인할 수 있습니다. 이를 통해 클래스의 기능을 확장하거나 인터페이스를 구현하는 방법을 이해할 수 있습니다.

# 3️⃣ 예제

다음은 Java Documentation을 활용하는 몇 가지 예제입니다.

## 예제 1: `ArrayList` 클래스의 메소드 사용법 확인 🙋‍♂️

1. **온라인 문서에서 `ArrayList` 클래스를 찾습니다.**
    - [Java SE Documentation](https://docs.oracle.com/en/java/javase/11/docs/api/index.html)에서 **`java.util.ArrayList`** 를 검색합니다.
    - **`ArrayList`** 클래스의 API 문서를 열어 메소드 목록을 확인합니다.

2. **`add(E e)` 메소드 사용법 확인하기.**
    - **`add(E e)`** 메소드는 리스트의 끝에 요소를 추가하는 메소드입니다.
    - 메소드 설명을 읽고, 예제를 확인하여 사용법을 이해합니다.

## 예제 2. `String` 클래스의 `substring` 메소드 사용법 확인 🙋‍♂️

1. **IDE 내장 문서 활용하기.**
    - IntelliJ IDEA나 Eclipse에서 **`String`** 클래스의 **`substring`** 메소드를 사용하려고 할 때, 메소드 이름 위에 커서를 올리면 JavaDoc이 표시됩니다.
    - JavaDoc을 통해 **`substring(int beingIndex, int endIndex)`** 메소드의 매개변수와 반환 값에 대한 설명을 읽습니다.

```java
public class Main {
    public static void main(String[] args) {
        String text = "Hello, World!";
        String subText = text.substring(7, 12); // "World"
        System.out.println(subText);
    }
}
```

위 예제에서 **`substring`** 메소드의 매개변수가 **`beginIndex`** 와 **`endIndex`** 임을 알 수 있으며, 이는 시작 인덱스부터 종료 인덱스 전까지의 문자열을 반환합니다.

## 예제 3. 예외 처리 방법 확인 🙋‍♂️

1. **예외 클래스 문서 확인하기.**
    - **`java.lang.NullPointerException`** 클래스의 문서를 확인하여 언제 이 예외가 발생하는지, 그리고 이를 어떻게 처리할 수 있는지에 대한 정보를 얻습니다.

2. **예외 처리 예제**

```java
public class Main {
    public static void main(String[] args) {
        try {
            String text = null;
            System.out.println(text.length());
        } catch (NullPointerException e) {
            System.out.println("Caught a NullPointerException");
        }
    }
}
```

이 예제는 **`NullPointException`** 이 발생할 때 이를 처리하는 방법을 보여줍니다.

# 📝 요약.

- **Java Documentation**은 Java API를 이해하고 사용하는 데 필수적인 자료입니다.
- **Java Documentation**를 온라인, IDE, 또는 로컬에서 접근할 수 있습니다.
- API 문서를 통해 클래스와 메소드의 세부 정보를 확인하고, 예제를 참고하여 올바르게 사용하는 방법을 배울 수 있습니다.
- 상속 구조와 인터페이스 구현 방법을 이해하여 코드의 재사용성과 확장성을 높일 수 있습니다.
