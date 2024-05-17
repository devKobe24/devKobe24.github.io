---
title: "☕️[Java] 문자열 비교 - 트러블슈팅"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-05-18"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 1️⃣ 문자열 비교.
```java
  private static String exceptionHandleForPersonOfNationalMerit(Scanner scanner) throws InvalidCheckException {
    System.out.print("국가유공자 여부를 입력해 주세요.(y/n) :");
    String personOfNationalMerit = scanner.next();

    // 들어온 값이 y 또는 n이 아닌 경우 예외를 던집니다.
    if (personOfNationalMerit != "y" && personOfNationalMerit != "n") {
      throw new InvalidCheckException("유효하지 않은 입력입니다. y 또는 n을 입력해주세요.");
    }
    return personOfNationalMerit;
  }
```

# 🤔 문제 상황.
## 1. 문자열 비교가 안되는 상황.
- **'=='** 연산자를 사용해서 **"y"** 또는 **"n"** 외의 다른 문자열이 들어올 경우 **커스텀한 에러** 를 발생시키려 했지만 정상적인 값인 **"y"** 와 **n** 을 넣어도 **에러가 발생** 하는 문제가 생겼습니다.

# 💻 트러블슈팅.
## 1. equals() 메소드의 사용.

자바에서는 문자열 비교를 할 때 **'=='** 연산자를 사용하는 것이 아니라 **'equals()'** 메소드를 사용해야 합니다.
- **'=='** 연산자는 객체의 레퍼런스를 비교하기 때문에, 두 문자열이 같은 객체를 참조하는지 여부를 확인합니다.
- **'equals()'** 메소드는 문자열의 내용을 비교합니다.

따라서, **"y"** 와 **"n"** 을 비교할 때 **'=='** 대신 **'equals()'** 를 사용해야 합니다.

```java
  private static String exceptionHandleForPersonOfNationalMerit(Scanner scanner) throws InvalidCheckException {
    System.out.print("국가유공자 여부를 입력해 주세요.(y/n) :");
    String personOfNationalMerit = scanner.next();

    // 들어온 값이 y 또는 n이 아닌 경우 예외를 던집니다.
    if (!personOfNationalMerit.equals("y") && !personOfNationalMerit.equals("n")) {
      throw new InvalidCheckException("유효하지 않은 입력입니다. y 또는 n을 입력해주세요.");
    }
    return personOfNationalMerit;
  }
```
