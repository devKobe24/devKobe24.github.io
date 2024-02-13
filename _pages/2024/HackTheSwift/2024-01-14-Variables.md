---
title: 👾 Day 1 - Variables
date: 2024-01-14 15:01:00
modified: 2024-01-14 15:24:00
tags: [Swift]
description: Swift의 변수에 대하여 
---

변수(Variables)는 프로그램 데이터를 저장할 수 있는 장소입니다.<br>
<strong>'변수(variables)'</strong>라고 불리는 이유는 그 값이 변할 수 있기 때문입니다.<br>
즉, 값을 자유롭게 변경할 수 있다는 뜻 입니다.<br>
<br>
플레이그라운드를 열고 아래와 같은 코드를 작성해봅시다.<br>

```swift
var greeting = "Hello, Kobe"
```

위 코드는 <strong>'Hello, Kobe'</strong>라는 값을 가진 <strong>greeting</strong>라는 새로운 변수를 생성합니다.<br>플레이그라운드 오른쪽에 있는 영역을 보면 'Hello, Kobe'를 볼 수 있습니다. - 이것은 Xcode가 값을 설정했다는 것을 보여주는 것 입니다.<br>

<strong>greeting</strong>은 변수이기 때문에 변경이 가능합니다:

```swift
greeting = "Hello, Min Seong"
```

값을 선언 또는 초기화 한 이후 값을 변경하려 할 때는 <strong>'var'</strong> 키워드는 필요하지 않습니다.<br>
왜냐하면 변수가 이미 생성되었기 때문입니다. - 우리는 단지 그것을 변경하는 것뿐입니다.<br>
