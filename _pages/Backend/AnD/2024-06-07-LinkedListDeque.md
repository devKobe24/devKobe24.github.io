---
title: "📦[DS,Algorithm] LinkedList를 사용한 Deque."
tags:
    - DataStructure
    - Algorithm
date: "2024-06-07"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 1️⃣ LinkedList를 사용한 Deque.

**'`LinkedList`'** 는 **'`Deque`'** 인터페이스를 구현한 클래스 중 하나로, 양쪽 끝에서 삽입과 삭제가 가능한 이중 연결 리스트 기반의 자료 구조입니다.

**'`LinkedList`'** 는 **'`Deque`'** 뿐만 아니라 **'`List`'**, **'`Queue`'** 인터페이스도 구현하여 다양한 형태로 사용할 수 있습니다.

# 2️⃣ 주요 특징.

- **이중 끝 큐 :** 양쪽 끝에서 요소를 추가하고 제거할 수 있습니다.

- **이중 연결 리스트 :** 각 노드는 이전 노드와 다음 노드를 가리키는 두 개의 포인터를 가집니다.

- **비동기적 :** **'`LinkedList`'** 는 비동기적으로 동작하므로 동기화된 환경에서 안전하지 않습니다.

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

- **'`push(E e)`' :** 스택의 맨 위에 요소를 추가합니다.(FIFO, First In First Out)

- **'`pop()`' :** 스택의 맨 위에 있는 요소를 제거하고 반환합니다.(LIFO, Last In First Out)

# 4️⃣ 시간 복잡도

- **삽입과 삭제 연산 :** **'`addFirst`'**, **'`addLast`'**, **'`removeFirst`'**, **'`removeLast`'**, **'`offerFirst`'**, **'`offerLast`'**, **'`pollFirst`'**, **'`pollLast`'** 등의 연산은 O(1)입니다.
    - 이중 연결 리스트를 사용하기 때문에 양쪽 끝에서의 삽입과 삭제는 상수 시간 내에 수행됩니다.

- **조회 연산 :** **'`getFirst`'**, **'`getLast`'**, **'`peekFirst`'**, **'`peekLast`'** 등의 연산은 O(1)입니다.

- **임의 접근 연산( **'`get(int index)`'**, **'`set(int index, E element)`' 등**) :** 인덱스를 사용한 접근 연산은 리스트의 중간에 있는 요소를 찾기 위해 리스트를 순회해야 하므로 O(n) 시간이 걸립니다.

# 5️⃣ 코드 예시.

아래 코드는 **'`LinkedList`'** 를 **'`Deque`'** 로 사용하는 예제입니다.

```java
import java.util.Deque;
import java.util.LinkedList;

public class LinkedListDequeExample {

	public static void main(String[] args) {
		// LinkedList로 Deque 생성
		Deque<Integer> deque = new LinkedList<>();

		// 요소 삽입
		deque.addFirst(1);
		deque.addLast(2);
		deque.offerFirst(0);
		deque.offerLast(3);

		// 요소 조회
		System.out.println("First element: " + deque.getFirst());
		System.out.println("Last element: " + deque.getLast());
		System.out.println("Peek first element: " + deque.peekFirst());
		System.out.println("Peek last element: " + deque.peekLast());

		// 요소 식제
		System.out.println("Removed first element: " + deque.removeFirst());
		System.out.println("Removed last element: " + deque.removeLast());
		System.out.println("Poll first element: " + deque.pollFirst());
		System.out.println("Poll last element: " + deque.pollLast());

		// 덱의 크기와 비어 있는지 여부 확인
		System.out.println("Deque size: " + deque.size());
		System.out.println("Is deque empty? " + deque.isEmpty());

		// 스택 연산.
		deque.push(4);
		System.out.println("Pushed element: " + deque.peekFirst());
		System.out.println("Popped element: " + deque.pop());
	}
}
/*
=== 출력 ===

First element: 0
Last element: 3
Peek first element: 0
Peek last element: 3
Removed first element: 0
Removed last element: 3
Poll first element: 1
Poll last element: 2
Deque size: 0
Is deque empty? true
Pushed element: 4
Popped element: 4
*/
```

## 🙋‍♂️ 설명.

- 1. **베열 초기화 :** **'`DEFAULT_CAPACITY`'** 크기의 배열을 초기화하고, **'`head`'**, **'`tail`'**, **'`size`'** 변수를 초기화 합니다.

- 2. **삽입 연산( **'`addFirst`'**, **'`addLast`'**) :** 요소를 덱의 첫 번째 또는 마지막에 추가합니다.

- 3. **삭제 연산( **'`removeFirst`'**, **'`removeLast`'**) :** 첫 번째 요소와 마지막 요소를 각각 제거합니다.

- 4. **조회 연산( **'`getFirst`'**, **'`getLast`'**, **'`peekFirst`'**, **'`peekLast`'**) :** 첫 번째 요소와 마지막 요소를 반환합니다.

- 5. **기타 메서드 :** **'`size`'** 와 **'`isEmpty`'** 메서드는 덱의 크기와 비어 있는지 여부를 반환합니다.

- 6. **스택 연산( **'`push`'**, **'`pop`'**) :** 스택의 맨 위에 요소를 추가하고, 스택의 맨 위에 있는 요소를 제거하고 반환합니다.

> 위 예시 코드에서는 **'`LinkedList`'** 를 **'`Deque`'** 로 사용하여 다양한 연산을 수행하는 방법을 보여줍니다.
> **'`LinkedList`'** 는 이중 연결 리스트를 사용하기 때문에 양쪽 끝에서의 삽입과 삭제가 빠르고 효율적입니다.
