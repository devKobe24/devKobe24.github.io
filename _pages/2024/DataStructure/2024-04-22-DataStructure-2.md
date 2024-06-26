---
title: "📦[DataStructure] 문자열"
tags:
    - DataStructure
date: "2024-04-22"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

**문자열(String)** 은 종종 특수한 종류의 배열로 생각할 수 있는, 순서가 지정된 문자의 리스트다.

문자열의 각 칸에는 문자, 숫자, 기호, 공뱁 또는 제한된 특수 기호 중 하나가 포함됩니다.
- 마지막 칸에 있는 특수 기호 `/`는 종종 문자열의 끝을 나타냅니다.

인덱스를 사용해 문자열의 문자에 직접 접근할 수 있습니다.

<img src = "https://github.com/devKobe24/images/blob/main/HELLOWORLD!%E1%84%85%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%A7%E1%86%AF%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B7.png?raw=true">

일부 프로그래밍 언어에서는 문자열을 그냥 문자 배열로 직접 구현합니다.

몇몇 다른 언어에서는 문자열이 객체일 수 있으며, 문자열 클래스는 문자를 담고 있는 배열이나 다른 자료 구조를 감싼 래퍼(wrapper) 클래스 역할을 합니다.
- 문자열 래퍼 크래스는 문자열의 크기를 동적으로 조정하거나 부분 문자열을 탐색하는 등 추가 기능을 제공합니다.
    - 두 경우 모두 일반 배열과 유사한 구조가 문장열에 대한 작업에 어떤 영향을 미칠지 생각해보는 것이 유용합니다.

컴퓨터 화면에 문자열을 표시할 때는 문자열의 각 문자를 반복하면서 하나씩 문자를 표시합니다.

동등성(equality) 검사는 더 흥미롭습니다.
- 한 번의 연산으로 직접 비교할 수 있는 정수와 달리, 문자열은 각 문자를 반복하면서 비교해야 합니다.
    - 두 문자열을 비교할 때는 서로 일치하지 않는 문자를 발견할 때까지 두 문자열에서 같은 위치에 존재하는 문자를 서로 비교합니다.

아래의 코드는 두 문자열의 동등성을 확인하는 알고리즘을 보여줍니다.
```
StringEqual(String: str1, String: str2):
    IF length(str1) != length(str2):
        return False
    Integer: N = length(str1)
    Integer: i = 0
    WHILE i < N AND str1[i] == str2[i]:
        i = i + 1
    return i == N
```

- 알고리즘은 먼저 문자열의 크기를 비교합니다.
    - 길이가 다르면 알고리즘은 해당 시점에 중지됩니다.
    - 길이가 같으면 알고리즘은 각 위치를 반복하면서 해당 위치에 있는 두 문자를 비교합니다.
        - 이때 두 문자가 서로 일치하지 않으면 루프를 중지할 수 있습니다.
            - 문자열을 모두 비교했는데 불일치가 일어나지 않았다면 두 문자열을 같다고 선언할 수 있습니다.

아래의 그림은 이 알고리즘이 두 문자열에 대해 어떻게 작동하는지 보여줍니다. `=`는 비교할 때 서로 일치한 문자 쌍을 나타냅니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%AE%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%A7%E1%86%AF%E1%84%87%E1%85%B5%E1%84%80%E1%85%AD%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B7.png?raw=true">
- `X`는 최초 불일치로 인해 검사가 종료된 문자쌍을 나타냅니다.

문자열 비교에서 최악의 경우 계산 비용은 문자열의 길잉 비례해 증가합니다.
- 두 작은 문자열을 비교하는 작업에서는 무시할 수 있지만, 두 긴 문자열을 비교하는 작업에서는 시간이 오래 걸릴 수 있습니다.
    - 예를 들어, 어떤 책의 1판과 2판을 처음부터 한 글자씩 비교하면서 두 책의 본문 문자 배열의 차이를 찾는 지겨운 과정을 상상해볼 수 있습니다.
        - 가장 좋은 경우에는 초기에 일치하지 않는 부분을 찾을 수 있지만, 최악의 경우에는 책의 대부분을 검사해야 합니다.

많은 프로그래밍 언어, 예를 들어 파이썬과 같은 언어는 직접 비교할 수 있는 문자열 클래스를 제공합니다.
- 따라서 위 코드와 같은 비교 코드를 직접 구현할 필요가 없습니다.
    - 그러나 간단한 비교 함수의 뒤에는 모든 문자를 반복하는 루프가 있습니다.
        - **이 중요한 세부 사항을 이해하지 않으면 문자열 비교 비용을 과소평가할 수 있습니다.**
