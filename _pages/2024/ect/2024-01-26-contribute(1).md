---
title: 🍎[Contribute] 나의 두 번째 기여 도전기 Array와 주석, 그 첫 번째 이야기.
date: 2024-01-26 07:38:00
modified: 2024-01-26 08:48:00
tags: [Contribute, Apple]
description: Indentation
---

# 🍎[Contribute] 나의 두 번째 기여 도전기 Array와 주석, 그 첫 번째 이야기.

<p>
    2주 전 <strong>Array</strong> 내부 구현을 보다가 <strong>주석(comment)</strong> 부분에서 조금 어색한 느낌을 받았습니다.
</p>

<p>
    <pre><code>
    /// You can access individual array elements through a subscript. The first
    /// element of a nonempty array is always at index zero. You can subscript an
    /// array with any integer from zero up to, but not including, the count of
    /// the array. Using a negative number or an index equal to or greater than
    /// `count` triggers a runtime error. For example:
    ///
    ///     print(oddNumbers[0], oddNumbers[3], separator: ", ")
    ///     // Prints "1, 7"
    ///
    ///     print(emptyDoubles[0])
    ///     // Triggers runtime error: Index out of range
    </code></pre>
</p>

<p>
    위 의 코드 조각은 <strong>Array.swift</strong>에서 제가 어색하다고 느낀 주석 부분을 그대로 가져온 것 입니다.
</p>
    
<p>
    그 부분을 해석해보면 아래와 같습니다.
</p>

<p>
    배열의 개별 요소에는 첨자(subscript)를 통해 접근할 수 있습니다.<br>
    비어 있지 않은 배열의 첫 번째 요소는 항상 인덱스 0에 있습니다.<br>
    배열에 대한 첨자로는 0부터 배열의 개수보다 작은 어떤 정수도 사용할 수 있습니다.<br>
    음수를 사용하거나 배열의 count와 같거나 그보다 큰 인덱스를 사용하면 런타임 오류가 발생합니다. 예를 들어:
</p>

## 1️⃣ 이 부분이 어색하다고 생각했습니다 🤔

<p>
    주석에는 <strong>"첨자로는 0부터 배열의 개수보다 작은 어떤 정수도 사용할 수 있다."</strong>라고 나와있습니다.<br>
    제 개인적인 견해로서 이 주석 밑에 예시는 <strong>"첨자로 0부터 배열의 개수보다 작은 어떤 정수"</strong>를 사용한 배열 예시가 명시되어야 한다고 생각 했었습니다.
</p>

## 2️⃣ 이 부분이 어색하다고 생각했습니다 🤔

<p>
    주석에는 <strong>"음수를 사용하거나 배열의 count와 같거나 그보다 큰 인덱스를 사용하면 런타임 오류가 발생합니다."</strong>라고 명시되어 있습니다.<br>
    저의 개인적인 생각으로는 이 주석의 예시로는 <strong>"첨자로 음수를 사용한 배열"</strong>과 <strong>"첨자로 배열의 count와 같은 배열"</strong> 그리고 <strong>"첨자로 배열의 count보다 큰 인덱스를 사용한 배열"</strong>을 사용한 예시가 명시되어야 한다고 생각 했었습니다.
</p>

## 🙋‍♂️ 그래서 저는 그 예시를 넣어서 PR을 보냈습니다.

<img src="https://github.com/devKobe24/images/blob/main/2024-01-26-PR-1.png?raw=true"><br>

<p>
    리뷰어 님께 처음 달린 코멘트는 다음과 같았습니다.<br>
    <br>
    <strong>"위 단락에서는 이러한 경우를 런타임 에러가 발생하는 것으로 설명했지만, 이렇게 명시적으로 설명해도 나쁘지 않습니다. 다양한 언어의 배열을 오랫동안 사용해 보지 않은 초보 Swift 개발자에게 도움이 될 것입니다."</strong>
</p>

<p>
    <strong>하지만 달콤한 코멘트는 잠시...였습니다.</strong><br>
</p>

<p>
    위 코멘트는 <strong>"추가 논의"</strong>가 필요하다는 코멘트가 달린 뒤 다른 리뷰어 님께서 참여하셔서 코멘트를 달아주셨습니다.
</p>

<p>
    "Thanks for this suggested change! I think this approach has some issues, and described an alternate approach in comments."<br>
    <br>
    번역 : "변경 사항을 제안해 주셔서 감사합니다! 이 접근 방식에는 몇 가지 문제가 있다고 생각하여 댓글로 다른 접근 방식을 설명했습니다."
</p>

<p>
    <strong>이후에 리뷰어 님께서 리퀘스트 체인지를 주셨고 함께 긴 시간 동안 추가 논의를 하는 시간을 가졌습니다.(약 8일?)</strong>
</p>

<img src="https://github.com/devKobe24/images/blob/main/2024-01-26-PR-2.png?raw=true">
<img src="https://github.com/devKobe24/images/blob/main/2024-01-26-PR-3.png?raw=true">

<p>
    <strong> 이렇게 오랜 시간동안의 핑퐁은 처음이여서 너무 설레고 기분이 상당히 좋았습니다!!</strong>(그렇다고 절 변태로 생각하시면 안됩....니..다.. 헤헿 ㅇㅅaㅇ)
</p>

<p>
    긴 핑퐁 끝에 저의 제안은 삭제하게 되었습니다.<br>
    <strong>하지만 여기서 끝이 아닙니다!!</strong><br>
    과연 뒤에 어떤 일이 기다릴까요?!
</p>

## 🙋‍♂️ 이 글은 시리즈입니다 여러분... 히히 커밍쑤운!!
