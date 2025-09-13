---
title: "☕️[Java] java에 대하여 간단하게 알아보기"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-10-02"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] java에 대하여 간단하게 알아보기.

## 1️⃣ 각 코드 행의 의미.

```java
int size = 27;
String name = "Fido";
Dog myDog = new Dog(name, size);
int x = size - 5;
if (x < 15) myDog.bark(8);

while (x > 3) {
    myDog.play();
}

int[] numList = {2, 4, 6, 8};
System.out.println("Hello");
System.out.pring("Dog: " + name);
String num = "8";
int z = Integer.parseInt(num);

try {
    readTheFile("myFile.txt");
}
catch (FileNoFoundException ex) {
    System.out.print("File not found.")
}
```

- 'size'라는 정수 변수를 선언하고 27이라는 값을 대입합니다.
- 'name'라는 문자열 변수를 선언하고 Fido라는 값을 대입합니다.
- 'myDog'라는 Dog 변수를 선언하고 'name'과 'size'를 사용해 새 Dog 객체를 만듭니다.
- 27('size'의 값)에서 5를 뺀 값을 'x'라는 변수에 대입합니다.
- 만약 x(값이 22)가 15보다 작으면 개가 여덟 번 짖도록 합니다.
- x가 3보다 크면 반복문을 사용합니다.
- 개가 놀도록 지시합니다(play() 메서드를 실행시킴)
- 반복문이 끝나는 부분입니다. `{}` 안에 있는 것들이 조건에 따라 반복됩니다.
- 'numList'라는 정수 배열을 선언하고 2,4,6,8을 집어넣습니다.
- "Hello"를 출력합니다. (아마 명령행으로 출력될것입니다.)
- 명령행에 "Dog: Fido"를 출력합니다.
- 'num'라는 문자열 변수를 선언하고 "8"이라는 값을 대입합니다.
- "8"이라는 문자열을 8이라는 숫자값으로 변환합니다.
- 무언가를 시도해 봅니다. 잘 안 될 수도 있습니다.
- "myFile.txe"를 읽습니다
- "시도하는 부분"이 끝이므로 여러 가지를 함께 시도할 수도 있습니다.
- 시도했던 것이 실패하면 실행됩니다.
- 실패했을 경우에 명령행에 "File not found"라고 출력합니다.
- 중괄호(`{}`) 안의 모든것은 'try' 부분이 실패한 경우에 수행할 작업입니다.

## 2️⃣ 자바 코드의 구조
<img src = "https://github.com/devKobe24/images2/blob/main/headfirst_java/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202024-10-02%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%206.53.17.png?raw=true">

- **소스 파일 안에는 "클래스"가 들어갑니다.**
- **클래스 안에는 "메서드"가 들어갑니다.**
- **메서드 안에는 "명령문"이 들어갑니다.**

### 소스 파일 안에는 무엇이 들어갈까요?
<img src = "https://github.com/devKobe24/images2/blob/main/headfirst_java/hfj-class.png?raw=true">

- 소스 코드 파일(.java라는 확장자가 붙은 파일)은 일반적으로 **클래스(class)** 를 한 개씩 정의합니다.
- 클래스는 보통 프로그램의 한 부분을 나타내지만, 아주 작은 애플리케이션 중에는 단 한 개의 클래스만으로 이뤄진 것도 있습니다.
- 클래스는 한 쌍의 중괄호({})로 둘러싸인 형태여야 합니다.

### 클래스 안에는 무엇이 들어갈까요?
<img src = "https://github.com/devKobe24/images2/blob/main/headfirst_java/hfj-method.png?raw=true">

- 하나의 클래스 안에는 하나 이상의 **메서드(method)** 가 들어갑니다.
- 예를 들어서,(개를 나타내는) Dog 클래스에는 (짖는 것을 의미하는) **bark** 라는 메서드가 들어갈 수 있으며, 이 메서드에는 개가 짖는 방법을 지시하는 내용이 들어갑니다.
- 메서드는 반드시 클래스 안에서 선언되어야 합니다.
- 즉, 클래스의 중괄호 안에 위치해야 합니다.

### 메서드 안에는 무엇이 들어갈까요?
<img src = "https://github.com/devKobe24/images2/blob/main/headfirst_java/hfj-statement.png?raw=true">

- 메서드를 감싸는 중괄호 안에는 메서드에서 처리할 일을 지시하는 내용이 들어갑니다.
- **메서드 코드**는 기본적으로 일련의 명령문을 모아 놓은 것이므로 지금은 메서드를 일종의 함수나 프로시저와 비슷한 것으로 생각하면 됩니다.

> 🙋‍♂️ [프로시저(Procedure)](https://www.devkobe24.com/CS/2024/2024-10-02-Procedure.html)
