---
title: "📦[DS,Algorithm] Circular Queue(원형 큐)의 중간 지점 찾기."
tags:
    - DataStructure
    - Algorithm
date: "2024-06-12"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 1️⃣ Circular Queue(원형 큐)의 중간 지점 찾기.

Java에서 배열을 사용하여 구현한 원형 큐에서 중간 지점을 찾는 방법은 큐의 시작 위치(**'front'**)와 끝 위치(**'rear'**)를 기준으로 계산할 수 있습니다.

<u>중간 지점을 찾는 공식은 원형 큐의 특성을 고려하여 적절히 조정되어야 합니다.</u>

# 2️⃣ 중간 지점을 찾기 위한 방법.

## 1️⃣ 중간 지점 계산 공식.

중간 지점을 찾는 방법은 큐의 시작점과 끝점을 이용하여 계산할 수 있습니다.

원형 큐의 크기, 시작 인덱스(front), 끝 인덱스(rear)를 사용하여 중간 인덱스를 계산할 수 있습니다.

이때 중간 지점을 계산하는 공식은 다음과 같습니다.

```java
(front + size / 2) % capacity
```

여기서 **'size'** 는 큐에 현재 저장된 요소의 수이고, **'capacity'** 는 큐의 전체 크기입나다.

# 3️⃣ 예시


```java
public class CircularQueue {
    private int[] queue;
    private int front, rear, size, capacity;

    public CircularQueue(int capacity) {
        this.capacity = capacity;
        this.queue = new int[capacity];
        this.front = 0;
        this.rear = 0;
        this.size = 0;
    }

    public boolean isFull() {
        return size == capacity;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public void enqueue(int data) {
        if (isFull()) {
            throw new RuntimeException("Queue is full");
        }
        queue[rear] = data;
        rear = (rear + 1) % capacity;
        size++;
    }

    public int dequeue() {
        if (isEmpty()) {
            throw new RuntimeException("Queue is empty");
        }
        int data = queue[front];
        front = (front + 1) % capacity;
        size--;
        return data;
    }

    public int getMiddle() {
        if (isEmpty()) {
            throw new RuntimeException("Queue is empty");
        }
        int middleIndex = (front + size / 2) % capacity;
        return queue[middleIndex];
    }

    public static void main(String[] args) {
        CircularQueue cq = new CircularQueue(5);
        cq.enqueue(10);
        cq.enqueue(20);
        cq.enqueue(30);
        cq.enqueue(40);
        cq.enqueue(50);
        
        System.out.println("Middle element: " + cq.getMiddle());  // Output: Middle element: 30
        
        cq.dequeue();
        cq.enqueue(60);
        
        System.out.println("Middle element: " + cq.getMiddle());  // Output: Middle element: 40
    }
}

```

이 코드에서는 **'CircularQueue'** 클래스를 정의하고, **'enqueue', 'dequeue', 'isFull', 'isEmpty'** 메서드를 포함합니다.

또한, 큐의 중간 요소를 반환하는 **'getMiddle'** 메서드를 정의합니다.

이 메서드는 현재 큐의 크기와 시작 인덱스를 사용하여 중간 인덱스를 계산한 후 해당 인덱스의 요소를 반환합니다.
