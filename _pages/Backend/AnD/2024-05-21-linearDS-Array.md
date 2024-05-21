---
title: "📦[DS,Algorithm] 선형 자료구조 - 배열"
tags:
    - DataStructure
    - Algorithm
date: "2024-05-21"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 1️⃣ 선형 자료구조 - 배열.

자료구조 관점에서 배열을 이해하고 여러 방법으로 구현 가능

## 1️⃣ 배열(Array).

자료구조 관점에서 배열(Array)은 동일한 타입의 데이터를 연속된 메모리 공간에 저장하는 선형 자료구조입니다.

배열은 조정된 크기를 가지며, 인덱스를 사용하여 각 요소에 빠르게 접근할 수 있는 특징이 있습니다.

배열은 가장 기본적이고 널리 사용되는 자료구조 중 하나입니다.

### 특징.
1. **고정된 크기(Fixed Size)**
    - 배열은 선언 시 크기가 결정되며, 배열의 크기는 변경할 수 없습니다. 이 크기는 배열을 사용하는 동안 고정되어 있습니다.
    - 예: **'`int[] numbers = new int[10];`'**(크기가 10인 정수형 배열)

2. **연속된 메모리 공간(Contiguous Memory Allocation)**
    - 배열의 요소들은 메모리상에 연속적으로 배치됩니다. 이는 인덱스를 통한 빠른 접근을 가능하게 합니다.
    - 첫 번째 요소의 메모리 주소를 기준으로 인덱스를 사용하여 다른 요소의 주소를 계산할 수 있습니다.

3. **인덱스를 통한 접근(Indexing)**
    - 배열의 각 요소는 인덱스를 통해 접근할 수 있습니다. 인덱스는 0부터 시작하여 배열의 크기 -1까지의 값을 가집니다.
    - 예: **'`numbers[0]`','`numbers[1]`',...,'`numbers[9]`'**

4. **동일한 데이터 타입(Homogeneous Data Type)**
    - 배열은 동일한 데이터 타입의 요소들로 구성됩니다. 즉, 배열 내 모든 요소는 같은 데이터 타입이어야 합니다.
    - 예: 정수형 배열, 문자열 배열 등.

### 장점.
1. **빠른 접근 속도(Fast Access) :**
    - 인덱스를 사용하여 O(1) 시간 복잡도로 배열의 임의의 요소에 접근할 수 있습니다. 이는 배열의 주요 장점 중 하나입니다.

2. **간단한 구현(Simple Implementation) :**
    - 배열은 데이터 구조가 간단하여 구현이 용이합니다. 기본적인 자료구조로, 다른 복잡한 자료구조의 기초가 됩니다.

### 단점.

1. **고정된 크기(Fixed Size) :**
    - 배열의 크기는 선언 시 결정되며, 크기를 변경할 수 없습니다. 이는 크기를 사전에 정확히 예측하기 어려운 경우 비효율적일 수 있습니다.

2. **삽입 및 삭제의 비효율성(Inefficient Insertions and Deletions) :**
    - 배열의 중간에 요소를 삽입하거나 삭제할 경우, 요소들을 이동시켜야 하기 때문에 O(n) 시간이 소요됩니다. 이는 큰 배열의 경우 성능 저하를 초래할 수 있습니다.

3. **메모리 낭비(Memory Waste) :**
    - 배열의 크기를 너무 크게 설정하면 사용되지 않는 메모리가 낭비될 수 있고, 너무 작게 설정하면 충분한 데이터를 저장할 수 없습니다.

### 배열의 사용 예시.

- **정수형 배열 선언 및 초기화**
```java
int[] numbers = new int[5];
numbers[0] = 10;
numbers[1] = 20;
numbers[2] = 30;
numbers[3] = 40;
numbers[4] = 50;
```

- **배열의 요소 접근**
```java
int firstElement = numbers[0]; // 10
int lastElement = numbers[4]; // 50
```

- **배열의 순회**
```java
for (int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}
```

### 마무리.

배열은 다양한 상황에서 기본적인 데이터 저장과 접근 방법을 제공하며, 특정 요구사항에 맞춰 다른 자료구조와 함께 사용되기도 합니다.

배열의 빠른 접근 속도와 간단한 구조 덕분에, 많은 알고리즘과 프로그램에서 핵심적인 역할을 합니다.

---

## 2️⃣ 배열 직접 구현.
```java
// CustomArray 클래스
public class CustomArray {
  private int[] data;
  private int size;

  // 특정 용량으로 배열을 초기화하는 생성자
  public CustomArray(int capacity) {
    data = new int[capacity];
    size = 0;
  }

  // 배열의 크기를 가져오는 메서드
  public int size() {
    return size;
  }

  // 배열이 비어 있는지 확인하는 메서드
  public boolean isEmpty() {
    return size == 0;
  }

  // 특정 인덱스의 요소를 가져오는 메서드
  public int get(int index) {
    if (index < 0 || index >= size) {
      throw new IndexOutOfBoundsException("Index out of bounds");
    }
    return data[index];
  }

  // 특정 인덱스에 요소를 설정하는 메서드
  public void set(int index, int value) {
    if (index < 0 || index >= size) {
      throw new IndexOutOfBoundsException("Index out of bounds");
    }
    data[index] = value;
  }

  // 배열에 요소를 추가하는 메서드
  public void add(int value) {
    if (size == data.length) {
      throw new IllegalStateException("Array is full");
    }
    data[size] = value;
    size++;
  }

  // 특정 인덱스의 요소를 삭제하는 메서드
  public void remove(int index) {
    if (index < 0 || index >= size) {
      throw new IndexOutOfBoundsException("Index out of bounds");
    }

    for (int i = index; i < size - 1; i++) {
      data[i] = data[i + 1];
    }
    size--;
  }

  // 모든 요소를 출력하는 메서드
  public void print() {
    for (int i = 0; i < size; i++) {
      System.out.print(data[i] + " ");
    }
    System.out.println();
  }
}
```

### 설명.
- **필드:**
    - **'data' :** 실제 데이터를 저장하는 배열.
    - **'size' :** 현재 배열에 저장된 요소의 개수.

- **생성자:**
    - **'CustomArray(int capacity)' :** 초기 용량을 설정하여 배열을 초기화 합니다.

- **메서드:**
    - **'size()' :** 현재 배열에 저장된 요소의 개수를 반환합니다.
    - **'isEmpty()' :** 배열이 비어있는지 확인합니다.
    - **'get(int index)' :** 특정 인덱스의 요소를 반환합니다.
    - **'set(int index, int value)' :** 특정 인덱스의 요소를 설정합니다.
    - **'add(int value)' :** 배열의 마지막에 요소를 추가합니다.
    - **'remove(int index)' :** 특정 인덱스의 요소를 제거하고, 이후의 요소들을 앞으로 이동시킵니다.
    - **'print()' :** 배열의 모든 요소를 출력합니다.
