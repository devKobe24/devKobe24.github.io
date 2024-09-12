---
title: 📝 배열의 용량 vs 배열의 길이
date: 2024-01-18 05:38:00
modified: 2024-01-18 05:50:00
tags: [algorithm, datastructure]
description: 배열의 용량과 배열의 길이의 차이.
---

# Array Capacity VS Length

`만약 누군가가 당신에게 DVD 배열의 길이가 얼마나 되는지 물어본다면, 당신의 대답은 무엇일까요?`<br>
<br>
당신은 두 가지 다른 대답을 할 수 있습니다.<br>
<br>
1. 상자가 가득 차있을 경우, 상자가 담을 수 있는 DVD의 수, 또는
2. 현재 상자에 들어있는 DVD의 수.
<br>
이 두 답변은 모두 정확하며, 매우 다른 의미를 가집니다!<br>
이 둘의 차이를 이해하고 올바르게 사용하는 것이 중요합니다.<br>
우리는 첫 번째를 배열의 <strong>'용량'</strong>이라고 부르고, 두번째를 <strong>'길이'</strong>라고 부릅니다.<br>

## Array Capacity

```java
DVD[] array = new DVD[6]
```

`array[6]`에 요소를 삽입하는 것이 유효한 작업일까요?<br>
`array[10]`은 어떨까요?<br>
<br>
아니요, 이 두 경우 모두 유효하지 않습니다.<br>
배열을 생성할 때, 이 배열이 최대 `6` 개의 DVD를 담을 수 있다고 지정했습니다.<br>
이것이 배열의 용량입니다.<br>
<br>
인덱싱이 `0`부터 시작한다는 것을 기억한다면, 우리는 오직 `array[0]`, `array[1]`, `array[2]`, `array[3]`, `array[4]` 그리고 `array[5]`에만 항목을 삽입할 수 있습니다.<br>
`array[-3]`, `array[6]`, `array[100]`과 같이 다른 곳에 요소를 넣으려고 하면 `ArrayIndexOutOfBoundsExecption`으로 코드가 충돌하게 됩니다.<br>
<br>
배열의 용량은 배열이 생성될 때 결정되어야 합니다.<br>
용량은 나중에 변경할 수 없습니다.<br>
우리가 사용한 종이 상자에 DVD를 넣는 비유로 돌아가 보면, 배열의 용량을 변경하는 것은 종이 상자를 더 크게 만들려는 것과 같습니다.<br>
고정된 크기의 종이 상자를 더 크게 만드는 것은 비현실적이며, 컴퓨터의 배열에서도 마찬가지입니다!<br>
<br>
그렇다면 7번째 DVD를 얻었을 때, 모든 DVD를 같은 배열에 넣고 싶다면 어떻게 할까요?<br>
불행히도 종이 상자의 경우와 마찬가지입니다.<br>
더 큰 상자를 새로 구해서, 기존의 DVD들과 새로운 것을 모두 옮겨야 합니다<br>
<br>
자바에서 배열의 **용량**은 배열의 `length` 속성값을 확인함으로써 알 수 있습니다.<br>
이는 `arr.length`라는 코드를 사용하여 확인되는데, 여기서 `arr`은 배열의 이름입니다.<br>
다른 프로그래밍 언어들은 배열의 길이를 확인하는 데 다른 방법을 사용합니다.

```java
int capacity = array.length;
System.out.println("The Array has a capacity of " + capacity);
```

이 코드를 실행하면 다음과 같은 출력이 나옵니다:

```java
The Array has a capacity of 6
```

## capacity property of Swift

### Instance Property
#### capacity
**배열이 새로운 저장 공간을 할당하지 않고 담을 수 있는 요소의 총 수입니다.**<br>

모든 배열은 그 내용을 저장하기 위해 특정 양의 메모리를 예약합니다.<br>
배열에 요소를 추가하고 그 배열이 예약된 용량을 초과하기 시작하면, 배열은 더 큰 메모리 영역을 할당하고 그 요소들을 새로운 저장 공간으로 복사합니다.<br>
새로운 저장 공간은 기존 저장 공간 크기의 배수입니다.<br>
이 지수적 성장 전략은 요소 추가 작업이 평균적으로 상수 시간 내에 이루어지게 하여, 많은 추가 작업의 성능을 평균화합니다.<br>
재할당을 유발하는 추가 작업에는 성능 비용이 들지만, 배열이 커짐에 따라 그런 작업은 점점 덜 자주 발생합니다.<br>
<br>
다음 예시는 배열 리터럴로부터 정수 배열을 생성한 다음, 다른 컬렉션의 요소들을 추가합니다.<br>
추가 하기 전에, 배열은 결과 요소들을 저장할 수 있을 만큼 충분히 큰 새로운 저장 공간을 할당합니다.<br>

```swift
var numbers = [10, 20, 30, 40, 50]
// numbers.count == 5
// numbers.capacity == 5

numbers.append(contentsOf: stride(from: 60, through: 100, by: 10))
// numbers.count == 10
// numbers.capacity == 10
```

## Array Length

**길이(length)** 의 또 다른 정의는 배열에 현재 들어 있는 DVD의 수, 또는 다른 항목들의 수입니다<br>
이것은 직접 추적해야 할 것이며, 기존 DVD를 덮어쓰거나 배열에 공백을 남겨두어도 오류는 발생하지 않습니다.<br>
<br>
이전 예제에서 `length` 변수를 사용하여 다음 비어 있는 인덱스를 추적하고 있는 것을 눈치챘을 수 있습니다.<br>

```java
// 용량이 6인 새 배열을 생성합니다.
int[] array = new int[6];

// 현재 길이는 0이며, 요소가 0개 있기 때문입니다.
int length = 0;

// 그 안에 3개의 항목을 추가합니다.
for (int i = 0; i < 3; i++) {
    array[i] = i * i;
    // 요소를 추가할 때마다 길이가 1씩 증가합니다.
    length++;
}

System.out.println("배열의 용량은 " + array.length + "입니다.");
System.out.println("배열의 길이는 " + length + "입니다.");
```

이 코드를 실행하면 다음과 같은 출력이 나옵니다:

```java
The Array has a capacity of 6
The Array has a length of 3
```

## count property of Swift

### Instance Property
#### count

**배열의 요소(elements) 수 입니다.**

```swift
var count: Int { get }
```

## Handling Array Parameters(배열 매개변수 처리하기)

LeetCode에서의 대부분의 배열 문제는 "길이"나 "용량" 매개변수 없이 매개변수로 배열을 전달합니다.<br>
이게 무슨 뜻일까요?<br>
예를 들어 설명해 보겠습니다.<br>
여기 당신이 풀게 될 첫 번째 문제의 설명이 있습니다.<br>

**'이진 배열이 주어졌을 때, 이 배열에서 연속된 1의 최대 개수를 찾아라.'**

그리고 여기 주어진 코드 템플릿이 있습니다.

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        
    }
}
```

유일한 매개변수는 `'nums'` 인데, 이는 배열입니다. `'nums'`의 길이를 모르면 이 문제를 해결할 수 없습니다.<br>
다행히도 이는 간단합니다.<br>
매개변수로 주어진 배열에 대한 추가 정보가 없을때는 **길이 == 용량 (length == capacity)** 이라고 안전하게 가정할 수 있습니다.<br>
즉, 배열은 그 데이터를 모두 담기에 정확히 적합한 크기입니다.<br>
따라서 `.length`를 사용할 수 있습니다.<br>
<br>
하지만 조심하세요, 배열은 0부터 시작하는 인덱스입니다.<br>
용량(capacity)/길이(length)는 항목의 수이지 최고 인덱스가 아닙니다.<br>
최고 인텍스는 `.lenght -1` 입니다.<br>
따라서 배열의 모든 항목을 순회하기 위해 다음과 같이 할 수 있습니다.<br>

```java
class Solution {
    public int findMaxConsectiveOnes(int[] nums) {
        // 힌트: 여기에 변수를 초기화하고 선언하여
        // 연속된 1이 몇 개인지 추적합니다.
        for (int i = 0; i < nums.length; i++) {
            // nums[i] 요소로 무언가를 합니다.
        }
    }
}
```

이것이 바로 시작하기 위해 필요한 배열의 기본 사항입니다!
