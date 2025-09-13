---
title: "📦[DS,Algorithm] Java의 배열."
tags:
    - DataStructure
    - Algorithm
date: "2024-06-02"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 1️⃣ Java의 배열.

## 1️⃣ 배열이란 무엇인가?

배열(Array)은 동일한 타입의 여러 요소를 하나의 변수로 관리할 수 있게 해주는 자료구조입니다.

배열은 연속된 메모리 공간에 할당되며, 각 요소는 인덱스를 통해 접근할 수 있습니다.

## 2️⃣ 배열의 선언과 초기화.

Java에서 배열은 다음과 같이 선언하고 초기화할 수 있습니다.

```java
int[] array = new int[5]; // 크기가 5인 정수형 배열 선언.
int[] array = {10, 20, 30, 40, 50}; // 초기화와 동시에 배열 선언
```

## 3️⃣ 배열의 요소와 접근.

배열의 각 요소는 인덱스를 통해 접근할 수 있으며, 인덱스는 0부터 시작합니다.

```java
int firstElement = array[0]; // element = 10, 첫 번째 요소에 접근
array[1] = 25; // [10, 25, 30, 40, 50], 두 번째 요소에 값 25를 저장
```

## 4️⃣ 배열의 시간 복잡도.

배열의 시간 복잡도는 연산의 종류에 따라 다릅니다.

아래는 일반적인 배열 연산과 그 시간 복잡도를 설명한 것입니다.

### 1. 접근(Access)

- 특정 인덱스의 요소에 접근하는 시간 복잡도는 O(1)입니다.
    - 이유 : 배열은 연속된 메모리 공간에 저장되므로 인덱스를 통해 바로 접근할 수 있기 때문입니다.
    
```java
// 접근(Access)
int element = array[2]; 
// element = 30,  time complexity = O(1)
// [10, 25, 30, 40, 50]
```

### 2. 탐색(Search)

- 배열에서 특정 값을 찾는 시간 복잡도는 O(n)입니다.
    - 이유: 최악의 경우 배열의 모든 요소를 검사해야 할 수도 있기 때문입니다.
    
```java
boolean found = false;
int target = 30;

for (int i = 0; i < array.length; i++) {
    if (array[i] == target) { // i = 2, array[i] = 30
        found = true;
        break;
    }
}
// [10, 25, 30, 40, 50]
```

### 3. 삽입(Insertion)

- 배열의 끝에 요소를 추가하는 시간 복잡도는 O(1)입니다.
- 배열의 특정 위치에 요소를 삽입하는 시간 복잡도는 O(n)입니다.
    - 이유: 특정 위치에 삽입하기 위해서는 해당 위치 이후의 모든 요소를 한 칸씩 뒤로 밀어야 하기 때문입니다.
    
```java
// 삽입(Insertion)

// 배열 삽입시 index가 array.length가 아니고 array.length - 1인 이유는
// array.length는 배열의 크기, 즉 5를 나타내기 때문입니다.
// index는 0부터 시작하기 때문에 배열의 크기가 5인 배열의 끝 index는 4입니다.
// 때문에 array.length - 1을 해줍니다.

array[array.length - 1] = 60; // 배열 끝에 삽입 (O(1)), [10, 25, 30, 40, 60]

// 배열 중간에 삽입하는 메서드
public static void insertion(int[] array, int index, int insertValue) {
  // 배열 중간에 삽입(O(n))
  for (int i = array.length - 1; i > index; i--) {
    array[i] = array[i - 1];
  }
  array[index] = insertValue;
  System.out.println(Arrays.toString(array));
}
```

### 4. 삭제(Deletion)

- 배열의 끝에서 요소를 제거하는 시간 복잡도는 O(1)입니다.
- 배열의 특정 위치의 요소를 제거하는 시간 복잡도는 O(n)입니다.
	- 이유: 특정 위치의 요소를 제거한 후에는 해당 위치 이후의 모든 요소를 한 칸씩 앞으로 당겨야 하기 때문입니다.
  
```java
// 삭제(Deletion)
    array[array.length - 1] = 0; // 배열의 끝에서 삭제 ((O(1)), [10, 25, 30, 77, 0]
    System.out.println(Arrays.toString(array));

    // 배열 중간에서 삭제하는 메서드
    int deletionValue = deletion(array, 2);
    System.out.println(deletionValue); // 30

// 배열 중간에 삭제하는 메서드
  public static int deletion(int[] array, int index) {
    // 배열 중간에 삭제(O(n))
    int[] returnValue = new int[array.length];

    for (int i = index, j = 0; i < array.length - 1 ; i++) {
      returnValue[j] = array[i];
      j++;
      array[i] = array[i + 1];

    }
    array[array.length - 1] = 0; // 마지막 요소 초기화.
    int deletionValue = returnValue[0]; // 배열을 메모리에서 지우기
    returnValue = null;
    return deletionValue;
  }
```

## 5️⃣ 배열의 장점과 단점.

### 장점.
- **빠른 접근 속도 :** 인덱스를 통해 O(1) 시간에 요소를 접근할 수 있습니다.
- **메모리 효율 :** 연속된 메모리 공간을 사용하므로 메모리 사용이 효율적입니다.

### 단점.
- **고정된 크기 :** 배열의 크기는 선언 시에 고정되므로, 실행 중에 크기를 변경할 수 없습니다.
- **삽입 및 삭제의 비효율성 :** 배열 중간에 요소를 삽입하거나 삭제할 때 O(n)의 시간이 소요됩니다.
- **연속된 메모리 할당 필요 :** 큰 배열을 사용할 떄는 연속된 메모리 공간이 필요하여 메모리 할당에 제한이 있을 수 있습니다.

> 배열은 이러한 특성들로 인해 빠른 접근이 필요한 상황에서는 매우 유용하지만, 삽입 및 삭제가 빈번히 일어나는 경우에는 비효율적일 수 있습니다.
> 따라서 상황에 맞게 적절한 자료구조를 선택하는 것이 중요합니다.
