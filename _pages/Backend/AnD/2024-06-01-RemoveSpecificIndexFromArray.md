---
title: "📦[DS,Algorithm] 배열에서 특정 인덱스의 요소를 삭제하기."
tags:
    - DataStructure
    - Algorithm
date: "2024-06-01"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 1️⃣ 배열에서 특정 인덱스의 요소를 삭제하기.

Java에서 배열의 특정 인덱스의 요소를 삭제하는 방법은 배열의 구조 특성상 직접적으로 제공되지 않습니다.

때문에 일반적으로 요소를 삭제하기 위해 다음의 방법을 사용합니다.

# 2️⃣ 배열에서 요소를 삭제하는 방법 2가지.

## 1️⃣ 새로운 배열을 생성하여 요소를 복사하는 방법 :)

● 특정 인덱스의 요소를 건너뛰고 나머지 요소를 새로운 배열에 복사합니다.

**방법 1 : 새로운 배열 생성하여 복사.** 
```java
// 배열의 특정 인덱스의 요소를 삭제하는 방법 - 1
// 방법1. 새로운 배열을 생성하여 요소를 복사하는 방법
// - 특정 인덱스의 요소를 건너뛰고 나머지 요소를 새로운 배열에 복사합니다.
public class Main {

  public static void main(String[] args) {
    int[] array = {10, 20, 30, 40, 50};
    array = removeElement(array, 0);

    for (int value : array) {
      System.out.println(value + " ");
    }

  }

  // 특정 배열을 지우는 메소드
  public static int[] removeElement(int[] array, int index) {
    if (index < 0 || index >= array.length) {
      throw new IndexOutOfBoundsException("Index out of bounds");
    }
    
    // 새로운 배열은 특정 요소를 지우기 때문에 기존 배열의 크기에서 -1 한 크기로 생성합니다.
    int[] newArray = new int[array.length - 1];

    for (int i = 0, j = 0; i < array.length; i++) {

      if (i != index) {
        newArray[j++] = array[i];
      }
    }
    return newArray;
  }
}
/* 
=== 출력 ===
20 
30 
40 
50 
*/
```
- "방법1의 장.단점"
    - **새 배열 생성 :** 메모리 사용량이 증가하지만, 원래 배열을 유지하고 싶은 경우 유용합니다.

## 2️⃣ 기존 배열을 이용하여 요소를 덮어쓰는 방법 :)

● 특정 인덱스 이후의 요소들을 앞으로 한 칸씩 이동시켜 덮어씁니다.

**방법 2 : 기존 배열을 이용하여 요소 덮어쓰기.**
```java
// 배열의 특정 인덱스의 요소를 삭제하는 방법 - 2
// 방법2. 기존 배열을 이용하여 요소를 덮어쓰는 방법.
// - 특정 인덱스 이후의 요소들을 앞으로 한 칸씩 이동시켜 덮어 씁니다.
public class Main {

  public static void main(String[] args) {
    int[] array = {10, 20, 30, 40, 50};
    array = removeElementInPlace(array, 0);

    for (int value : array) {
      System.out.println(value + " ");
    }
  }

  public static int[] removeElementInPlace(int[] array, int index) {
    if (index < 0 || index >= array.length) {
      throw new IndexOutOfBoundsException("Index out of bounds");
    }

    for (int i = index; i < array.length - 1; i++) {
      array[i] = array[i + 1];
    }

    // 배열의 마지막 요소를 0 또는 다른 기본값으로 설정 (선택 사항)
    array[array.length - 1] = 0;

    return array;
  }
}
/*
=== 출력 ===
20 
30 
40 
50 
0 
*/ 
```
- "방법2의 장.단점"
    - **기존 배열 사용 :** 메모리를 절약할 수 있지만, 배열의 마지막 요소는 기본값으로 설정해야 합니다.
