---
title: 📝 배열 삽입 2(배열의 시작 부분에 삽입하기 - Inserting at the Start of an Array)
date: 2024-01-19 13:28:00
modified: 2024-01-19 13:53:00
tags: [swift, algorithm, datastructure]
description: 배열의 삽입, 시작 부분에 삽입하기.
---

### 배열 삽입 시리즈

<a href="https://www.devkobe24.com/InsertingAtTheEndOfAnArray/">배열 삽입1 (배열의 끝에 삽입하기-Inserting at the End of an Array)</a>

---

# 배열의 시작 부분에 삽입하기(Inserting at the Start of an Array)

배열의 시작 부분에 요소를 삽입하려면, 새 요소를 위한 공간을 만들기 위해 배열의 다른 모든 요소들을 오른쪽으로 하나의 인덱스만큼 이동시켜야 합니다.<br>
이것은 비용이 매우 많이 드는 작업입니다, 왜냐하면 기존의 요소들을 모두 오른쪽으로 한 단계씩 이동시켜야 하기 때문입니다.<br>
모든 것을 이동시켜야 한다는 것은 이 작업이 상수 시간 작업이 아니라는 것을 의미합니다.<br>
사실, 배열의 시작 부분에 삽입하는 데 걸리는 시간은 배열의 길이에 비례할 것입니다.<br>
시간 복잡도 분석 측면에서 이는 선형 시간 복잡도, 즉 `O(N)`인데, 여기서 `N`은 배열의 길이입니다.<br>
<br>
<img src="https://github.com/devKobe24/images/blob/main/Array_Insertion_2.png?raw=true">
<br>
다음은 코드의 모습입니다.<br>
```swift
// 배열삽입 1 코드 참고
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

// 먼저, 새로운 요소를 위한 공간을 만들어야 합니다.
// 이를 위해 각 요소를 오른쪽으로 하나의 인덱스만큼 이동시킵니다.
// 이것은 먼저 인덱스 3의 요소를 이동시키고, 그 다음 2, 그 다음 1, 마지막으로 0을 이동시킵니다.
// 어떤 요소도 덮어쓰지 않기 위해 뒤에서부터 진행해야 합니다.
for i in(0...3).reversed() {
    intArray[i + 1] = intArray[i]
}

// 이제 새로운 요소를 위한 공간을 만들었으므로,
// 시작 부분에 삽입할 수 있습니다.
intArray[0] = 20

printArray()
```

다음은 `printArray()`를 실행한 결과입니다.

```swift
Index 0 contains 20.
Index 1 contains 0.
Index 2 contains 1.
Index 3 contains 2.
Index 4 contains 10.
Index 5 contains 0.
```
