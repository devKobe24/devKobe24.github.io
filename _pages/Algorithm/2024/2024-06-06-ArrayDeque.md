---
title: "📦[DS,Algorithm] ArrayDeque"
tags:
    - DataStructure
    - Algorithm
date: "2024-06-06"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 1️⃣ ArrayDeque.

Java에서 **'`ArrayDeque`'** 는 **'`java.util`'** 패키지에 속하는 클래스이며, 큐(Queue)와 덱(Deque)의 기능을 모두 지원하는 배열 기반의 자료 구조입니다.

**'`ArrayDeque`'** 는 **'`Deque`'** 인터페이스를 구현하며, 그기가 가변적인 배열을 사용하여 요소를 저장합니다.

# 2️⃣ 주요 특징.

- **이중 끝 큐 :** 양쪽 끝에서 요소를 추가하고 제거할 수 있습니다.

- **크기 조정 :** 필요에 따라 내부 배열의 크기를 자동으로 조정합니다.

- **스택 및 큐로 사용 가능 :** **'`ArrayDeque`'** 는 스택(LIFO, Last In First Out)과 큐(FIFO, First In First Out) 모두로 사용할 수 있습니다.

- **비동기적 :** **'`ArrayDeque`'** 는 비동기적으로 동작하므로 동기화된 환경에서 안전하지 않습니다.

# 3️⃣ 주요 메서드.

## 삽입 연산.

- **'`addFirst(E e)`' :** 지정된 요소를 덱의 앞쪽에 추가합니다.

- **'`addLast(E e)`' :** 지정된 요소를 덱의 뒤쪽에 추가합니다.

- **'`offerFirst(E e)`' :** 지정된 요소를 덱의 앞쪽에 추가합니다.

- **'`offerLast(E e)`' :** 지정된 요소를 덱의 뒤쪽에 추가합니다.

## 삭제 연산.

- **'`removeFirst()`' :** 덱의 앞쪽에서 요소를 제거하고 반환합니다.

- **'`removeLast()`' :** 덱의 뒤쪽에서 요소를 제거하고 반환합니다.

- **'`pollFirst()`' :** 덱의 앞쪽에서 요소를 제거하고 반환합니다.

- **'`pollLast()`' :** 덱의 뒤쪽에서 요소를 제거하고 반환합니다.

## 조회 연산.

- **'`getFirst()`' :** 덱의 앞쪽에 있는 요소를 반환합니다.

- **'`getLast()`' :** 덱의 뒤쪽에 있는 요소를 반환합니다.

- **'`peekFirst()`' :** 덱의 앞쪽에 있는 요소를 반환합니다.

- **'`peekLast()`' :** 덱의 뒤쪽에 있는 요소를 반환합니다.

## 스택 연산.

- **'`push(E e)`' :** 스택의 맨 위에 요소를 추가합니다.(LIFO, Last In First Out)

- **'`pop(E e)`' :** 스택의 맨 위에 있는 요소를 제거하고 반환합니다.(LIFO, Last In First Out)

# 4️⃣ 시간 복잡도.

- **삽입과 삭제 연산 :** **'`addFirst`'**, **'`addLast`'**, **'`removeFirst`'**, **'`removeLast`'**, **'`offerFirst`'**, **'`offerLast`'**, **'`pollFirst`'**, **'`pollLast`'**, 등의 연산은 평균적으로 O(1)입니다.

- **조회 연산 :** **'`getFirst`'**, **'`getLast`'**, **'`peekFirst`'**, **'`peekLast`'** 등의 연산은 O(1)입니다.

- **크기 조정 :** 베열의 크기가 가득 찼을 때 크기를 두 배로 늘리거나 줄이는 작업은 O(n) 시간이 걸리지만, 이는 드물게 발생하므로 평균적으로는 O(1)로 간주합니다. (amortized O(1)).

# 5️⃣ 예제 코드

아래의 코드는 **'`ArrayDeque`'** 를 사용한 예제 코드입니다.

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class ArrayDequeExample {

	public static void main(String[] args) {
		// ArrayDeque로 Deque 생성
		Deque<Integer> deque = new ArrayDeque<>();

		// 요소 삽입
		System.out.println("=== 요소 삽입 ===");
		deque.addFirst(1);
		deque.addLast(2);
		deque.offerFirst(0);
		deque.offerLast(3);
		System.out.println(deque);
		System.out.println();

		// 요소 조회
		System.out.println("=== 요소 조회 ===");
		System.out.println("First element: " + deque.getFirst());
		System.out.println("Last element: " + deque.getLast());
		System.out.println("Peek first element: " + deque.peekFirst());
		System.out.println("Peek last element: " + deque.peekLast());
		System.out.println();

		// 요소 삭제
		System.out.println("=== 요소 삭제 ===");
		System.out.println("Removed first element: " + deque.removeFirst());
		System.out.println("Removed last element: " + deque.removeLast());
		System.out.println("Poll first element: " + deque.pollFirst());
		System.out.println("Poll last element: " + deque.pollLast());
		System.out.println();

		// 덱의 크기와 비어 있는지 여부 확인
		System.out.println("=== 덱의 크기와 비어 있는지 여부 확인 ===");
		System.out.println("Deque size: " + deque.size());
		System.out.println("Is deque empty? " + deque.isEmpty());
		System.out.println();
		
		// 스택 연산
		System.out.println("=== 스택 연산 ===");
		deque.push(4);
		System.out.println("Pushed element: " + deque.peekFirst());
		System.out.println("Popped element: " + deque.pop());
	}
}

/*
=== 출력 ===
=== 요소 삽입 ===
[0, 1, 2, 3]

=== 요소 조회 ===
First element: 0
Last element: 3
Peek first element: 0
Peek last element: 3

=== 요소 삭제 ===
Removed first element: 0
Removed last element: 3
Poll first element: 1
Poll last element: 2

=== 덱의 크기와 비어 있는지 여부 확인 ===
Deque size: 0
Is deque empty? true

=== 스택 연산 ===
Pushed element: 4
Popped element: 4
*/
```
