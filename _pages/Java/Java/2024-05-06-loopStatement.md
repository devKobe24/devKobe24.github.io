---
title: "☕️[Java] 반복문"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-05-06"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 1️⃣ 반복문

## 1. for 반복문.
자바 프로그래밍에서 **'for'** 반복문은 특정 조건을 만족하는 동안 코드 블록을 반복해서 실행하도록 설계된 제어 구조입니다.
**'for'** 문은 초기화, 조건 검사, 반복 후 실행할 작업(일반적으로 증감)을 한 줄에 명시하여 코드의 가독성과 관리를 용이하게 합니다.
이는 반복 실행이 필요한 많은 상황에서 유용하게 사용됩니다.

## 1.2 for 반복문의 기본 구조.
**'for'** 문의 기본 구조는 다음과 같습니다.

```java
for (초기화; 조건; 증감) {
    // 반복해서 실행할 코드
}
```
- **초기화 :** 반복문이 시작할 때 한 번 실행되는 부분으로, 반복문의 제어 변수를 초기 설정합니다.
- **조건 :** 이 조건이 참(**'true'**) 인 동안 반복문 내의 코드가 실행 됩니다. 조건이 거짓(**'false'**)이 되면 반복문은 종료됩니다.
- **증감 :** 각 반복의 끝에서 실행되며, 주로 제어 변수의 값을 증가시키거나 감소시키는데 사용됩니다.

## 1.3 for 반복문의 예시
**기본 예시**

```java
for (int i = 0; i < 5; i++) {
    System.out.println("i의 값은: " + i);
}
```

- 이 예제에서는 **'i'** 를 0부터 시작하여 **'i'** 가 5미만일 동안 반복하여, 매 반복마다 **'i'** 를 1씩 증가시킵니다.
    - 따라서 **"i의 값은: 0" 부터 "i의 값은: 4"** 까지 총 다섯 번의 출력을 하게 됩니다.

**확장된 예시: 다중 제어 변수**
```java
for (int i = 0, j = 10; i < j; i++, j--) {
    System.out.println("i = " + i + ". j = " + j);
}
```

- 이 예제에서는 두 개의 제어 변수 **'i'** 와 **'j'** 를 사용합니다.
    - **'i'** 는 증가하고 **'j'** 는 감소하며, **'i'** 가 **'j'** 와 같거나 **'j'** 보다 크게 되면 반복이 종료됩니다.
        - 이런 패턴은 복잡한 반복 조건이 필요한 경우 유용하게 사용됩니다.

## 1.4 사용 사례
**'for'** 반복문은 배열이나 컬렉션과 같은 데이터 구조를 순회할 때 매우 유용합니다.
예를 들어, 배열의 모든 요소를 처리하거나 조작할 때 자주 사용됩니다.
```java
int[] numbers = {1,2,3,4,5};
for (int i = 0; i < numbers.length; i++) {
    System.out.println("배열 요소: " + numbers[i]);
}
```

- 여기서 **'numbers.length'** 는 배열의 길이를 반환하며, **'i'** 는 0에서 시작하여 배열의 크기 미만이 될 때까지 증가하면서 배열의 각 요소에 접근합니다.

## 📝 정리
**'for'** 반복문은 코드를 간결하게 하면서 반복적인 작업을 효과적으로 처리할 수 있도록 도와줍니다.

---

## 2. while 반복문.
자바 프로그래밍에서 **'while'** 반복문은 특정 조건이 참(**'true'**)인 동간 주어진 코드 블록을 반복적으로 실행하는 구조 입니다.
**'while'** 문은 주로 반복 횟수가 불확실할 때 또는 반복 횟수를 사전에 정확히 알 수 없을 때 사용됩니다.

## 2.1 while 반복문의 기본 구조.
**'while'** 문의 기본 구조는 다음과 같습니다.

```java
while (조건) {
    // 조건이 참인 동안 반복 실행될 코드
}
```
- 여기서 **'조건'** 은 각 반복 이전에 평가되며, 이 조건이 참(**'true'**)일 때 반복 블록 내의 코드가 실행됩니다. 조건이 거짓(**'false'**)이 되면 반복문은 종료됩니다.

## 2.2 while 반복문 예시.
**간단한 예**
```java
int i = 0;
while (i < 5) {
    System.out.println("i의 값은: " + i);
    i++; // i 값을 증가시켜 조건이 eventually 거짓이 되도록 함
}
```

- 이 예제에서는 **'i'** 가 0부터 시작하여 5미만인 동안 반복문을 실행합니다.
    - 반복문 내에서 **'i'** 를 1씩 증가시켜 eventually 조건이 거짓이 되도록 합니다.
        - 결과적으로 **'i'** 의 값은 0부터 4까지 콘솔에 출력됩니다.

**사용자 입력 받기**
**'while'** 문은 사용자 입력을 받고, 그 입력에 따라 반복을 계속할지 여부를 결정할 때 유용하게 사용될 수 있습니다.
예를 들어, 사용자가 특정 문자를 입력할 때까지 입력을 계속 받는 프로그램은 다음과 같이 작성할 수 있습니다.
```java
Scanner scanner = new Scanner(System.in);
String input = "";
while (!input.equals("종료")) {
    System.out.println("문자열을 입력하세요. 종료하려면 '종료'를 입력하세요: ");
    input = scanner.nextLine();
}
scanner.close();
```

## 2.3 주의사항.
**'while'** 반복문을 사용할 때는 반복문 내에서 조건이 eventually(결국) 거짓이 될 수 있도록 조치를 취해야 합니다.
그렇지 않으면, 조건이 항상 참으로 평가될 경우 무한 후프에 빠질 수 있습니다.
따라서 조건 변수를 적절히 조작하거나 적절한 로직을 구현하여 반복문이 적절한 시점에 종료될 수 있도록 해야 합니다.

## 📝 정리.
**'while'** 문은 그 구조가 간단하고 유연하여, 특정 조건 하에 반복 실행을 해야 할 때 매우 유용한 프로그래밍 도구입니다.

---

## 3. do-while 반복문.
자바 프로그래밍에서 **'do-while'** 반복문은 조건을 검사하기 전에 최소 한 번은 코드 블록을 실행하는 반복문입니다.
이 구조는 **'while'** 반복문과 비슷하지만, 조건의 참/거짓 여부에 관계없이 최소한 처음에는 반복문 내의 코드를 실행한다는 점이 다릅니다.
**'do-while'** 문은 주로 사용자 입력을 처리하거나, 조건이 반복문의 실행 후에 결정되어야 할 때 유용합니다.

## 3.1 do-while 반복문의 기본 구조.
**'do-while'** 문의 기본 구조는 다음과 같습니다.
```java
do {
    // 최소 한 번은 실행될 코드
} while (조건);
```

- 여기서 **'조건'** 은 반복문의 끝에서 평가됩니다.
    - 조건이 참(**'true'**)이면, 코드 블록이 반복적으로 실행됩니다. 조건이 거짓(**'false'**)이면, 반복이 종료됩니다.

## 3.2 do-while 반복문의 예시.
**기본 예**
```java
int i = 0;
do {
    System.out.println("i의 값은: " + i);
    i++;
} while (i < 5);
```

- 이 예제에서는 **'i'** 가 0부터 시작하여 **'i < 5'** 인 동안 반복합니다.
    - **'do-while'** 문은 **'i'** 의 초기 값에 상관없이 최소 한 번은 "i의 값은: 0"을 출력하고 시작합니다. 그 후 **'i'** 가 5미만인 동안 계속해서 반복됩니다.

**사용자 입력 받기**
사용자로부터 입력을 받고, 특정 조겅("종료" 문자열 입력)을 만족할 때까지 계속 입력을 받는 프로그램을 구현할 때 **'do-while'** 문이 유용하게 사용됩니다.

```java
Scanner scanner = new Scanner(System.in);
String input;
do {
    System.out.println("문자열을 입력하세요. 종료하려면 '종료'를 입력하세요:");
    input = scanner.nextLine();
} while (!input.equals("종료"));
scanner.close();
```
- 이 코드는 사용자가 "종료"를 입력할 때까지 계속해서 입력을 받습니다.
    - 입력을 받는 동작은 최소 한 번은 실행되며, 이는 **'do-while'** 문이 최조 실행을 보장하기 때문입니다.

## 3.3 주의사항.
**'do-while'** 반복문을 사용할 때, 조건을 적절히 설정하여 반복문이 적절한 시점에 종료되도록 해야 합니다. 그렇지 않으면 무한 루프에 빠질 수 있습니다.
또한, 조건 검사가 반복문의 끝에서 이루어지므로, 조건이 매우 빨리 거짓이 되어도 코드 블록이 한 번은 실행됨을 기억해야 합니다.

## 📝 정리.
**'do-while'** 문은 조건이 반복 블록 실행 후에만 알 수 있거나, 반복 블록을 적어도 한 번은 실행해야 하는 경우 특히 유용한 도구입니다.

---

## 4. continue.
자바 프로그래밍에서 **'continue'** 문은 반복문 내에서 사용되며, 그것이 실행될 때 현재 반복의 나머지 부분을 건너뛰고 즉시 다음 반복으로 넘어가도록 합니다.
이를 통해 특정 조건에서 반복문의 다음 순환을 즉시 시작할 수 있게 해줍니다.
**'continue'** 는 주로 반복문 내에서 특정 조건에 대한 예외 처리나 불필요한 처리를 건너뛰기 위해 사용됩니다.

## 4.1 continue 기본 사용법
**'continue'** 문은 주로 **'for'**, **'while'**, 또는 **'do-while'** 반복문 내에서 사용됩니다.
간단하게는 **'continue'** 를 실행하면 반복문의 현재 순회에서 남은 코드를 실행하기 않고, 다음 반복으로 진행합니다.

## 4.2 continue 예시
**'for' 반복문에서의 사용**
```java
for (int i = 0; i < 10; i++) {
    if (i % 2 == 0) {
        continue; // 짝수인 경우, 출력을 건너뛰고 다음 반복으로 넘어감
    }
    System.out.println(i); // 홀수만 출력
}
```

- 이 코드는 0부터 9까지의 숫자 중에서 홀수만 출력합니다.
    - **'i'** 가 짝수일 경우, **'continue'** 문이 실행되어 **'System.out.println(i);'** 줄을 건너뛰고 다음 반복으로 넘어갑니다.

**'while' 반복문에서의 사용**
```java
int i = 0;
while (i < 10) {
    i++;
    if (i % 2 == 0) {
        continue; // 짝수인 경우, 아래의 출력을 건너뛰고 다음 반복으로 넘어감
    }
    System.out.println(i); // 홀수만 출력
}
```

- 이 예제에서도 **'continue'** 는 짝수를 확인하는 조건에서 참일 경우 나머지 코드를 건너뛰고 다음 반복을 계속 진행하도록 합니다.

## 4.3 continue 특징 및 주의사항.
- **'continue'** 문은 현재 수행 중인 반복의 나머지 부분을 건너뛰고, 반복문의 조건 검사로 직접 이동하여 다음 반복을 시작합니다.
- **'continue'** 문을 사용할 때는 반복문이 무한 루프에 빠지지 않도록 주의해야 합니다.
    - 예를 들어, **'continue'** 문이 반복문의 변수 값을 변경하는 코드를 건너뛰면 그 변수의 값이 업데이트되지 않아 무한 루프가 발생할 수 있습니다.
- **'continue'** 는 루프의 흐름을 제어하고, 코드의 읽기 어려움을 증가시킬 수 있으므로, 사용할 때는 명확한 이유가 있어야 합니다.

## 📝 정리.
**'continue'** 문은 코드를 보다 효율적으로 만들고, 필요 없는 조건을 빠르게 건너뛸 수 있게 도와줍니다.
그러나 코드의 가독성과 유지 관리에 영향을 줄 수 있으므로 신중하게 사용해야 합니다.

---

## 5. break문.
자바 프로그래밍에서 **'break'** 문은 반복문(**'for', 'while', 'do-while'**) 또는 **'switch'** 문에서 현재 블록의 실행을 즉시 종료하고, 해당 블록의 바깥으로 제어를 이동시키는 역할을 합니다.

이는 반복문 또는 **'switch'** 문 내에서 특정 조건을 만족할 때 추가적인 처리 없이 루프나 선택 구조를 벗어나기 위해 사용됩니다.

## 5.1 break 기본 사용법.
- **반복문에서의 사용 :** **'break'** 를 사용하여 무한 루프를 종료하거나 특정 조건이 만족될 때 반복문을 조기에 종료할 수 있습니다.
- **'switch' 문에서의 사용 :** 각 **'case'** 블록 뒤에 **'break'** 를 사용하여 **'switch'** 문을 종료하고, 다음 **'case'** 로 넘어가지 않도록 합니다.

## 5.2 break 예시
**반복문에서의 'break'**
```java
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        break; // i가 5가 되면 for 루프를 종료
    }
    System.out.println(i);
}
```
- 이 코드에서는 **'i'** 가 5에 도달하면 **'break'** 문이 실행되어 **'for'** 루프가 즉시 종료됩니다.
    - 결과적으로, 0부터 4까지의 숫자만 출력됩니다.

**'while' 반복문에서의 'break'**
```java
int i = 0;
while (true) { // 무한 루프
    if (i == 5) {
        break; // i가 5가 되면 while 루프를 종료
    }
    System.out.println(i);
    i++;
}
```
- 이 예제에서도 **'i'** 가 5일 때 **'break'** 문을 사용하여 무한 루프를 종료합니다.

**'switch' 문에서의 'break'**
```java
int number = 2;
switch (number) {
    case 1:
        System.out.println("Number is 1");
        break;
    case 2:
        System.out.println("Number is 2");
        break;
    case 3:
        System.out.println("Number is 3");
        break;
    default:
        System.out.println("Number is not 1, 2, or 3");
}
```

- **'switch'** 문에서 **'number'** 가 2일 때 해당하는 **'case'** 블록이 실행되고, **'break'** 문으로 인해 **'switch'** 문을 벗어나게 됩니다.

## 5.3 break문 특징 및 주의사항
- **'break'** 문은 코드 실행을 즉시 중단시키므로, 효과적인 프로그램 흐름 제어를 가능하게 합니다.
- 반복문이나 **'switch'** 문 내에서만 **'break'** 문을 사용할 수 있습니다.
- **'break'** 문을 사용할 때는 코드의 흐름을 명확히 이해하고 있어야 하며, 무분별한 사용은 코드의 가독성과 유지보수를 어렵게 만들 수 있습니다.

## 📝 정리.
**'break'** 는 코드의 복잡성을 줄이고 특정 조건에서 즉시 반복문을 종료할 수 있는 강력한 도구입니다.
그러나 그 사용은 코드의 구조를 명확하게 유지하는 방식으로 신중하게 이루어져야 합니다.

---

## 6. for-each문.
자바 프로그래밍에서 **'for-each'** 문, 또는 강화된 **'for'** 문(enhanced for loop)은 배열이나 컬렉션 프레임워크에 저장된 각 요소를 순회하기 위해 사용되는 구문입니다.

기존의 **'for'** 문보다 간결하며, 코드를 읽고 작성하기가 더 쉬워 배열이나 컬렉션의 모든 요소에 접근할 때 일반적으로 권장되는 방식입니다.

## 6.1 for-each문 기본 구조.
**'for-each'** 문의 기본 구조는 다음과 같습니다.
```java
for (타입 변수명 : 배열 또는 컬렉션) {
    // 변수명을 사용한 코드
}
```

- 여기서 **'타입'** 은 배열 또는 컬렉션에 저장된 요소의 타입을 말하고, **'변수명'** 은 반복되는 각 요소를 참조하는 데 사용되는 변수 이름입니다.
- **'배열 또는 컬렉션'** 은 순회할 배열이나 컬렉션 객체를 지정합니다.

## 6.2 for-each문 예시
**배열 사용 예**
```java
int[] numbers = {1,2,3,4,5};
for(int number: numbers) {
    System.out.println(number);
}
```
- 이 코드에서 **'for-each'** 문은 **'numbers'** 배열의 모든 요소를 순회합니다.
    - 각 반복에서 **'number'** 변수에 배열의 요소가 할당되며, 그 값을 출력합니다.

**컬렉션 사용 예**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
for (String name : names) {
    System.out.println(name);
}
```
- 이 예에서는 **'names'** 리스트의 모든 요소를 순회합니다.
    - 각 요소는 **'name'** 변수에 할당되고, **'System.out.println'** 을 통해 출력됩니다.

## 6.3 for-each문의 장점.
- **가독성 향상:** **'for-each'** 문은 간결하고 이해하기 쉬워, 코드의 가독성을 크게 향상시킵니다.
- **오류 감소:** 전통적인 **'for'** 문에서 발생할 수 있는 인덱스 관련 실수나 경계 조건 오류를 방지할 수 있습니다.
- **향상된 추상화:** 컬렉션의 내부 구조나 크기를 몰라도 각 요소에 접근할 수 있습니다.

## 6.4 for-each문의 제한사항.
- **컬렉션 수정 불가:** **'for-each'** 문을 사용하는 동안 컬렉션을 수정할 수 없습니다.
    - 예를 들어, 순회 중인 컬렉션에서 요소를 추가하거나 제거할 수 없습니다.
- **인덱스 접근 불가:** **'for-each'** 문은 각 요소에 대한 인덱스를 제공하지 않습니다.
    - 특정 인덱스의 요소에 접근하거나 인덱스를 활용한 복잡한 로직이 필요한 경우에는 전통적인 **'for'** 문을 사용해야 합니다.

## 📝 정리.
**'for-each'** 문은 자바에서 컬렉션과 배열을 효율적으로 처리할 수 있는 강력하고 사용하기 쉬운 도구입니다.
이를 통해 코드를 더욱 간결하고 안전하게 만들 수 있습니다.

---