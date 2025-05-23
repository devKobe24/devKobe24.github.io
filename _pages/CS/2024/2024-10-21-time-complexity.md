---
title: "💾 [CS] 알고리즘의 시간 복잡도(Time Complexity)란 무엇일까요?"
tags:
    - CS
date: "2024-10-21"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 알고리즘의 시간 복잡도(Time Complexity)란 무엇일까요?
- **시간 복잡도(Time Complexity)는 알고리즘이 입력 크기에 따라 실행하는 연산의 수를 측정하는 지표로, 알고리즘의 효율성을 평가하는 데 사용됩니다.** 
- 시간 복잡도(Time Complexity)는 주로 **입력 크기(n)가** 커질수록 알고리즘의 실행시간이 얼마나 빨리 증가하는 지를 나타내며, **빅-오 표기법(Big-O Notaion)으로** 표현됩니다.

## 1️⃣ 왜 시간 복잡도(Time Complexity)가 중요한가요?

### 1️⃣ 효율적인 알고리즘 설계.
- 알고리즘의 효율성을 평가할 때, 입력 데이터의 크기가 커질수록 얼마나 빠르게 실행 되는지를 고려해야 합니다.
    - 시간 복잡도(Time Complexity)를 통해 알고리즘(Algorithm)의 **성능을 비교하고, 더 나은 알고리즘(Algorithm)을** 선택할 수 있습니다.

### 2️⃣ 대규모 데이터 처리.
- 실제 프로그램은 대규모 데이터를 처리해야 할 때가 많습니다.
    - 시간 복잡도(Time Complexity)가 높은 알고리즘(Algorithm)은 큰 입력을 처리할 때 **실행 시간이 크게 증가하여** 성능이 저하될 수 있습니다.

### 3️⃣ 성능 예측.
- 시간 복잡도(Time Complexity)를 알면 입력 크기 변화에 따른 알고리즘의 성능을 예측할 수 있습니다.
    - 이를 통해 최적화할 부분을 식별하고, 더 효율적인 코드로 개선할 수 있습니다.

## 2️⃣ 시간 복잡도(Time Complexity)의 기본 개념.
- 시간 복잡도(Time Complexity)는 **입력 크기(n)에** 따라 알고리즘이 **수행해야 하는 기본 연산의 수를 나타냅니다.**
- 일반적으로 알고리즘의 실행 시간을 절대적인 **초 단위**로 측정하지 않고, **연산 횟수**로 **추상화하여** 표현합니다.
    - 이는 **하드웨어 성능에 따라 실제 시간은 다를 수 있지만, 연산 횟수는 동일하기 때문**입니다.
- 시간 복잡도(Time Complexity)는 **최악의 경우**를 기준으로 하는 것이 일반적이며, 이는 알고리즘(Algorithm)의 성능을 보장하는 데 도움이 됩니다.

## 3️⃣ 빅-오 표기법(Big-O Notation)
- **빅-오 표기법(Big-O Notation)은** 시간 복잡도(Time Complexity)를 표현하는 방식으로, **입력 크기(n)가 증가할 때 알고리즘의 실행 시간이 얼마나 빠르게 증가하는지를 나타냅니다.**

### 1️⃣ 주요 빅-오 표기법.

#### 1️⃣ O(1) 👉 상수 시간(Constant Time)
- **알고리즘의 실행 사간이 입력 크기와 상관업시 일정합니다.**
- 예: 배열의 특정 인덱스에 접근하기(예: `arr[0]`)

#### 2️⃣ O(lon g) 👉 로그 시간(Longarithmic Time)
- **입력 크기가 커질 수록 실행 시간이 완만하게 증가합니다.**
- 예: 이진 탐색(Binary Search)

#### 3️⃣ O(n) 👉 선형 시간(Linear Time)
- **입력 크기에 비례하여 실행 시간이 증가합니다.**
- 예: 배열에서 특정 값을 찾기 위해 모든 요소를 검사하는 경우.

#### 4️⃣ O(n log n) 👉 선형 로그 시간(Linearithmic Time)
- 선형 시간보다 더 복잡한 시간 복잡도.
    - **일반적으로 효율적인 정렬 알고리즘에서 발생합니다.**
- 예: 퀵 정렬(Quick Sort), 병합 정렬(Merge Sort)

#### 5️⃣ O(n^2) 👉 이차 시간(Quadratic Time)
- 입력 크기에 대해 **제곱에 비례하여 실행 시간이 증가합니다.**
    - 일반적으로 **중첩된 반복문에서 발생합니다.**
- 예: 버블 정렬(Bubble Sort), 삽입 정렬(Insertion Sort)

#### 6️⃣ O(2^n) 👉 지수 시간(Exponential Time)
- **입력 크기가 커질수록 실행 시간이 지수적으로 증가합니다.**
    - 보통 입력 크기가 작을 때만 사용할 수 있습니다.
- 예: 피보나치 수열을 재귀적으로 계산하는 알고리즘

#### 7️⃣ O(n!) 👉 팩토리얼 시간(Factorial Time)
- **입력 크기가 커질수록 실행 시간이 매우 빠르게 증가합니다.**
    - 주로 모든 가능한 순열을 계산하는 알고리즘에서 발생합니다.
- 예: 순열(permutation) 생성.

## 4️⃣ 시간 복잡도의 예시.

### 1️⃣ O(1) 👉 상수 시간.
```java
int getFirstElement(int[] arr) {
    return arr[0]; // 입력 크기와 상관없이 일정한 시간
}
```

### 2️⃣ O(n) 👉 선형 시간.
```java
int sum(int[] arr) {
    int sum = 0;
    for (int num : arr) {
        sum += num; // 배열의 모든 요소를 순회
    }
    return sum;
}
```

### 3️⃣ O(n^2) 👉 이차 시간.
```java
void printPairs(int[] arr) {
    for(int i = 0; i < arr.length; i++) {
        for(int j = 0; j < arr.length; j++) {
            System.out.println(arr[i] + ", " + arr[j]); // 중첩된 반복문
        }
    }
}
```

## 5️⃣ 시간 복잡도 분석 방법.

### 1️⃣ 반복문.
- 반복문이 한 번 실행될 때마다 **O(n).**
    - 중첩된 반복문 **O(n^2), O(n^3)와** 같은 더 높은 차수의 시간 복잡도를 가집니다.

### 2️⃣ 조건문.
- **if-else** 조건문은 입력 크기에 영향을 미치지 않는 경우 **O(1)**

### 3️⃣ 재귀 호출.
- 재귀 함수의 시간 복잡도는 재귀 호출의 깊이와 각 호출의 작업에 따라 달라집니다.
    - **피보나치 재귀 함수는 O(2^n)** 시간 복잡도를 가집니다.

### 4️⃣ 연산.
- 일반적인 산술 연산, 비교, 할당 등은 **O(1)**

## 6️⃣ 시간 복잡도 예측의 중요성.
- 시간 복잡도를 잘 이해하면, 알고리즘을 설계할 때 **효율성을 예측**할 수 있습니다.
- 코딩테스트나 실제 개발에서도 **성능 최적화**가 중요한데, 시간 복잡도(Time Complexity)를 통해 입력 크기에 따라 알고리즘이 어떻게 동작할지 미리 예상할 수 있습니다.

## 7️⃣ 요약.
- **시간 복잡도(Time Complexity)는** 알고리즘이 **입력 크기에 따라 얼마나 빠르게 실행 시간이 증가하는지를 평가하는 지표로, 알고리즘의 효율성을 비교하고 성능을 최적화하는 데 중요한 역할을 합니다.**
- **빅-오 표기법(Big-O Notation)은 시간 복잡도(Time Complexity)를 표현하는 방식으로, 알고리즘의 성능을 추상적이고 간결하게 나타내오, 입력 크기 변화에 따른 알고리즘의 동작을 예측할 수 있게 도와줍니다.**
- 효율적인 알고리즘을 설계하기 위해서는 **시간 복잡도에 대한 깊은 이해**가 필수적입니다.
