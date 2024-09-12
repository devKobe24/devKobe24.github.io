---
title: "☕️[Java] java.lang 패키지 소개"
tags:
    - Java
    - Programming Language
date: "2024-03-28"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# java.lang 패키지 소개.
자바가 기본적으로 제공하는 라이브러리(클래스 모음)중에 가장 지본이 되는 것이 바로 `java.lang` 패키지입니다.

여기서 `lang`은 `Language`(언어)의 줄임말 입니다.

쉽게 이야기해서 자바 언어를 이루는 가장 기본이 되는 클래스들을 보관하는 패키지를 뜻합니다.

## java.lang 패키지의 대표적인 클래스들.
- `Object` : 모든 자바 객체의 부모 클래스
- `String` : 문자열
- `Integer`, `Long`, `Double` : 래퍼 타입, 기본형 데이터 타입을 객체로 만든 것
- `Class` : 클래스 메타 정보
- `System`: 시스템과 관련된 기본 기능들을 제공

**"여기 나열한 클래스들은 자바 언어의 기본을 이루기 때문에 반드시 잘 알아두어야 합니다."**

## import 생략 가능
`java.lang` 패키지는 모든 자바 애플리케이션에 자동으로 임포트(`import`)됩니다.

따라서 임포트 구문을 사용하지 않아도 됩니다.

다른 패키지에 있는 클래스를 사용하려면 다음과 같이 임포트를 사용해야 합니다.

```java
package lang;

import java.lang.System;

public class LangMain {
    
    public static void main(String[] args) {
        System.out.println("hello.java");
    }
}
```

`System` 클래스는 `java.lang` 패키지 소속입니다. 따라서 다음과 같이 임포트를 생략할 수 있습니다.

```java
package lang;

public class LangMain {

  public static void main(String[] args) {
    System.out.println("hello, java");
  }
}
```
- `import java.lang.System;` 코드를 삭제해도 정상 동작합니다.
