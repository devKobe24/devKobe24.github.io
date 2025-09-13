---
title: "📦[DS,Algorithm] Circular Queue(원형 큐)란?"
tags:
    - DataStructure
    - Algorithm
date: "2024-06-09"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 1️⃣ Circular Queue(원형 큐)란?

원형 큐는 큐의 일종으로, 배열을 사용하여 구현되며, 큐의 마지막 위치가 처음 위치와 연결되어 원형 구조를 가지는 큐입니다.

원형 큐는 고정된 크기의 배열을 사용하여 구현되므로, <u>큐의 마지막 인덱스가 배열의 끝에 도달하면 다음 인덱스가 배열의 시작 부분으로 이동합니다.</u>

이를 통해 메모리를 효율적으로 사용할 수 있으며, 큐의 처음과 끝을 관리하는 데 도움이 됩니다.

# 2️⃣ 원형 큐의 원리.

1. **고정된 크기 :** 원형 큐는 고정된 크기의 배열을 사용하여 구현됩니다. 따라서 배열의 크기를 초과하여 요소를 추가할 수 없습니다.

2. **연결된 인덱스 :** 큐의 마지막 인덱스가 배열의 끝에 도달하면, 다음 인덱스는 배열의 처음 부분으로 이동합니다.

3. **두 개의 포인터 :** 원형 큐는 두 개의 포인터를 사용하여 구현됩니다.
    - **'front' :** 큐의 첫 번째 요소를 가리킵니다.
    - **'rear' :** 큐의 마지막 요소를 가리킵니다.

4. **비어 있는 상태와 가득 찬 상태 :** 큐가 비어 있는 상태와 가득 찬 상태를 구별해야 합니다. 이를 위해 추가적인 변수를 사용하거나 포인터의 위치를 비교하여 상태를 확인합니다.

# 3️⃣ 원형 큐의 주요 연산.

1. **초기화 :** 큐의 크기를 설정하고, **'front'** 와 **'rear'** 포인터를 초기화합니다.

2. **isEmpty() :** 큐가 비어 있는지 확인합니다.

3. **isFull() :** 큐가 가득 찼는지 확인합니다.

4. **enqueue() :** 큐에 요소를 추가합니다. **'rear'** 포인터를 업데이트합니다.

5. **dequeue() :** 큐에서 요소를 제거하고 반환합니다. **'front'** 포인터를 업데이트합니다.

6. **peek() :** 큐의 첫 번째 요소를 반환합니다.

# 4️⃣ 원형 큐의 예제 구현.

```java
public class CircularQueue {
    private int[] queue;
    private int front;
    private int rear;
    private int size;
    private int capacity;

    // 생성자
    public CircularQueue(int capacity) {
        this.capacity = capacity;
        queue = new int[capacity];
        front = 0;
        rear = -1;
        size = 0;
    }

    // 큐가 비어 있는지 확인
    public boolean isEmpty() {
        return size == 0;
    }

    // 큐가 가득 찼는지 확인
    public boolean isFull() {
        return size == capacity;
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

    // 큐에서 요소 제거
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

    // 큐의 첫 번째 요소 확인
    public int peek() {
        if (isEmpty()) {
            System.out.println("Queue is empty");
            return -1;
        }
        return queue[front];
    }

    // 큐의 크기 반환
    public int getSize() {
        return size;
    }

    // 큐의 모든 요소 출력
    public void display() {
        if (isEmpty()) {
            System.out.println("Queue is empty");
            return;
        }
        int i = front;
        int count = 0;
        while (count < size) {
            System.out.print(queue[i] + " ");
            i = (i + 1) % capacity;
            count++;
        }
        System.out.println();
    }

    // 메인 메서드 (테스트용)
    public static void main(String[] args) {
        CircularQueue cq = new CircularQueue(5);

        cq.enqueue(10);
        cq.enqueue(20);
        cq.enqueue(30);
        cq.enqueue(40);
        cq.enqueue(50);

        cq.display(); // 출력: 10 20 30 40 50

        System.out.println("Dequeued: " + cq.dequeue()); // 출력: Dequeued: 10
        System.out.println("Dequeued: " + cq.dequeue()); // 출력: Dequeued: 20

        cq.display(); // 출력: 30 40 50

        cq.enqueue(60);
        cq.enqueue(70);

        cq.display(); // 출력: 30 40 50 60 70

        System.out.println("Front element: " + cq.peek()); // 출력: Front element: 30
    }
}
```

## 🙋‍♂️ 설명.

1. **큐 초기화:**
    - **'capacity' :** 큐의 최대 크기입니다.
    - **'queue' :** 큐를 저장할 배열입니다.
    - **'front' :** 큐의 첫 번째 요소를 가리키는 인덱스입니다.
    - **'rear' :** 큐의 마지막 요소를 가리키는 인덱스입니다.
    - **'size' :** 큐에 있는 요소의 개수입니다.

2. **메서드:**
    - **'isEmpty()' :** 큐가 비어 있는지 확인합니다.
    - **'isFull()' :** 큐가 가득 찼는지 확인합니다.
    - **'enqueue(int element)' :** 큐에 요소를 추가합니다.
    - **'dequeue()' :** 큐에서 요소를 제거하고 반환합니다.
    - **'peek()' :** 큐의 첫 번째 요소를 반환합니다.
    - **'getSize()' :** 큐에 있는 요소의 개수를 반환합니다.
    - **'display()' :** 큐의 모든 요소를 출력합니다.

# 5️⃣ 결론.

원형 큐는 배열을 효율적으로 사용하여 큐의 크기를 고정하고, 처음과 끝이 연결된 형태로 큐를 관리하는 자료구조입니다.

이를 통해 큐의 공간을 최대한 활용하고, 큐가 비어 있는지 가득 찼는지를 쉽게 확인할 수 있습니다.

# 🤔 궁금했던 부분.

```java
rear = (rear + 1) % capacity;
```

## 1️⃣ 이 코드에서 `% capacity` 를 하는 이유는 무엇일까?

원형 큐에서 **'rear'** 포인터를 업데이트 할 때 **`% capacity`** 를 사용하는 이유는 큐가 마지막 인덱스에 도달한 후, 다시 처음 인덱스로 돌아가도록 하기 위해서입니다.

이를 통해 큐가 원형으로 동작할 수 있습니다.

구체적으로 말하면, 큐의 크기를 고정된 크기의 배열로 구현할 때, 배열의 끝에 도달했을 때 다시 처음으로 돌아가는 기능을 제공합니다.

## 2️⃣ % 연산자의 역할.

배열의 인덱스는 0부터 시작하여 배열의 크기보다 1 작은 값까지입니다.

예를 들어, 배열의 크기가 5라면 인덱스는 0부터 4까지입니다.

원형 큐에서 새로운 요소를 추가할 때마다 **'rear'** 포인터를 증가시키는데, 이 포인터가 배열의 끝을 넘어가지 않도록 해야 합니다.

이를 위해 **`% capacity`** 연산을 사용합니다.

- **`rear = (rear + 1) % capacity;`**

이 연산은 **'rear'** 포인터를 1씩 증가시키다가, 배열의 끝에 도달하면 다시 0으로 돌아가게 합니다.

즉, 배열의 인덱스가 배열의 크기를 넘어가면, 다시 처음 인덱스(0)로 순환되게 합니다.

### 👉 예제.

배열의 크기가 5인 원형 큐를 생각해봅시다.

- 초기 상태: **'rear = -1'**
- 요소 추가 시, **'rear'** 포인터의 변화를 관찰해보면
    - 첫 번째 추가: **'rear = (rear + 1) % 5 -> rear = 0'**
    - 두 번째 추가: **'rear = (rear + 1) % 5 -> rear = 1'**
    - 세 번째 추가: **'rear = (rear + 1) % 5 -> rear = 2'**
    - 네 번째 추가: **'rear = (rear + 1) % 5 -> rear = 3'**
    - 다섯 번째 추가: **'rear = (rear + 1) % 5 -> rear = 4'**
    - 여섯 번째 추가: **'rear = (rear + 1) % 5 -> rear = 0'** (다시 처음으로 돌아감)

이렇게 **'rear'** 포인터가 배열의 끝에 도달하면 다시 배열의 시작 부분으로 순환되므로, 배열을 효율적으로 사용할 수 있게 됩니다.

### 💻 코드 예제.

위 개념을 이용한 원형 큐의 **'enqueue'** 메서드 구현

```java
public void enqueue(int element) {
    if (isFull()) {
        System.out.println("Queue is full");
        return;
    }
    rear = (rear + 1) % capacity; // rear 포인터를 증가시키고, 배열의 처음으로 순환시킴.
    queue[rear] = element;
    size++;
}
```

# 6️⃣ 정리.

원형 큐에서 **'% capacity'** 연산은 **'rear'** 포인터와 **'front'** 포인터가 배열의 끝에 도달했을 때, 다시 배열의 시작 부분으로 돌아가기 위해 사용됩니다.

이를 통해 배열의 고정된 크기를 효율적으로 활용하며, 원형 큐의 특성을 유지할 수 있습니다.
