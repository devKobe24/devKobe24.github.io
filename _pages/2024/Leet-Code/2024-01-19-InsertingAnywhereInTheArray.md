---
title: 📝 배열 삽입 3(배열의 아무 곳에나 삽입하기 - Inserting Anywhere in the Array)
date: 2024-01-19 14:05:00
modified: 2024-01-19 14:29:00
tags: [swift, algorithm, datastructure]
description: 배열 삽입의 아무 곳에나 삽입하기.
---
### 배열 삽입 시리즈

<a href="https://www.devkobe24.com/InsertingAtTheEndOfAnArray/">
    배열 삽입1 (배열의 끝에 삽입하기-Inserting at the End of an Array)
</a><br>
<a href="https://www.devkobe24.com/InsertingAtTheStartOfAnArray/">
    배열 삽입2 (배열의 시작 부분에 삽입하기 - Inserting at the Start of an Array)
</a>

---

마찬가지로, 주어진 인덱스에 삽입하기 위해서는, 해당 인덱스부터 시작하는 모든 요소들을 오른쪽으로 한 자리씩 이동시켜야 합니다.<br>
새 요소를 위한 공간이 생성되면, 삽입을 진행합니다.<br>
생각해보면, 시작 부분에 삽입하는 것은 사실 주어진 인덱스에 요소를 삽입하는 것의 특별한 경우에 해당합니다.<br>
그 경우에 주어진 인덱스는 `0`이었습니다.<br>
<br>
<img src="https://github.com/devKobe24/images/blob/main/Array_Insertion_3.png?raw=true">
<br>
다시 한 번 말씀드리지만, 이것도 비용이 많이 드는 작업입니다.<br>
새 요소를 실제로 삽입하기 전에 거의 모든 다른 요소들을 오른쪽으로 이동시켜야 할 수도 있기 때문입니다.<br>
위에서 보셨듯이, 많은 요소들을 오른쪽으로 한 칸씩 이동시키는 것은 삽입 작업의 시간 복잡도를 증가시킵니다.<br>
<br>
다음은 코드의 모습입니다<br>
```swift
// 배열 삽입 1,2 코드 참고
var intArray = [Int](repeating: 0, count: 6)
var length = 0


for i in 0..<3 {
	intArray[length] = i
	length += 1
}

func printArray() {
	for i in 0..<intArray.count {
			print("Index \(i) contains \(intArray[i])")
	}
}

intArray[length] = 10
length += 1


for i in(0...3).reversed() {
	intArray[i + 1] = intArray[i]
}

intArray[0] = 20

// 인덱스 2에 요소를 삽입하고 싶다고 가정해봅시다.
// 먼저, 새로운 요소를 위한 공간을 만들어야 합니다.
for i in stride(from: 4, through: 2, by: -1) {
	// 각 요소를 오른쪽으로 한 위치씩 이동시킵니다.
	intArray[i + 1] = intArray[i]
}

// 이제 새로운 요소를 위한 공간을 만들었으므로,
// 필요한 인덱스에 삽입할 수 있습니다.
intArray[2] = 30

printArray()
```

다음은 `printArray`를 실행한 결과입니다.

```swift
Index 0 contains 20.
Index 1 contains 0.
Index 2 contains 30.
Index 3 contains 1.
Index 4 contains 2.
Index 5 contains 10.
```

주의해야 할 주요한 것은 `array.capacity`가 베열의 전체 용량을 제공한다는 점을 기억하는 것입니다.
마지막으로 사용된 슬롯을 알고 싶다면 `count` 변수를 사용하여 직접 추적해야합니다.
