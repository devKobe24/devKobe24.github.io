---
title: "📦[DS,Algorithm] 원형 큐(Circular Queue)"
tags:
    - DataStructure
    - Algorithm
date: "2024-05-23"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 1️⃣ 원형 큐(Circular Queue).

원형 큐(Circular Queue)란, **"고정된 크기의 배열"** 을 사용하여 구현된 큐로서, 배열의 끝에 도달하면 다시 배열의 시작 부분으로 돌아가는 구조를 가진 큐입니다.

이를 통해 큐가 꽉 차 있는지, 비어 있는지를 효율적으로 관리할 수 있으며, 공간을 효율적으로 사용할 수 있습니다.

## 1️⃣ 원형 큐의 특징.
- 1. **선입선출(FIFO, First In First Out) :** 일반적인 큐와 마찬가지로 먼저 들어간 데이터가 먼저 나오는 구조입니다.
- 2. **순환 구조 :** 배열의 끝에 도달하면 다시 처음으로 돌아갑니다. 이를 통해 고정된 크기의 배열을 이용하여 메모리를 효율적으로 사용합니다.
- 3 **고정 크기 :** 큐의 최대 크기는 배열의 크기로 제한 됩니다.

## 2️⃣ 원형 큐의 주요 연산.
- 1. **Enqueue :** 큐의 뒤쪽(rear)에 새로운 요소를 추가합니다.
- 2. **Dequeue :** 큐의 앞쪽(front)에서 요소를 제거하고 반환합니다.
- 3. **Peek :** 큐의 앞쪽(front) 요소를 제거하지 않고 반환합니다.
- 4. **IsEmpty :** 큐가 비어 있는지 여부를 확인합니다.
- 5. **IsFull :** 큐가 가득 찼는지 여부를 확인합니다.

## 3️⃣ 원형 큐의 장점.
- **메모리 효율성 :** 고정된 크기의 배열을 사용하여 메모리를 효율적으로 사용합니다.
- **연속된 메모리 사용 :** 배열을 사용하여 메모리를 연속적으로 사용하므로 캐시 효율성이 높습니다.

## 4️⃣ 원형 큐의 단점.
- **고정 크기 제한 :** 크기가 고정되어 있으므로, 큐가 가득 찬 경우 더 이상 요소를 추가할 수 없습니다. 이 문제를 해결하려면 동적으로 크기를 조절할 수 있는 방법을 추가로 구현해야 합니다.

## 5️⃣ 원형 큐의 구현 예제

```java
// CircularQueue
public class CircularQueue {
  private int[] queue;
  private int front;
  private int rear;
  private int size;
  private int capacity;

  // 원형 큐 초기화
  public CircularQueue(int capacity) {
    this.capacity = capacity;
    queue = new int[capacity];
    front = 0;
    rear = -1;
    size = 0;
  }

  // 큐에 요소 추가
  public void enqueue(int element) {
    if (isFull()) {
      System.out.println("Queue is full");
      return;
    }

    rear = (rear + 1) % capacity;
    queue[rear] = element;
    size++;
  }

  // 큐에서 요소 제거 및 반환
  public int dequeue() {
    if (isEmpty()) {
      System.out.println("Queue is empty");
      return -1;
    }
    int element = queue[front];
    front = (front + 1) % capacity;
    size--;
    return element;
  }

  // 큐의 앞쪽 요소 반환
  public int peek() {
    if (isEmpty()) {
      System.out.println("Queue is empty");
      return -1;
    }
    return queue[front];
  }

  // 큐가 비어 있는지 확인
  public boolean isEmpty() {
    return size == 0;
  }

  // 큐가 가득 찼는지 확인
  public boolean isFull() {
    return size == capacity;
  }

  // 큐의 크기 반환
  public int getSize() {
    return size;
  }
}

// Main
public class Main {

  public static void main(String[] args) {
    CircularQueue circularQueue = new CircularQueue(5);

    circularQueue.enqueue(1);
    circularQueue.enqueue(2);
    circularQueue.enqueue(3);
    circularQueue.enqueue(4);
    circularQueue.enqueue(5);

    System.out.println("Dequeue: " + circularQueue.dequeue());
    System.out.println("Dequeue: " + circularQueue.dequeue());

    circularQueue.enqueue(6);
    circularQueue.enqueue(7);

    System.out.println("Peek: " + circularQueue.peek());

    while (!circularQueue.isEmpty()) {
      System.out.println("Dequeue: " + circularQueue.dequeue());
    }
  }
}

// 출력
/*
Dequeue: 1
Dequeue: 2
Peek: 3
Dequeue: 3
Dequeue: 4
Dequeue: 5
Dequeue: 6
Dequeue: 7
 */
```

### 코드 설명.

- 1. **CircularQueue 클래스:**
    - **'`queue`' :** 큐를 저장하는 배열.
    - **'`front`' :** 큐의 앞쪽 인덱스.
    - **'`rear`' :** 큐의 뒤쪽 인덱스.
    - **'`size`' :** 현재 큐에 저장된 요소의 개수.
    - **'`capacity`' :** 큐의 최대 크기.

- 2. **메서드:**
    - **'`enqueue(int element)`' :** 큐가 가득 차지 않았으면 요소를 큐에 추가합니다.
    - **'`dequeue()`' :** 큐가 비어 있지 않으면 큐의 앞쪽 요소를 제거하고 반환합니다.
    - **'`peek()`' :** 큐의 앞쪽 요소를 제거하지 않고 반환합니다.
    - **'`isEmpty()`' :** 큐가 비어 있는지 확인합니다.
    - **'`isFull()`' :** 큐가 가득 찼는지 확인합니다.
    - **'`getSize()`' :** 큐의 현재 크기를 반환합니다.

- 3. **main 메서드:**
    - 원형 큐를 생성하고 여러 연산을 수행하여 큐의 동작을 테스트합니다.
