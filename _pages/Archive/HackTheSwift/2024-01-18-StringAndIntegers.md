---
title: 👾 Day 2 - String And Integers
date: 2024-01-18 10:58:00
modified: 2024-01-18 11:10:00
tags: [Swift, Programming]
description: 문자열과 숫자형
---

Swift는 타입 안전(type-safe) 언어로 알려져 있으며, 이는 모든 변수가 하나의 특정 타입이어야 한다는 것을 의미합니다.<br>
Xcode가 우리를 위해 생성한 <a href="https://www.devkobe24.com/Variables/">`greeting`</a> 변수는 "Hello, Kobe"라는 글자들의 문자열을 담고 있으므로 Swift는 이 변수에 `String` 타입을 할당합니다.<br>
<br>
반면, 누군가의 나이를 저장하고 싶다면 다음과 같은 변수를 만들 수 있습니다.

```swift
var age = 34
```

이 변수는 전체 숫자를 담고 있으므로 Swift는 이에 `Int` 타입을 할당합니다 - 'integer'의 줄임말입니다.<br>
<br>
큰 숫자를 다룰 때, Swift는 턴 단위 구분자로 밑줄을 사용할 수 있게 해줍니다. - 이것은 숫자를 변경하지 않지만 읽기 쉽게 만들어줍니다.<br>
예를 들어:
```swift
var population = 8_000_000
```

문자열과 정수는 다른 타입이며, 혼합될 수 없습니다.<br>
그래서 `greeting`을 "Hello, Min Seong"으로 변경하는 것은 안전하지만, 34로 변경하는 것은 불가능합니다.<br>
왜냐하면 그것은 `Int`이지 `String`이 아니기 때문입니다.
