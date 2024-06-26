---
title: "📦[DataStructure] 문제 정의와 선형 스캔"
tags:
    - DataStructure
date: "2024-04-23"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 문제 정의.

새로운 알고리즘을 정의하기 전에, 항상 그 알고리즘이 해결하려는 문제를 정의해야 합니다.

여기서는 리스트에서 주어진 목푯값과 일치하는 원소를 하나 찾을 수 있는 효율적인 알고리즘을 만들려고 합니다.

이 탐색 문제를 형식적으로 정의하면 다음과 같습니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%90%E1%85%A1%E1%86%B7%E1%84%89%E1%85%A2%E1%86%A8%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6%E1%84%85%E1%85%B3%E1%86%AF%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%B5%E1%86%A8%E1%84%8C%E1%85%A5%E1%86%A8%E1%84%8B%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%B4%E1%84%92%E1%85%A1%E1%86%AB%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B7.png?raw=true">

일상생활에서는 이러한 작업을 **'어떤 것을 찾아줘'** 라고 표현합니다.
- 이 탐색 문제는 우리가 하루에도 몇 번씩 직면하는 문제입니다.
    - 사전에서 단어를 찾거나 연락처 목록에서 이름을 찾거나 역사적 사건 목록에서 특정 날짜를 찾거나 상품으로 꽉 찬 슈퍼마켓 선반에서 좋아하는 커피 브랜드를 찾는 경우 등이 있습니다.
        - 우리에게는 대상 목록과 목푯값의 일치 여부를 확인할 수 있는 방법이 필요합니다.

# 선형 스캔
이진 참색의 이점을 이해하기 위해 비교 대상을 제공하겠습니다.

더 간단한 알고리즘은 **선형 스캔(linear scan)** 부터 살펴봅시다.
- 선형 스캔은 리스트에서 한 번에 하나씩 값을 목푯값을 찾거나 목록의 끝에 도달할 때까지 비교해 목푯값을 찾습니다.

수로 이뤄진 배열 A에서 목푯값을 찾으려 한다고 가정해봅시다.
- 이 경우 target = 21을 사용합니다.
    - 배열의 각 상자 안에 든 값이 21과 같은지 반복해서 확인합니다.
        - 이 과정이 아래의 그림에 묘사되어 있습니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%89%E1%85%AE%E1%84%87%E1%85%A2%E1%84%8B%E1%85%A7%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%E1%84%8B%E1%85%B4%E1%84%89%E1%85%A5%E1%86%AB%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%AB.png?raw=true">

```
LinearScan(Array: A, Integer: target):
    Integer: i = 0
    WHILE i < length(A):
        IF A[i] == target:
            return i
                i = i + 1
            return -1
````

위 코드는 선형 스캔 코드를 보여줍니다.
- 이 코드는 일치하는 원소의 인덱스를 반환하고 탐색에 실패하면 원소가 없으므로 -1을 인덱스로 반환합니다.
    - 단일 WHILE 루프는 배열의 각 원소를 반복하고, 내부 IF 문은 인덱스에 해당하는 원소를 목푯값과 비교합니다.
        - 목푯값을 찾은 경우 즉시 해당 인덱스를 반환합니다.
            - 배열의 끝까지 확인한 경우 -1을 반환합니다.

선형 스캔은 멋지거나 똑똑하지 않습니다.

목표가 데이터에 있는지 찾기 위해 가능한 항목을 모두 확인하기 때문에 '무식한 검사'입니다.
- 특히 원소가 아주 많은 리스트에서 비효율적입니다.

A의 자료 구조에 대해 아무것도 모르면 프로세스를 최적화할 수 있는 방법이 없습니다.
- 목푯값이 모든 상자에 있을 수 있으므로 모든 상자를 확인해야 할 수도 있습니다.

선형 스캔의 한계를 보여주기 위해, 교실 바깥에 줄 서 있는 기초 프로그래밍 과목을 듣는 학생들 같은 물리적인 시퀀스에서 이러한 탐색을 수행한다고 상상해보자.
- 특정 학생의 숙제를 반환하려는 교사는 각 학생에게 "이름이 제레미입니까?" 라고 묻고 다음 학생으로 이동할 수 있습니다.
    - 교사가 올바른 학생을 찾거나 줄 끝까지 이동하면 탐색이 중지됩니다.
        - 학생들은 교사가 비효율적이라고 생각하면서 궁시렁될 것 입니다.

때로 선형 탐색에서 각각의 비교를 더 빨리 할 수 있는 방법이 있는 경우가 가끔 있습니다.
- 예를 들어, 복잡한 데이터가 문자열일 때는 앞서 올린 포스팅에서 설명한 것처럼 최초로 일치하지 않는 글자에서 비교를 멈춤으로써 비교에 걸리는 시간을 최적화할 수 있습니다.
    - 그러나 이런 최적화에도 한계가 있습니다.
        - 여전히 모든 원소를 하나씩 확인해야 하기 때문입니다.
