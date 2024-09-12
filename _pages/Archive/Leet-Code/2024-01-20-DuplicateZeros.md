---
title: 🆙 [LeetCode] 1089.Duplicate Zeros.
date: 2024-01-20 10:28:00
modified: 2024-01-20 11:53:00
tags: [swift, algorithm, datastructure]
description: 배열 삽입.
---

# 🆙 [LeetCode] 1089.Duplicate Zeros

- Difficulty: Easy
- Topic: Array, Two Pointers

---

문제는 배열을 제자리에서 수정하도록 요구합니다.<br>
제자리 수정이 제약 조건이 아니라면, 원본 배열에서 대상 배열로 요소를 복사하는 방법을 사용했을 것입니다.<br>
<br>
<img src="https://github.com/devKobe24/images/blob/main/1089_Duplicate_Zeros_1.png?raw=true"><br>
<br>
<strong>0을 두 번 복사한 것을 주목하세요.</strong>
```swift
var s = 0
var d = 0

let N = source.count // 여기서 'source'는 Int 타입의 배열이라고 가정합니다.
var destination = [Int]()

// 목적지 배열이 가득 찰 때까지 복사가 수행됩니다.
while s < N {
    if source[s] == 0 {
        // 0을 두 번 복사합니다.
        destination.append(0)
        d += 1
        destination.append(0)
    } else {
        destination.append(source[s])
    }
    
    d += 1
    s += 1
}
```

문제 설명에는 새 배열을 확장하지 않고 원래 배열의 길이로만 자른다고도 언급되어 있습니다.<br>
이는 배열의 끝에서 몇몇 요소를 버려야 함을 의미합니다.<br>
이러한 요소들은 새로운 인덱스가 원래 배열의 길이를 넘어서는 요소들입니다.<br>
<br>
<img src="https://github.com/devKobe24/images/blob/main/1089_Duplicate_Zeros_2.png?raw=true">
<br>
우리에게 주어진 문제 제약 사항에 대해 다시 생각해 봅시다.<br>
추가 공간을 사용할 수 없기 때문에, 우리의 원본 배열과 대상 배열은 본질적으로 동일합니다.<br>
우리는 단순히 원본을 대상 배열로 그대로 복사할 수 없습니다.<br>
그렇게 하면 몇몇 요소를 잃어버릴 것입니다.<br>
왜냐하면, 우리는 배열을 덮어쓰게 될 것이기 때문입니다.<br>
<br>
<img src="https://github.com/devKobe24/images/blob/main/1089_Duplicate_Zeros_3.png?raw=true"><br>
이를 염두에 두고 아래 겁근 방식에서는 배열의 끝 부분에 복사를 시작합니다.<br>

## Approach 1: Two pass, O(1) space

### Intuition(직관)

만약 우리가 배열의 끝에서 버려질 요소의 수를 안다면, 나머지는 복사할 수 있습니다.<br>
우리는 어떻게 배열의 끝에서 버려질 요소의 수를 알아낼 수 있을까요?<br>
그 수는 배열에 추가될 여분의 0의 수와 같을 것입니다.<br>
여분의 0은 배열의 끝에서 요소 하나를 밀어내면서 자신을 위한 공간을 만듭니다.<br>
<br>
일단 우리가 원래 배열에서 최종 배열의 일부가 될 요소의 수를 알게 되면, 우리는 끝에서부터 복사하기 시작할 수 있습니다.<br>
끝에서부터 복사하는 것은, 마지막 몇 개의 불필요한 요소들을 덮어쓸 수 있기 때문에, 어떤 요소도 잃어버리지 않게 해줍니다.<br>

### Algorithm

1️⃣. 중복될 제로의 수를 찾습니다. 이를 `possible_dups`라고 합시다.<br>
최종 배열의 일부가 되지 않을 잘린 제로들을 세지 않도록 주의해야 합니다.<br>
버려진 제로들은 최종 배열의 일부가 되지 않기 때문입니다.<br>
`possible_dups`의 개수는 원래 배열에서 잘릴 요소의 수를 알려줄 것입니다.<br>
따라서 어느시점에서든, `length_ - possible_dups`는 최종 배열에 포함될 요소의 수입니다.<br>
<br>
<img src="https://github.com/devKobe24/images/blob/main/1089_Duplicate_Zeros_4.png?raw=true">
<br>
참고: 위의 다이어그램에서는 이해를 돕기 위해 원본 배열과 대상 배열을 보여줍니다<br>
우리는 이러한 연산들을 오직 하나의 배열에서만 수행할 것입니다.<br>
<br>

2️⃣. 남은 요소들의 경계에 있는 제로에 대한 에지 케이스를 처리합니다.<br>
<br>
이 문제의 에지 케이스에 대해 이야기해 봅시다.<br>
남은 배열에서 제로를 복제할 때는 특별한 주의가 필요합니다.<br>
이 주의는 경계에 놓인 `Zero`에 대해서 취해져야 합니다.<br>
왜냐하면, 이 제로는 가능한 중복으로 간주되거나, 그것의 복제를 수용할 공간이 없을 때 남은 부분에 포함될 수 있기 때문입니다.<br>
만약 그것이 `possible_dups`의 일부라면 우리는 그것을 복제하고 싶을 것이고, 그렇지 않다면 복제하지 않을 것입니다.<br>
<br>
<blockquote>
    에지 케이스의 예는 - [8,4,5,0,0,0,0,7] 입니다.<br>
    이 배열에서 첫 번째와 두 번째로 제로의 중복을 수용할 공간이 있습니다.<br>
    하지만 세번째 제로의 중복을 위한 충분한 공간이 없습니다.<br>
    <strong>따라서 복사할 때 세 번째 제로에 대해선 두 번 복사하지 않도록 주의해야 합니다.</strong><br>
    결과 = [8,4,5,0,`0`,0,`0`,0]
</blockquote><br>
<br>
3️⃣. 배열의 끝에서부터 순회하여, 0이 아닌 요소는 한 번, 0 요소는 두 번 복사합니다.<br>
우리가 불필요한 요소들을 버린다고 할 때, 이는 단순히 불필요한 요소들의 왼쪽에서 시작하여 새로운 값들로 그것들을 덮어쓰고, 결국 남은 요소들은 오른쪽으로 이동시켜 배열 안에 중복된 요소들을 위한 공간을 만들어낸다는 것을 의미합니다.<br>
<br>
<img src="https://github.com/devKobe24/images/blob/main/1089_Duplicate_Zeros_5.png?raw=true"><br>
<br>

```swift
class Solution {
    func duplicateZeros(_ arr: inout [Int]) {
        var possibleDups = 0
        let length_ = arr.count - 1
        // 복제할 0의 개수를 찾습니다.
        // 원래 배열의 마지막 요소를 넘어서면 중지합니다.
        // 수정된 배열의 일부가 될 마지막 요소를 넘어서면 중지합니다.
        for left in 0...(length_ - possibleDups) {
            // 0 숫자 세기
            if arr[left] == 0 {
                // Edge case: 이 0은 복제할 수 없습니다. 더 이상 공간이 없습니다.
                // 왼쪽은 포함될 수 있는 마지막 요소를 가리키고 있습니다.
                if left == length_ - possibleDups {
                    // 이 0의 경우 중복 없이 복사합니다.
                    arr[length_] = 0
                    break
                }
                possibleDups += 1
            }
        }
        
        // 새 배열의 일부가 될 마지막 요소부터 거꾸로 시작합니다.
        var last = length_ - possibleDups
        
        // 0을 두 번 복사하고 0이 아닌 것을 한 번 복사합니다.
        while last >= 0 {
            if arr[last] == 0 {
                arr[last + possibleDups] = 0
                possibleDups -= 1
                arr[last + possibleDups] = 0
            } else {
                arr[last + possibleDups] = arr[last]
            }
            last -= 1
        }
    }
}
```

### Complexity Analysis
- 시간 복잡도(Time Complexity): `O(N)`, 여기서 `N`은 배열의 요소 수입니다. 우리는 배열을 두 번 순회하는데, 하나는 `possible_dups`의 수를 찾기 위해, 다른 하나는 요소들을 복사하기 위해 사용됩니다. 최악의 경우, 배열에 zero가 적거나 없을 때 배열 전체를 순회할 수도 있습니다.
- 공간 복잡도(Space Complexity): `O(1)`, 우리는 추가적인 공간을 사용하지 않습니다.
