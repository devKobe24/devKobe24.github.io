---
title: "☕️[Java] 변수와 자료형(3)"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-05-03"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 변수와 자료형(3).

## 1️⃣ 자료형에 대한 이해.

### 1. String.
**`String`** 클래스는 불변(immutable)의 문자열을 다룹니다.
- 이는 한 번 생성된 **`String`** 객체의 내용이 변경될 수 없다는 것을 의미합니다.
- 문자열을 변경하려고 할 때마다 실제로 새로운 **`String`** 객체가 생성되고, 기존 객체는 변경되지 않습니다.

### 1.1 String의 주요 메소드.
- **`charAt(int index)` :** 지정된 위치의 문자를 반환합니다.
- **`concat(String str)` :** 현재 문자열의 끝에 지정된 문자열을 붙여 새로운 문자열을 반환합니다.
- **`contains(CharSequence s)` :** 특정 문자열이 포함되어 있는지 확인합니다.
- **`startsWith(String prefix)` :** 문자열이 특정 문자열로 시작하는지 확인합니다.
- **`endsWith(String suffix)` :** 문자열이 특정 문자열로 끝나는지 확인합니다.
- **`equals(Object anObject)` :** 문자열이 주어진 객체와 동일한지 비교합니다.
- **`indexOf(int ch)`, `indexOf(String str)` :** 주어진 문자 또는 문자열의 위치를 찾습니다.
- **`length()` :** 문자열의 길이를 반환합니다.
- **`replace(char oldChar, char newChar)` :** 문자열 중 일부 문자를 다른 문자로 대체합니다.
- **`substring(int beginIndex, int endIndex)` :** 문자열의 부분을 추출합니다.
- **`toLowerCase()`, `toUpperCase()` :** 문자열을 소문자 또는 대문자로 변환합니다.
- **`trim()` :** 문자열의 앞뒤 공백을 제거합니다.

### 2. StringBuffer.
**`StringBuffer`** 클래스는 가변(mutable)의 문자열을 다루며, 문자열 변경 작업이 빈번할 때 사용하면 효율적입니다.
- **`StringBuffer`** 객체는 내용을 직접 변경할 수 있어, 새로운 객체를 계속 생성하지 않아도 됩니다.

### 2.1 StringBuffer의 주요 메소드.
- **`append(String str)`**: 문자열의 끝에 주어진 문자열을 추가합니다.
- **`delete(int start, int end)`**: 문자열의 시작 인덱스부터 종료 인덱스 전까지의 부분을 삭제합니다.
- **`deleteCharAt(int index)`**: 지정된 위치의 문자를 삭제합니다.
- **`insert(int offset, String str)`**: 지정된 위치에 문자열을 삽입합니다.
- **`replace(int start, int end, String str)`**: 시작 인덱스부터 종료 인덱스 전까지의 문자열을 새로운 문자열로 대체합니다.
- **`reverse()`**: 문자열의 순서를 뒤집습니다.
- **`length()`**: 문자열의 길이를 반환합니다.
- **`capacity()`**: 현재 버퍼의 크기를 반환합니다.
- **`setCharAt(int index, char ch)`**: 지정된 위치의 문자를 다른 문자로 설정합니다.

## 📝 정리.
**`StringBuffer`** 는 스레드에 안전(thread-safe)합니다, 즉 멀티스레드 환경에서 동시에 접근해도 안전하게 사용할 수 있습니다.
- 이는 내부적으로 메소드들이 동기화되어 있기 때문입니다.

반면, **`StringBuilder`** 는 **`StringBuffer`** 와 유사하지만 멀티스레드 환경에서의 동기화 자원이 없어 단일 스레드에서 더 빠르게 작동합니다.

이처럼 **`String`** 과 **`StringBuffer`** 는 각각의 특성에 맞게 선택하여 사용할 수 있으며, 성능과 사용상황에 따라 적절히 활용하면 됩니다.

---

### 3. Array.
자바 프로그래밍에서 배열(Array)은 동일한 타입의 여러 데이터를 연속적인 메모리 위치에 저장하기 위한 자료구조입니다.
배열은 고정된 크기를 가지며, 배열의 각 요소는 같은 데이터 타입을 가집니다.
배열을 사용하면 여러 데이터를 하나의 변수 이름으로 관리할 수 있어 코드를 간결하게 작성할 수 있습니다.

### 3.1 배열의 특징.
- **고정된 크기 :** 배열은 생성 시 지정된 크기를 변경할 수 없습니다.
    - 배열의 크기는 프로그램 실행 도중에 변경할 수 없으며, 더 많은 데이터를 저장해야 할 경우 새로운 배열을 생성하고 데이터를 복사해야 합니다.

- **인덱스 접근 :** 배열의 각 요소는 인덱스를 통해 접근할 수 있습니다.
    - 인덱스는 0부터 시작하여 배열의 크기 -1까지 번호가 할당됩니다.

- **동일 타입 :** 모든 배열 요소는 동일한 데이터 타입을 가져야 합니다.
    - 예를 들어, **`int`** 타입의 배열은 **`int`** 타입의 값만을 요소로 가질 수 있습니다.

### 3.2 배열의 선언과 초기화.
배열을 선언하고 사용하기 위해서는 다음 단계를 따라야 합니다.

- **1. 배열 선언 :** 데이터 타입 뒤에 대괄호 **`[]`** 를 사용하여 배열을 선언합니다.

```java
int[] myArray;
String[] stringArray;
```

- **2. 배열 생성 :** **`new`** 키워드를 사용하여 배열을 생성하고, 배열의 크기를 지정합니다.

```java
myArray = new int[10]; // 10개의 정수를 저장할 수 있는 배열
stringArray = new String[5]; // 5개의 문자열을 저장할 수 있는 배열
```

- **3. 배열 초기화 :** 배열의 각 요소에 값을 할당합니다.
    - 인덱스를 사용하여 접근할 수 있습니다.

```java
myArray[0] = 50;
myArray[1] = 100;
stringArray[0] = "Hello";
stringArray[1] = "World";
```

### 3.3 배열 사용 예.
다음은 자바에서 **`int`** 배열을 선언, 생성, 초기화하는 예제 코드입니다.

```java
public class ArrayExample {
    public static void main(Stringp[] args) {
        // 배열 선언과 동시에 생성
        int[] numbers = new int[3];
        
        // 배열 초기화
        numbers[0] = 7;
        numbers[1] = 20;
        numbers[2] = 33;
        
        // 배열 사용
        System.out.println("첫 번째 숫자: " + numbers[0]);
        System.out.println("두 번째 숫자: " + numbers[1]);
        System.out.println("세 번째 숫자: " + numbers[2]);
    }
}
```
## 📝 정리
이 예제에서는 **`numbers`** 라는 이름의 **`int`** 배열을 생성하고, 세 개의 정수를 저장한 후 출력합니다.
배열을 사용하는 이점 중 하나는 이처럼 단일한 이름으로 여러 데이터를 효율적으로 관리할 수 있다는 것입니다.
