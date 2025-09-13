---
title: "☕️[Java] String 클래스 - 불변 객체"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-09-05"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] String 클래스 - 불변 객체.
- `String`은 불변 객체입니다.
    - 따라서 생성 이후에 절대로 내분의 문자열 값을 변경할 수 없습니다.

다음 예시를 봅시다.
```java
public class StringImmutable1 {

    public static void main(String[] args) {
        String str = "hello";
        str.concat(" java");
        System.out.println("str = " + str);
    }
}
```
- `String.concat() 메서드를 사용하면 기존 문자열에 새로운 문자열을 연결해서 합칠 수 있습니다.
    - 이 경우 어떤 실행 결과가 나올까요?

**실행 결과**
```bash
str = hello
```
- 실행 결과를 보면 문자가 전혀 합쳐지지 않았습니다.

다음 코드를 봐봅시다.
```java
public class StringImmutable2 {

    public static void main(String[] args) {
        String str1 = "hello";
        String str2 = str1.concat(" java");
        System.out.println("str1 = " + str1);
        System.out.println("str2 = " + str2);
    }
}
```
- `String`은 불변 객체입니다.
    - 따라서 변경이 필요한 경우 기존 값을 변경하지 않고, 대신에 새로운 결과를 만들어서 반환합니다.

**실행 결과**
```bash
str1 = hello
str2 = hello java
```

<img src = "https://github.com/devKobe24/images2/blob/main/Inflearn-Java-Mid/java-string-immutable.png?raw=true">

- `String.concat()`은 내부에서 새로운 `String` 객체를 만들어서 반환합니다.
    - 따라서 불변과 기존 객체의 값을 유지합니다.

## 1️⃣ String이 불변으로 설계된 이유,
- `String` 이 불변으로 설계된 이유는 앞서 [불변 객체](https://www.devkobe24.com/Backend/Java/2024-09-01-ImmutableObject-Value-Change.html)에서 배운 내용에 추가로 다음과 같은 이유도 있습니다.
- 문자열 풀에 있는 `String` 인스턴스의 값이 중간에 변경되면 같은 문자열을 참고하는 다른 변수의 값도 함께 변경됩니다.

<img src = "https://github.com/devKobe24/images2/blob/main/Inflearn-Java-Mid/java-string-pool.png?raw=true">

- `String`은 자바 내부에서 문자열 풀을 통해 최적화를 합니다.
- 만약 `String` 내부의 값을 변경할 수 있다면, 기존에 문자열 풀에서 같은 문자를 참조하는 변수의 모든 문자가 함께 변경되어 버리는 문제가 발생합니다.
    - 다음의 경우 `str3`이 참조하는 문자를 변경하면 `str4`의 문자도 함께 변경되는 사이드 이펙트 문제가 발생합니다.
        - `String str3 = "hello"`
        - `String str4 = "hello"`

`String` 클래스는 불변으로 설계되어서 이런 사이드 이펙트 문제가 발생하지 않습니다.
