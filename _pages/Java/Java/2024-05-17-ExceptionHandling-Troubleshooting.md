---
title: "☕️[Java] 예외 처리 - 트러블슈팅"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-05-17"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 1️⃣ 예외 처리.

실제 코드를 작성하면서 예외 처리에 대한 문제를 맞닥뜨려 어떻게 해야 할지 고민하다 다음과 같이 풀어보았습니다.

```java
  private static int enterTheAge(Scanner scanner) {
    while (true) {
      try {
        System.out.print("나이를 입력해 주세요.(숫자): ");
        int age = scanner.nextInt();
        return age;
      } catch (InputMismatchException e) {
        System.out.println("숫자를 입력해 주세요.");
        scanner.next();
      }
    }
  }
```

# 🤔 문제 상황.
## 1. 나이는 숫자만 입력되어야 한다.
- 숫자가 아닌 것들이 들어왔을 경우 예외 처리를 해줘서 다시 유저가 나이를 입력할 수 있게끔 하도록 만드는 과정에서 문제 상황이 발견되었습니다.

# 💻 트러블슈팅.
## 1. while 반복문의 사용.
- `while (true)` 를 사용하여 무한 루프를 돌려 내부에서 숫자가 아니면 다시금 유저가 **"유효하지 않은 입력입니다"** 라는 예외 처리 출력문을 받고 **"나이를 입력해 주세요"** 라는 원래의 출력문을 받을 수 있도록 했습니다.

## 2. try-catch 문의 사용.
- **try-catch** 문을 사용하여 **Scanner** 클래스 메소드를 사용할 때 발생하는 예외중, 사용자가 기대하는 타입의 입력을 제공하지 않았을 때 던져지는 **InputMismatchException** 를 **catch** 문에 넣어주어 이 예외가 발생시 잡아서 실행하는 블럭 내부에 예외 처리 코드를 아래와 같이 삽입 하였습니다.
```java
try {
    System.out.print("나이를 입력해 주세요.(숫자): ");
    int age = scanner.nextInt();
    return age;
} catch (InputMismatchException e) {
    System.out.println("숫자를 입력해 주세요.");
    scanner.next();
}
```

- **'scanner.next()'** 를 호출하여 잘못된 입력을 버리고, 다음 입력을 기다리도록 합니다.
- 사용자가 유효한 숫자를 입력하면 **'age'** 를 반환하고 메소드를 종료합니다.
