---
title: "📦[DS,Algorithm] Circular Queue(원형 큐)를 배열로 구현시 rear를 -1으로 설정하지 않는 이유."
tags:
    - DataStructure
    - Algorithm
date: "2024-06-13"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 1️⃣ Circular Queue(원형 큐)를 배열로 구현시 rear를 -1으로 설정하지 않는 이유.

Java에서 원형 큐를 구현할 때 **'rear'** 의 초기값을 **'-1'** 로 설정하지 않는 이유는 여러 가지가 있습니다.

<u>주요 이유는 큐의 인덱싱을 단순화하고, 논리적인 흐름을 일관되게 유지하기 위함입니다.</u>

다음은 그 이유를 자세히 설명한 것입니다.

## 1️⃣ 인덱스의 일관성 유지.

- **'rear'** 를 **'0'** 으로 초기화하면 큐의 인덱싱이 단순해집니다.
    - **'rear'** 와 **'front'** 모두 0에서 시작하여, 큐의 크기를 **'capacity'** 로 나눈 나머지를 사용하여 인덱스를 순환시킵니다.
    - 이는 모듈로 연산을 사용하여 인덱스를 관리하는 데 있어 편리합니다.

- **'rear'** 를 **'-1'** 로 설정할 경우, 요소를 추가할 때마다 매번 **'rear'** 를 **'0'** 으로 조정하는 특별한 처리가 필요하게 됩니다.
    - 이는 코드의 복잡성을 증가시키고 실수의 가능성을 높입니다.

## 2️⃣ 코드의 단순화.

- **'rear'** 를 **'0'** 으로 초기화하면 초기 상태와 요소 추가, 삭제시 별도의 조건 검사를 줄일 수 있습니다.
    - 예를 들어 **'rear'** 가 **'-1'** 인지 확인하는 추가 조건문을 피할 수 있습니다.

- **'0'** 부터 시작하면 **'enqueue'** 와 **'dequeue'** 연산에서 **'rear'** 와 **'front'** 인덱스의 증가와 감소가 일관되게 처리됩니다.

## 3️⃣ 편리한 초기 상태 처리.

- **'rear'** 를 **'0'** 으로 설정하면 초기 상태에서 큐가 비어 있는지 확인하는 것이 더 직관적입니다.
    - 예를 들어 **'isEmpty'** 메서드는 단순히 **'size'** 변수를 확인하여 큐가 비어 있는지 확인할 수 있습니다. **'rear'** 가 **'-1'** 이면 추가적인 상태 검사가 필요할 수 있습니다.

# 2️⃣ 예시 코드.

다음은 **'rear'** 의 초기값을 **'0'** 으로 설정하는 원형 큐의 코드입니다.

```java
public class CircularQueue {
	private int[] queue;
	private int front;
	private int rear;
	private int size;
	private int capacity;

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

	public int peek() {
		if (isEmpty()) {
			throw new RuntimeException("Queue is empty");
		}
		return queue[front];
	}

	public int getSize() {
		return size;
	}

	public void displayCircularQueue() {
		if (isEmpty()) {
			throw new RuntimeException("Queue is empty");
		}
		int i = front;
		int count = 0;
		while (count < size) {
			System.out.print("["+ queue[i] + "]");
			i = (i + 1) % capacity;
			count++;
		}
		System.out.println();
	}

	public int getMiddle() {
		if (isEmpty()) {
			throw new RuntimeException("Queue is empty");
		}
		int middleIndex = (front + size / 2) % capacity;
		return queue[middleIndex];
	}
}

// Main
public class Main {

	public static void main(String[] args) {
		CircularQueue circularQueue = new CircularQueue(5);
		CircularQueue.enqueue(10);
		CircularQueue.enqueue(20);
		CircularQueue.enqueue(30);
		CircularQueue.enqueue(40);
		CircularQueue.enqueue(50);
		CircularQueue.displayCircularQueue();
		System.out.println();

		System.out.println("Middle element: " + circularQueue.getMiddle());
		circularQueue.displayCircularQueue();
		System.out.println();

		System.out.println("===DEQUEU===");
		System.out.println(circularQueue.dequeue());
		circularQueue.enqueue(60);
		circularQueue.displayCircularQueue();
		System.out.println();

		System.out.println("Middle element: " + circularQueue.getMiddle());
		circularQueue.displayCircularQueue();
	}
}

// === 출력 ===
/*
[10][20][30][40][50]

Middle element: 30
[10][20][30][40][50]

===DEQUEU===
10
[20][30][40][50][60]

Middle element: 40
[20][30][40][50][60]
*/
```

> 위 코드에서 **'rear'** 를 0으로 설정함으로써 큐의 인덱싱과 상태 처리가 단순해지고, 이해하기 쉬워집니다.
> 이는 코드의 가독성과 유지보수성을 높이는 데 도움이 됩니다.
