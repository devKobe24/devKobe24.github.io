---
title: 🆙 [LeetCode] 88.Merge Sorted Array.
date: 2024-01-21 12:23:00
modified: 2024-01-22 06:58:00
tags: [swift, algorithm, datastructure]
description: Merge Sorted Array.
---

# 🆙 [LeetCode] 88.Merge Sorted Array.

- Difficulty: Easy
- Topic: Array, Two Pointer, Sorting

---

## Approach 1: Merge and sort

### Intuition(직관)

순진한 접근 방식은 `nums2`의 값을 그저 `nums1`의 끝에 쓰고, 그다음 `nums1`을 정렬하는 것입니다.<br>
우리는 값을 반환할 필요가 없으며, `nums1`을 직접 수정해야 합니다.<br>
이 방법은 코딩하기는 쉽지만, 이미 정렬된 상태를 활용하지 않기 때문에 높은 시간 복잡도를 가집니다.<br>

### Implementation(구현)

```swift
class Solution {
    func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
        for i in 0..<n {
            nums1[i + m] = nums2[i]
        }
        nums1.sort()
    }
}
```

- Time complexity(시간 복잡도): `O((n + m) log(n+m))`
    - 내장된 정렬 알고리즘을 사용하여 길이가 `x`인 리스트를 정렬하는 비용은 `O(xlogx)`입니다. 이 경우에는 길이가 `m+n`인 리스트를 정렬하므로 총 시간 복잡도는 `O((n + m) log(n + m))`가 됩니다.

- Space complexity(공간 복잡도): `O(n)`, 하지만 상황에 따라 다를 수 있습니다.
    - 대부분의 프로그래밍 언어는 `O(n)` 공간을 사용하는 내장 정렬 알고리즘을 가지고 있습니다.

---

## Approach 2: Three Pointers (Start From the Beginning)

### Intuition(직관)

각 배열이 이미 정렬되어 있기 때문에, Two pointer 기법을 활용하면 `O(n+m)`의 시간 복잡도를 달성할 수 있습니다.<br>

### Algorithm

`nums1`의 값을 복사하여 `nums1Copy`라는 새 배열을 만드는 것이 가장 간단한 구현 방법입니다.<br>
그런 다음 두 개의 **읽기(read)** 포인터와 하나의 **쓰기(write)** 포인터를 사용하여 `nums1Copy`와 `nums2`에서 값을 읽고 `nums1`에 씁니다.<br>
<br>
- `nums1Copy`를 `nums1`의 처음 `m` 값이 포함된 새 배열로 초기화합니다.
- 읽기 포인터 `p1`을 `nums1Copy`의 시작 부분에 초기화합니다.
- 읽기 포인터 `p2`를 `nums2`의 시작 부분에 초기화합니다.
- 쓰기 포인터 `p`를 `nums1`의 시작 부분에 초기화합니다.
- p가 여전히 `nums1` 내에 있는 동안:
    - `nums1Copy[p1]`이 존재하고 `nums2[p2]` 보다 작거나 같으면:
        - `nums1Copy[p1]`을 `nums1[p]`에 쓰고 `p1`을 `1` 증가시킵니다.
    - 그렇지 않으면
        - `nums2[p2]`를 `nums1[p]`에 쓰고 `p2`를 `1` 증가시킵니다.
    - `p`를 `1` 증가시킵니다.

<img src="https://github.com/devKobe24/images/blob/main/88_beginning.png?raw=true"><br>
<br>

```swift
class Solution {
    func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
        // nums1의 처음 m개 원소의 복사본을 만듭니다.
        let nums1Copy = Array(nums1[0..<m])

        // nums1Copy와 nums2에 대한 읽기 포인터입니다.
        var p1 = 0
        var p2 = 0

        // nums1Copy와 nums2에서 원소를 비교하여 더 작은 것을 nums1에 씁니다.
        for p in 0..<(m + n) {
            // p1과 p2가 각각의 배열 범위를 벗어나지 않도록 확인합니다.
            if p2 >= n || (p1 < m && nums1Copy[p1] < nums2[p2]) {
                nums1[p] = nums1Copy[p1]
                p1 += 1
            } else {
                nums1[p] = nums2[p2]
                p2 += 1
            }
        }
    }
}
```

### Complexity Analysis

- Time complexity(시간 복잡도) : `O(n+m)`
    - 우리는 `n+2*m` 번의 읽기와 `n+2*m` 번의 쓰기를 수행하고 있습니다. Big O 표기법에서 상수는 무시되므로, 이는 `O(n+m)`의 시간 복잡도를 의미합니다.

- Space complexity(공간 복잡도) : `O(m)`
    - 우리는 추가적으로 길이가 `m`인 배열을 할당하고 있습니다.

---

## Approach 3: Three Pointers (Start From the End)

### Intuition

<blockquote>
    <p align="left">
        인터뷰 팁: 이것은 쉬운 문제에 대한 중간 수준의 솔루션입니다.<br>
        쉬운 수준의 문제 중 상당수는 더 어려운 해결책을 갖고 있으며,<br>
        좋은 지원자는 이를 찾을것으로 예상됩니다.
    </p>
</blockquote><br>
<br>

Approach 2는 이미 최상의 시간 복잡도인 `O(n+m)`을 보여주지만, 여전히 추가 공간을 사용합니다.<br>
이는 `nums1` 배열의 요소들을 어딘가에 저장해야 하기 때문에, 그것들이 덮어쓰여지지 않도록 해야하기 때문입니다<br>
<br>
그렇다면 대신 `nums1`의 끝부터 덮어쓰기 시작하면 어떨까요? 거기에는 아직 정보가 없으니까요.<br>
<br>
알고리즘은 이전과 유사하지만, 이번에는 `p1`을 `nums1`의 `m - 1` 인덱스에, `p2`를 `nums2`의 `n - 1` 인덱스에, 그리고 `p`를 `nums1`의 `m + n - 1` 인덱스에 두는 방식입니다.<br>
이 방식으로, `nums1`의 처음 `m` 값들을 덮어쓰기 시작할 때, 이미 각각을 새 위치에 써 놓았을 것이라는 것이 보장됩니다.<br>
이런 방식으로, 추가 공간을 없앨 수 있습니다.<br>
<br>

<blockquote>
    <p align="left">
        인터뷰 팁: 베열 문제를 제자리에서 해결하려고 할 때는 항상 배열을 앞에서 뒤로 순회하는 대신 뒤에서 앞으로 순회하는 가능성을 고려해보세요<br>
        이것은 문제를 완전히 바꾸어 놓고, 훨씩 쉽게 만들 수 있습니다.
    </p>
</blockquote><br>
<br>

<img src="https://github.com/devKobe24/images/blob/main/88_end.png?raw=true"><br>
<br>

### Implementation

1️⃣<img src="https://github.com/devKobe24/images/blob/main/88_1.png?raw=true"><br>
2️⃣<img src="https://github.com/devKobe24/images/blob/main/88_2.png?raw=true"><br>
3️⃣<img src="https://github.com/devKobe24/images/blob/main/88_3.png?raw=true"><br>
4️⃣<img src="https://github.com/devKobe24/images/blob/main/88_4.png?raw=true"><br>
5️⃣<img src="https://github.com/devKobe24/images/blob/main/88_5.png?raw=true"><br>
6️⃣<img src="https://github.com/devKobe24/images/blob/main/88_6.png?raw=true"><br>

```swift
class Solution {
    func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
        // 각 배열의 끝을 가리키는 p1과 p2를 설정합니다.
        var p1 = m - 1
        var p2 = n - 1

        // p를 배열을 통해 뒤로 이동하면서, 매번 p1 또는 p2가 가리키는 더 작은 값을 작성합니다.
        for p in stride(from: m + n - 1, through: 0, by: -1) {
            if p2 < 0 {
                break
            }
            if p1 >= 0 && nums1[p1] > nums2[p2] {
                nums1[p] = nums1[p1]
                p1 -= 1
            } else {
                nums1[p] = nums2[p2]
                p2 -= 1
            }
        }
    }
}
```

### Complexity Analysis

- Time complexity: `O(n + m)`
    - Same as Approach 2.
- Space complexity: `O(1)`
    - Unlike Approach 2, we're not using an extra array.

### Proof(optional)

이 주장에 대해 조금 회의적일 수도 있습니다.<br>
정말 모든 경우에 작동하나요?<br>
이렇게 대담한 주장을 하는 것이 안전한가요?<br>
<br>

<blockquote>
    <p align="left">
        이 방식으로, `nums1`의 처음 `m`개 값을 덮어쓰기 시작하면, 각각을 이미 새 위치에 써 놓았을 것입니다<br>
        이런 방식으로 우리는 추가 공간을 없앨 수 있습니다.
    </p>
</blockquote><br>
<br>

훌륭한 질문입니다!<br>
그렇다면 왜 이 방법이 작동할까요?<br>
이를 증명하기 위해, `p`가 `nums1`에서 `p1`이 아직 읽지 않은 값을 덮어쓰지 않는 것을 확실히 해야 합니다.<br>
<br>

<blockquote>
    <p align="left">
        <strong>조언 :</strong>증명에 겁을 먹고 있나요?<br>
        많은 소프트웨어 엔지니어들이 그렇습니다.<br>
        좋은 증명은 간단히 각각의 논리적 주장들이 다음 주장 위에 구축되는 것입니다.<br>
        이런 방식으로, 우리는 <strong>"명백한"</strong> 진술로부터 시작하여 증명하고자 하는 것에 이룰 수 있습니다.<br>
        각 진술을 하나씩 읽으며, 다음으로 넘어가기 전에 각각을 이해하는 것이 중요합니다.
    </p>
</blockquote>

1. 초기화 시 `p`는 `p1`보다 `n`만큼 앞서 있다는 것을 알 수 있습니다.(다른 말로, `p1 + n = p` 입니다.)
2. 또한, 이 알고리즘이 수행하는 `p`의 반복 동안, `p`는 항상 `1` 만큼 감소하고, `p1` 또는 `p2` 중 하나가 `1` 만큼 감소한다는 것도 알고 있습니다.
3. `p1`이 감소할 때, `p`와 `p1` 사이의 간격은 동일하게 유지되므로, 그 경우에 "추월(overtake)"이 발생할 수 없다는 것을 추론할 수 있습니다.
4. 하지만 `p2`가 감소할 때는, `p`는 움직이지만 `p1`은 그렇지 않으므로, `p`와 `p1` 사이의 간격이 `1`만큼 줄어든가는 것을 추론할 수 있습니다.
5. 그리고 이로부터, `p2`가 감소할 수 있는 최대 횟수는 `n`번임을 추론할 수 있습니다. 다시 말해, `p`와 `p1` 사이의 간격은 최대 `n` 만큼 `1` 씩 줄어들 수 있습니다.
6. 결론적으로, 그들이 처음에 `n`만큼 떨어져 있었기 때문에 추월이 일어날 수 없습니다. 그리고 `p = p1`일 때, 간격은 `n` 번 줄어들어야 합니다. 이는 `nums2`의 모든 것이 병합되었으므로 더 이상 할 일이 없음을 의미합니다.
