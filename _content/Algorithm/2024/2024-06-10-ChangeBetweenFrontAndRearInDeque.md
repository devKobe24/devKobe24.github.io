---
title: "📦[DS,Algorithm] Deque에서의 front와 rear의 변화."
tags:
    - DataStructure
    - Algorithm
date: "2024-06-10"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 🧨 시발점.

Deque을 공부하던 중 동적으로 변하는 front와 rear가 근본적으로 어떻게 동작하는지 궁금해졌습니다.

이것을 알게되면 정확하게 Deque의 addFirst, addLast, removeFirst, removeLast 시 front와 rear가 어디에 위치하는지 알 수 있고 Deque의 원리를 이해 할 수 있을 것 같았습니다.

# 1️⃣ Deque의 front와 rear의 위치는 변할 수 있나요? 🤔

**'`Deque`'** (Double Ended Queue)에서 **'`front`'** 와 **'`rear`'** 의 위치는 변할 수 있습니다.

**'`Deque`'** 는 양쪽 끝에서 삽입과 삭제가 모두 가능한 자료구조이기 때문에, **'`front`'** 와 **'`rear`'** 의 위치는 데이터가 삽입되거나 제거될 때마다 변합니다.

# 2️⃣ Deque에서의 front와 rear의 변화. 🤩

## 1️⃣ 삽입 연산 (**'`addFirst`'** 와 **'`addLast`'**)

- **'`addFirst`' :** 요소를 덱의 앞쪽에 삽입합니다.
    - **'`front`'** 위치가 바뀝니다.

- **'`addLast`' :** 요소를 덱의 뒤쪽에 삽입합니다.
    - **'`rear`'** 위치가 바뀝니다.

## 2️⃣ 삭제 연산 (**'`removeFirst`'** 와 **'`removeLast`'**)

- **'`removeFirst`' :** 덱의 앞쪽에서 요소를 제거합니다.
    - **'`front`'** 위치가 바뀝니다.

- **'`removeLast`' :** 덱의 뒤쪽에서 요소를 제거합니다.
    - **'`rear`'** 위치가 바뀝니다.

# 3️⃣ 예제 코드.

아래는 **'Deque'** 의 **'LinkedList'** 구현을 사용하여 **'front'** 와 **'rear'** 의 변화를 보여주는 예제 코드입니다.

```java
import java.util.Deque;
import java.util.LinkedList;

public class DequeExample {
	public static void main(String[] args) {
		Deque<String> deque = new LinkedList<>();

		// 요소를 덱의 앞과 뒤에 추가
		deque.addFirst("A"); // front: A
		deque.addLast("B"); // rear: B
		deque.addFirst("C"); // front: C, rear: B
		deque.addLast("D"); // rear: D

		System.out.println("Initial Deque: " + deque); // 출력 : [C,A,B,D]

		// 앞쪽에서 요소 제거
		System.out.println("Removed from front: " + deque.removeFirst()); // 출력: C

		// 뒤쪽에서 요소 제거
		System.out.println("Removed from rear: " + deque.removeLast()); // 출력: D

		System.out.println("Deque after removals: " + deque); // 출력: [A, B]

		// 덱의 앞쪽과 뒤쪽에서 요소 확인
		System.out.println("Front element: " + deque.getFirst()); // 출력: A
		System.out.println("Rear element: " + deque.getLast()); // 출력: B
	}
}
```

## 👉 설명.

### 1️⃣ 삽입 연산.

- **'deque.addFirst("A")' :** "A"를 덱의 앞에 삽입합니다.

- **'deque.addLast("B")' :** "B"를 덱의 뒤에 삽입합니다.

- **'deque.addFirst("C")' :** "C"를 덱의 앞에 삽입합니다.

- **'deque.addLast("D")' :** "D"를 덱의 뒤에 삽입합니다.

> 이 연산들은 **'front'** 와 **'rear'** 의 위치를 업데이트합니다.

### 2️⃣ 삭제 연산.

- **'deque.removeFirst()' :** 덱의 앞쪽에서 "C"를 제거합니다.

- **'deque.removeLast()' :** 덱의 뒤쪽에서 "D"를 제거합니다.

> 이 연산들은 **'front'** 와 **'rear'** 의 위치를 다시 업데이트합니다.

### 3️⃣ 요소 확인.

- **'deque.getFirst()' :** 덱의 앞쪽 요소를 확인합니다.

- **'deque.getLast()' :** 덱의 뒤쪽 요소를 확인합니다.

> 이 예시 코드는 **'front'** 와 **'rear'** 가 데이터의 삽입 및 삭제 연산에 따라 어떻게 변하는지 잘 보여줍니다.
> **'Deque'** 는 이처럼 양쪽 끝에서의 삽입과 삭제 연산을 지원하므로, **'front'** 와 **'rear'** 의 위치는 동적입니다.
