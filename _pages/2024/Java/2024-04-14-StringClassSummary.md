---
title: "☕️[Java] String 클래스 - 정리"
tags:
    - Java
    - Programming Language
date: "2024-04-14"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# String 클래스 - 정리

## 1. 자바에서 문자를 다루는 대표적인 타입
 - char
 - String
 - char 배열을 사용하면 문자열을 다룰 수 있으나 불편합니다.
     - 그래서 String이라는 것을 Java에서 제공해줍니다.

## 2. String 클래스를 통해 문자열을 생성하는 2가지 방법.
```java
String str1 = "hello"; // 방법 1 -> 문자열 리터럴
String str2 = new String("hello"); // 방법 2
```
- 쌍따옴표 사용: 방법 1
- 객체 생성 : 방법 2

문자열 리터럴을 사용하더라도 결국 `new String("hello");` 가 되는 것 입니다.
- 편의상 쌍따옴표로 문자열을 감싸면(문자열 리터럴) 자바 언어에서 `new String("hello");` 와 같이 변경해 줍니다.
    - 이 경우 실제로는 성정 최적화를 위해 문자열 풀을 사용합니다.

> 문자열은 참조형입니다.

## 3. String 클래스 내부
String 클래스는 대략 다음과 같이 생겼습니다.
```java
public final class String {
    // 문자열 보관
    private final char[] value; // 자바 9 이전
    private final byte[] value; // 자바 9 이후
    
    // 여러 메서드
    public String concat(String str) {...}
    public int length() {...}
}
```

## 3. String 클래스 비교.
String 클래스를 비교할 때는 `==` 비교가 아니라 항상 `equals()` 비교를 해야합니다.
- **동일성(Identity) :** `==` 연산자를 사용해서 두 객체의 **참조(Reference)** 가 동일한 객체를 가리키고 있는지 확인합니다.
- **동등성(Equality) :** `equals()` 메서드를 사용하여 두 객체가 **논리적으로 같은지** 확인합니다.

## 4. 문자열 리터럴, 문자열 풀.
<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%A7%E1%86%AF%E1%84%85%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%A5%E1%86%AF%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%A7%E1%86%AF%E1%84%90%E1%85%B5%E1%86%B8.png?raw=true">

- `String str3 = "hello"` 와 같이 문자열 리터럴을 사용하는 경우 자바는 메모리 효율성과 성능 최적화를 위해 문자열 풀을 사용합니다.
- 자바가 실행되는 시점에 클래스에 문자열 리터럴이 있으면 문자열 풀에 `String` 인스턴스를 미리 만들어 둡니다.
    - 이때 같은 문자열이 있으면 만들지 않습니다.
- `String str3 - "hello"`와 같이 문자열 리터럴을 사용하면 문자열 풀에서 `"hello"`라는 문자를 가진 `String` 인스턴스를 찾습니다.
    - 그리고 찾은 인스턴스의 참조(`x003`)를 반환합니다.
- `String str4 = "hello"`의 경우 `"hello"` 문자열 리터럴을 사용하므로 문자열 풀에서 `str3`과 같은 `x003` 참조를 사용합니다.
- 문자열 풀 덕분에 같은 문자를 사용하는 경우 메모리 사용을 줄이고 문자를 만드는 시간도 줄어들기 때문에 성능도 최적화 할 수 있습니다.

> 따라서 문자열 리터럴을 사용하는 경우 같은 참조값을 가지므로 `==` 비교에 성공합니다.

> **참고**
> 풀(Pool)은 자원이 모여있는 곳을 의미합니다.
> 프로그래밍에서 풀(Pool)은 공용 자원을 모아둔 곳을 뜻합니다.
> 여러 곳에서 함께 사용할 수 있는 객체를 필요할 때 마다 생성하고, 제거하는 것은 비효율적입니다.
> 대신에 이렇게 문자열 풀에 필요한 `String` 인스턴스를 미리 만들어두고 여러곳에서 재사용할 수 있다면 성능과 메모리를 더 최적화할 수 있습니다.
> 참고로 문자열 풀은 힙 영역을 사용합니다.
> 그리고 문자열 풀에서 문자를 찾을 때는 해시 알고리즘을 사용하기 때문에 매우 빠른 속도로 원하는 `String` 인스턴스를 찾을 수 있습니다.

## 5. String 클래스는 불변객체.
`String`은 불변 객체입니다.
따라서 생성 이후에 절대로 내부의 문자열 값을 변경할 수 없습니다.
```java
public static void main(String[] args) {
    String str1 = "hello";
    String str2 = str1.concat(" java");
    System.out.println("str1 = " + str1);
    System.out.println("str1 = " + str2);
}
```
- `String`은 불변 객체입니다. 따라서 변경이 필요한 경우 기존 값을 변경하지 않고, 대신에 새로운 결과를 만들어서 반환합니다.

**실행 결과**
```
str1 = hello
str2 = hello java
```

<img src = "https://github.com/devKobe24/images/blob/main/String%E1%84%8F%E1%85%AE%E1%86%AF%E1%84%85%E1%85%A2%E1%84%89%E1%85%B3%E1%84%87%E1%85%AE%E1%86%AF%E1%84%87%E1%85%A7%E1%86%AB%E1%84%80%E1%85%A2%E1%86%A8%E1%84%8E%E1%85%A6-%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B71.png?raw=true">
- `String.concat()`은 내부에서 새로운 `String` 객체를 만들어서 반환합니다.
    - 따라서 불변과 기존 객체의 값을 유지합니다.

문자열에서 무언가 변경하는 것이 있으면 그 내부에서 인스턴스를 생성하여 반환합니다.
- 때문에 반환값을 받아서 사용해야 합니다.

## 6. String 클래스가 불변으로 설계된 이유.
1. 사이드 이팩트 문제.
2. 문자열 풀을 사용시에 불변으로 설계되어 있어야 안전하게 사용할 수 있기 때문입니다.

## 7. String 클래스 주요 메서드.
[주요 메서드 블로그 글 1](https://www.devkobe24.com/2024/Java/2024-04-09-StringClassMethod-1.html)
[주요 메서드 블로그 글 2](https://www.devkobe24.com/2024/Java/2024-04-10-StringClassMethod2.html)

## 참고. CharSequence
**CharSequence** 는 **String**, **StringBuilder**의 상위 타입입니다.
문자열을 처리하는 다양한 객체를 받을 수 있습니다.

## 8. StringBuilder - 가변 String
**불변인 String 클래스의 단점:** 뭔가 값을 더하거나 변경하거나 할 때마다 계속 새로운 객체를 만들어내야 한다는 점 입니다. 때문에 성능도 느려질 수 있습니다. (물론, 불변이기 때문에 안전하기는 합니다.)
- 이러한 단점들 때문에 **StringBuilder** 가 있습니다.

**StringBuilder:** **가변 String** 입니다.
- 값을 쭉 바꾸고 쓰면 되는데, 마지막에는 다시 String으로 바꾸는 `toString()`을 사용합니다.
    - 즉, 다시 안전한 불변으로 바꾸는 작업입니다.
    - 변경이 필요할 때 가변으로, 마지막에는 불변으로 바꿔서 쓰는것을 권장합니다.
    - "뭔가 문자열을 변경할 일이 많다" -> **StringBuilder** 를 사용하면 됩니다.

## 9. String 최적화.
그러나 생각보다 **StringBuilder**를 사용할 때가 많이 없습니다.
- 그 이유는 자바가 **String을 최적화하기 때문입니다.**

컴파일러에서도 최적화를 하고, 변수로 되어있어도 컴파일러가 최적화를 수행합니다.
- 예를 들어 Java가 직접 StringBuilder를 사용합니다.
```java
String result = new StringBuilder().append(str1).append(str2).toString();
```

## 10. String 최적화가 어려운 경우.
다음과 같이 문자열을 루프안에서 문자열을 더하는 경우에는 최적화가 이루어지지 않습니다.
```java
public class LoopStringMain {
    public static void main(String[] args) {
        long startTime = System.currentTimeMillis();
        
        String result = "";
        for (int i = 0; i < 100000; i++) {
            result += "Hello Java ";
        }
        long endTime = System.currentTimeMillis();
        
        System.out.println("result = " + result);
        System.out.println("time = " + (endTime - startTime) + "ms");
    }
}
```

왜냐하면 대략 다음과 같이 최적화가 되기 때문입니다.(최적화 방식은 자바 버전에 따라 다릅니다.)
```java
String result = "";
for (int i = 0; i < 1000000; i++) {
    result = new StringBuilder().append(result).append("Hello Java ").toString();
}
```

반복문의 루프 내부에서는 최적화가 되는 것 처럼 보이지만, **반복 횟수만큼 객체를 생성해야 합니다.**
**반복문 내에서의 문자열 연결은, 런타임에 연결할 문자열의 개수와 내용이 결정됩니다.**
이런 경우, 컴파일러는 얼마나 많은 반복이 일어날지, 각 반복에서 문자열이 변할지 예측할 수 없습니다.
따라서, 이런 상황에서는 최적화가 어렵습니다.

**`StringBuilder`** 는 물론이고, 아마도 대략 반복 횟수인 100,000번의 **`String`** 객체를 생성했을 것입니다.

**이럴 때는 직접 `StringBuilder` 를 사용하면 됩니다.**
```java
public class LoopStringMain {
    public static void main(String[] args) {
        long startTime = System.currentTimeMillis();
        
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 100000; i++) {
            sb.append("Hello Java ");
        }
        String result = sb.toString();
        long endTime = System.currentTimeMillis();
        
        System.out.println("result = " + result);
        System.out.println("time = " + (endTime - startTime) + "ms");
    }
}
```

**정리**
- 문자열을 합칠 때 대부분의 경우 최적화가 되므로 `+` 연산을 사용하면 됩니다.

**StringBuilder를 직접 사용하는 것이 더 좋은 경우**
- 반복문에서 반복해서 문자를 연결할 때
- 조건문을 통해 동적으로 문자열을 조합할 때
- 복잡한 문자열의 특정 부분을 변경해야 할 때
- 매우 긴 대용량 문자열을 다룰 때

## 11. 메서드 체이닝.
자기 자신의 값을 반환해서 메서드를 쭉 연결해서 사용할 수 있습니다.

자바의 많은 라이브러리들이 메서드 체이닝 기법을 사용하고 있습니다.

[메서드 체이닝 블로그 글](https://www.devkobe24.com/2024/Java/2024-04-11-MethodChaining.html)
