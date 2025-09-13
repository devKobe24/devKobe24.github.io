---
title: 📝 배열 삽입 1(배열의 끝에 삽입하기-Inserting at the End of an Array)
date: 2024-01-19 11:50:00
modified: 2024-01-19 13:14:00
tags: [algorithm, datastructure]
description: 배열 삽입.
---

배열에 새로운 요소를 삽입하는 것은 여러 형태를 가질 수 있습니다:<br>
<br>
1. 배열의 끝에 새 요소를 삽입하기.
2. 배열의 시작 부분에 새 요소를 삽입하기.
3. 배열 내의 주어진 인덱스에 새 요소를 삽입하기.

## 배열의 끝에 삽입하기(Inserting at the End of an Array).

어느 시점에서든, 우리는 `length` 변수에 추적해둔 배열의 마지막 요소의 인덱스를 알고 있습니다.<br>
끝에 요소를 삽입하기 위해 해야 할 일은 현재 마지막 요소 다음 인덱스에 새 요소를 할당하는 것뿐입니다.<br>
<br>
<img src="https://github.com/devKobe24/images/blob/main/Array_Insertion_1.png?raw=true">
<br>
이것은 우리가 이미 본 것과 거의 같습니다.<br>
최대 `6`개의 항목을 담을 수 있는 새 배열을 만들고, 그 다음 첫 `3`개의 인덱스에 항목을 추가하는 코드입니다.<br>

```swift
// 6개의 요소를 가진 정수 배열 선언
var intArray = [Int](repeating: 0, count: 6)
var length = 0

// 배열에 3개의 요소 추가
for i in 0..<3 {
    intArray[length] = i
    length += 1
}
```

우리가 무슨 일이 일어나고 있는지 시각화하는 데 도움이 되는 함수, `printArray`를 정의해봅시다.<br>

```swift
for i in 0..<intArray.count {
    print("Index \(i) contains \(intArray[i])")
}
```

우리가 `printArray` 함수를 실행하면 다음과 같은 출력을 얻을 수 있습니다.<br>

```swift
Index 0 contains 0.
Index 1 contains 1.
Index 2 contains 2.
Index 3 contains 0.
Index 4 contains 0.
Index 5 contains 0.
```

인덱스 `3`, `4`, `5`가 모두 `0`을 포함하고 있는 것을 보셨나요?<br>
이것은 Swift가 사용하지 않는 `int` 배열 슬롯을 `0`으로 채우기 때문입니다.<br>
<br>
이제 4번째 요소를 추가해봅시다. 숫자 10을 추가할 것입니다.<br>

```swift
// 배열 끝에 새 요소를 삽입. 다시 한번,
// 새 요소를 삽입하기 위해 배열에 충분한 공간이 있는지 확인하는 것이 중요합니다.
intArray[length] = 10
length += 1
```

길이를 1 증가시킨 이유를 알아채셨나요?<br>
길이를 1 증가시키는 것이 중요합니다.<br>
이 단계를 건너 뛰면 다음에 다른 요소를 추가할 때 방금 추가한 요소를 실수로 덮어쓰게 됩니다!<br>
`printArray`를 다시 실행하면 다음과 같은 결과를 얻을 수 있습니다.<br>

```swift
Index 0 contains 0.
Index 1 contains 1.
Index 2 contains 2.
Index 3 contains 10.
Index 4 contains 0.
Index 5 contains 0.
```
