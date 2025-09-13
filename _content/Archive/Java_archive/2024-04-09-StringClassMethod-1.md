---
title: "☕️[Java] String 클래스 - 주요 메서드 1"
tags:
    - Java
    - Programming Language
date: "2024-04-09"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# String 클래스 - 주요 메서드 1

## 주요 메서드 목록
`String` 클래스는 문자열을 편리하게 다루기 위한 다양한 메서드를 제공합니다.
여기서는 자주 사용하는 기능 위주로 나열했습니다.
참고로 기능이 너무 많기 때문에 메서드를 외우기 보다는 주로 사용하는 메서드가 이런 것이 있구나 대략 알아두고, 필요할 때 검색하거나 API 문서를 통해서 원하는 기능을 찾는 것이 좋습니다.

**문자열 정보 조회**
- `length()` : 문자열의 길이를 반환합니다.
- `isEmpty()` : 문자열이 비어 있는지 확인합니다. (길이가 0).
- `isBlank()` : 문자열이 비어 있는지 확인합니다. (길이가 0이거나 공백(Whitespace)만 있는 경우), 자바 11
- `charAt(int index)`: 지정된 인덱스에 있는 문자를 반환합니다.

**문자열 비교**
- `equals(Object anObject)` : 두 문자열이 동일한지 비교합니다.
- `equalsIgnoreCase(String anotherString)` : 두 문자열을 대소문자 구분 없이 비교합니다.
- `compareTo(String anotherString)` : 두 문자열을 사전 순으로 비교합니다.
- `compareToIgnoreCase(String str)` : 두 문자열을 대소문자 구분 없이 사전적으로 비교합니다.
- `startWith(String prefix)` : 문자열이 특정 접두사로 시작하는지 확인합니다.
- `endWith(String suffix)` : 문자열이 특정 접미사로 끝나는지 확인합니다.

**문자열 검색**
- `contains(CharSequence s)` : 문자열이 특정 문자열을 포함하고 있는지 확인합니다.
- `indexOf(String ch)` / `indexOf(String ch, int fromIndex)`: 문자열이 처음 등장하는 위치를 반환합니다.
- `lastIndexOf(String ch)` : 문자열이 마지막으로 등장하는 위치를 반환합니다.

**문자열 조작 및 변환**
- `substring(int beginIndex)` / `substring(int beginIndex, int endIndex)` : 문자열의 부분 문자열을 반환합니다.
- `concat(String str)` : 문자열의 끝에 다른 문자열을 붙입니다.
- `replace(CharSequence target, CharSequence replacement)` : 특정 문자열을 새 문자열로 대체합니다.
- `replaceAll(String regex, String replacement)` : 문자열에서 정규 표현식과 일치하는 부분을 새 문자열로 대체합니다.
- `replaceFirst(String regex, String replacement)` : 문자열에서 정규 표현식과 일치하는 첫 번째 부분을 새 문자열로 대체합니다.
- `toLowerCase()` / `toUpperCase()` : 문자열을 소문자나 대문자로 변환합니다.
- `trim()` : 문자열 양쪽 끝의 공백을 제거합니다. 단순 `Whitespace`만 제거할 수 있습니다.
- `strip()` : `Whitespace` 와 유니코드 공백을 포함해서 제거합니다, 자바 11

**문자열 분할 및 조합.**
- `split(String regex)` : 문자열을 정규 표현식을 기준으로 분할합니다.
- `join(CharSequence delimiter, CharSequence... elements)` : 주어진 구분자로 여러 문자열을 결합합니다.

**기타 유틸리티.**
- `valueOf(Object obj)` : 다양한 타입을 문자열로 변환합니다.
- `toCharArray()` : 문자열을 문자 배열로 변환합니다.
- `format(String format, Object... args)` : 형식 문자열과 인자를 사용하여 새로운 문자열을 생성합니다.
- `matches(String regex)` : 문자열이 주어진 정규 표현식과 일치하는지 확인합니다.

이제 본격적으로 하나씩 알아봅시다.

> **참고: `CharSequence`** 는 `String`, `StringBuilder`의 상위 타입입니다.
> 문자열을 처리하는 다양한 객체를 받을 수 있습니다.

## 문자열 정보 조회
- `length()` : 문자열의 길이를 반환합니다.
- `isEmpty()` : 문자열이 비어 있는지 확인합니다. (길이가 0).
- `isBlank()` : 문자열이 비어 있는지 확인합니다. (길이가 0이거나 공백(Whitespace)만 있는 경우), 자바 11
- `charAt(int index)`: 지정된 인덱스에 있는 문자를 반환합니다.

```java
package lang.string.method;

public class StringInfoMain {

  public static void main(String[] args) {
    String str = "Hello, Java!";
    System.out.println("문자열의 길이: " + str.length());
    System.out.println("문자열이 비어 있는지: " + str.isEmpty());
    System.out.println("문자열이 비어 있거나 공백인지 1: " + str.isBlank());
    System.out.println("문자열이 비어 있거나 공백인지 2: " + "          ".isBlank());

    char c = str.charAt(7);
    System.out.println("7번째 인덱스의 문자 = " + c);
  }
}
```

**실행 결과**
```
문자열의 길이: 12
문자열이 비어 있는지: false
문자열이 비어 있거나 공백인지 1: false
문자열이 비어 있거나 공백인지 2: true
7번째 인덱스의 문자 = J
```

## 문자열 비교
- `equals(Object anObject)` : 두 문자열이 동일한지 비교합니다.
- `equalsIgnoreCase(String anotherString)` : 두 문자열을 대소문자 구분 없이 비교합니다.
- `compareTo(String anotherString)` : 두 문자열을 사전 순으로 비교합니다.
- `compareToIgnoreCase(String str)` : 두 문자열을 대소문자 구분 없이 사전적으로 비교합니다.
- `startWith(String prefix)` : 문자열이 특정 접두사로 시작하는지 확인합니다.
- `endWith(String suffix)` : 문자열이 특정 접미사로 끝나는지 확인합니다.

```java
package lang.string.method;

public class StringComparisonMain {

  public static void main(String[] args) {
    String str1 = "Hello, Java!"; // 대문자 일부 있음
    String str2 = "hello, java!";
    String str3 = "Hello, World!";

    System.out.println("str equals str2: " + str1.equals(str2));
    System.out.println("str equalsIgnoreCase str2: " + str1.equalsIgnoreCase(str2));

    System.out.println("'a' compareTo 'b': " + "a".compareTo("b"));
    System.out.println("'b' compareTo 'a': " + "b".compareTo("a"));
    System.out.println("'c' compareTo 'a': " + "c".compareTo("a"));

    System.out.println("str1 compareTo str3: " + str1.compareTo(str3));
    System.out.println("str1 compareToIgnoreCase str2: " + str1.compareToIgnoreCase(str2));

    System.out.println("str1 starts with 'Hello': " + str1.startsWith("Hello"));
    System.out.println("str1 ends with 'Java!': " + str1.endsWith("Java!"));
  }
}
```

**실행 결과**
```
str equals str2: false
str equalsIgnoreCase str2: true
'a' compareTo 'b': -1
'b' compareTo 'a': 1
'c' compareTo 'a': 2
str1 compareTo str3: -13
str1 compareToIgnoreCase str2: 0
str1 starts with 'Hello': true
str1 ends with 'Java!': true
```

## 문자열 검색
- `contains(CharSequence s)` : 문자열이 특정 문자열을 포함하고 있는지 확인합니다.
- `indexOf(String ch)` / `indexOf(String ch, int fromIndex)`: 문자열이 처음 등장하는 위치를 반환합니다.
- `lastIndexOf(String ch)` : 문자열이 마지막으로 등장하는 위치를 반환합니다.

```java
package lang.string.method;

public class StringSearchMain {

  public static void main(String[] args) {
    String str = "Hello, Java! Welcome to Java world.";

    System.out.println("문자열에 'Java'가 포함되어 있는지: " + str.contains("Java"));
    System.out.println("'Java'의 첫 번째 인덱스: " + str.indexOf("Java"));
    System.out.println("인덱스 10부터 'Java'의 인덱스: " + str.indexOf("Java", 10));
    System.out.println("'Java'의 마지막 인덱스: " + str.lastIndexOf("Java"));
  }
}
```

**실행 결과**
```
문자열에 'Java'가 포함되어 있는지: true
'Java'의 첫 번째 인덱스: 7
인덱스 10부터 'Java'의 인덱스: 24
'Java'의 마지막 인덱스: 24
```
