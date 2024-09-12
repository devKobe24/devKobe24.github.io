---
title: "☕️[Java] String 클래스 - 주요 메서드 2"
tags:
    - Java
    - Programming Language
date: "2024-04-10"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# String 클래스 - 주요 메서드 2

## 문자열 조작 및 변환
- `substring(int beginIndex)` / `substring(int beginIndex, int endIndex)` : 문자열의 부분 문자열을 반환합니다.
- `concat(String str)` : 문자열의 끝에 다른 문자열을 붙입니다.
- `replace(CharSequence target, CharSequence replacement)` : 특정 문자열을 새 문자열로 대체합니다.
- `replaceAll(String regex, String replacement)` : 문자열에서 정규 표현식과 일치하는 부분을 새 문자열로 대체합니다.
- `replaceFirst(String regex, String replacement)` : 문자열에서 정규 표현식과 일치하는 첫 번째 부분을 새 문자열로 대체합니다.
- `toLowerCase()` / `toUpperCase()` : 문자열을 소문자나 대문자로 변환합니다.
- `trim()` : 문자열 양쪽 끝의 공백을 제거합니다. 단순 `Whitespace`만 제거할 수 있습니다.
- `strip()` : `Whitespace` 와 유니코드 공백을 포함해서 제거합니다, 자바 11

```java
package lang.string.method;

public class StringChangeMain2 {

  public static void main(String[] args) {
    String strWithSpaces = "   Java Programming ";

    System.out.println("소문자로 변환: " + strWithSpaces.toLowerCase());
    System.out.println("대문자로 변환: " + strWithSpaces.toUpperCase());

    System.out.println("공백 제거(trim): '" + strWithSpaces.trim() + "'");
    System.out.println("공백 제거(strip): '" + strWithSpaces.strip() + "'");
    System.out.println("앞 공백 제거(stripLeading): '" + strWithSpaces.stripLeading() + "'");
    System.out.println("뒤 공백 제거(stripTrailing): '" + strWithSpaces.stripTrailing() + "'");
  }
}
```

**실행 결과**
```
소문자로 변환:    java programming 
대문자로 변환:    JAVA PROGRAMMING 
공백 제거(trim): 'Java Programming'
공백 제거(strip): 'Java Programming'
앞 공백 제거(stripLeading): 'Java Programming '
뒤 공백 제거(stripTrailing): '   Java Programming'
```

## 문자열 분할 및 조합
- `split(String regex)` : 문자열을 정규 표현식을 기준으로 분할합니다.
- `join(CharSequence delimiter, CharSequence... elements)` : 주어진 구분자로 여러 문자열을 결합합니다.

```java
package lang.string.method;

public class StringSplitJoinMain {

  public static void main(String[] args) {
    String str = "Apple,Banana,Orange";

    // split()
    String[] splitStr = str.split(",");
    for (String s : splitStr) {
      System.out.println(s);
    }

    String joinStr = "";
    for (int i = 0; i < splitStr.length; i++) {
      String string = splitStr[i];
      joinStr += string;
      if (i != splitStr.length-1) {
        joinStr += "-";
      }
    }

    System.out.println("joinStr = " + joinStr);

    // join()
    String joinedStr = String.join("-", "A", "B", "C");
    System.out.println("연결된 문자열  = " + joinedStr);

    // 문자열 배열 연결
    String result = String.join("-", splitStr);
    System.out.println("result = " + result);
  }
}
```

**실행 결과**
```
Apple
Banana
Orange
joinStr = Apple-Banana-Orange
연결된 문자열  = A-B-C
result = Apple-Banana-Orange
```

## 기타 유틸리티.
- `valueOf(Object obj)` : 다양한 타입을 문자열로 변환합니다.
- `toCharArray()` : 문자열을 문자 배열로 변환합니다.
- `format(String format, Object... args)` : 형식 문자열과 인자를 사용하여 새로운 문자열을 생성합니다.
- `matches(String regex)` : 문자열이 주어진 정규 표현식과 일치하는지 확인합니다.

```java
package lang.string.method;

public class StringUtilsMain1 {

  public static void main(String[] args) {
    int num = 100;
    boolean bool = true;
    Object obj = new Object();
    String str = "Hello, Java!";

    // valueOf 메서드
    String numString = String.valueOf(num);
    System.out.println("숫자의 문자열 값: " + numString);
    String boolString = String.valueOf(bool);
    System.out.println("불리언의 문자열 값: " + boolString);
    String objString = String.valueOf(obj);
    System.out.println("객체의 문자열 값: " + objString);

    // 문자 + x -> 문자
    String numString2 = "" + num;
    System.out.println("빈 문자열 + num: " + numString2);

    // toCharArray 메서드
    char[] strCharArray = str.toCharArray();
    System.out.println("문자열을 문자 배열로 변환 : " + strCharArray);
    for (char c : strCharArray) {
      System.out.print(c);
    }
    System.out.println();
  }
}
```

**실행 결과**
```
숫자의 문자열 값: 100
불리언의 문자열 값: true
객체의 문자열 값: java.lang.Object@a09ee92
빈 문자열 + num: 100
문자열을 문자 배열로 변환 : [C@30f39991
Hello, Java!
```

```java
package lang.string.method;

public class StringUtilsMain2 {

  public static void main(String[] args) {
    int num = 100;
    boolean bool = true;
    String str = "Hello, Java!";

    // format 메서드
    String format1 = String.format("num: %d, bool: %b, str: %s", num, bool, str);
    System.out.println(format1);

    String format2 = String.format("숫자: %.2f ", 10.1234);
    System.out.println(format2);

    //printf
    System.out.printf("숫자: %.2f\n", 10.1234);

    // matches 메서드
    String regex = "Hello, (Java!|World)";
    System.out.println("'str'이 패턴과 일치하는가? " + str.matches(regex));
  }
}
```

**실행 결과**
```
num: 100, bool: true, str: Hello, Java!
숫자: 10.12 
숫자: 10.12
'str'이 패턴과 일치하는가? true
```

format 메서드에서 `%d`는 숫자 `%b`는 `boolean`, `%s`는 문자열을 뜻합니다.

