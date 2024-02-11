---
title: 🤔 학습의 깊이와 그에 대한 사유(思惟) 그리고 자각(自覺)
date: 2024-01-15 11:39:00
modified: 2024-01-15 13:01:00
tags: [essay]
description: 학습의 깊이를 어느정도 하는지에 따른 나 스스로의 생각과 깨달음.
---

요 근래 많은 생각들이 오고 갔습니다.<br>

학습을 하는 데 있어서 올바른 방향으로 가는지 뒤돌아 볼 필요가 있다는 생각이 들었습니다.<br>

나는 곧장 앞으로 가고 있는데 막상 뒤를 돌아보니 이리저리 꼬부랑길을 걷는 것 마냥 걷고 있을 수 있으니 말입니다.<br>

사유(思惟)를 함으로써 자각(自覺)을 하게 되면 다시 올바른 방향으로 나 자신을 되잡게 됩니다.<br>

때문에 저는 스스로 생각을 했고, 그에 따라 많은 사람들의 의견이 필요하다는 판단이 들었습니다.<br>

그래서 요즘 활동중인 <strong>reddit의 swift 커뮤니티</strong>에 다음과 같은 제목과 내용으로 글을 올렸습니다.<br>
<br>
<img src="https://github.com/devKobe24/images/blob/main/2024-01-15-reddit-question-1.png?raw=true">
<img src="https://github.com/devKobe24/images/blob/main/2024-01-15-reddit-question-2.png?raw=true">
<br>
글의 전체적인 내용은 다음과 같습니다.<br>

- 나는 Swift 내부 구현이 어떻게 작성 되어있는지 궁금해서 처음 레포지토리를 들어다보게 되었습니다.
    - 이후 내부 구현을 보고 Swift의 동작과 흐름이 어떻게 흘러가는지가 궁금해지기 시작했습니다.
    - 그래서 내부 구현 코드를 보고 공부를 시작하다 보니 너무 깊게 학습을 하게되어 본래 주제였던 것을 잃게 되기도 합니다.

```swift
@frozen
@_eagerMove
public struct Array<Element>: _DestructorSafeContainer {
  #if _runtime(_ObjC)
  @usableFromInline
  internal typealias _Buffer = _ArrayBuffer<Element>
  #else
  @usableFromInline
  internal typealias _Buffer = _ContiguousArrayBuffer<Element>
  #endif

  @usableFromInline
  internal var _buffer: _Buffer

  /// Initialization from an existing buffer does not have "array.init"
  /// semantics because the caller may retain an alias to buffer.
  @inlinable
  internal init(_buffer: _Buffer) {
    self._buffer = _buffer
  }
}
```

그래서 위 예시를 들어 본격적으로 나의 학습 방법을 설명하며 고민과 질문을 말했습니다.<br>

- 먼저, Array를 주제로 잡고 공부하기로 하여 Array 내부 구현을 봅니다.
    - 첫 번째 줄부터 코드를 읽어나가기 시작합니다. 그래서 `@frozen`의 의미를 이해하려고 공부를하고,
    - 두 번째 줄 `@_eagerMove`의 의미를 이해하려고 공부를합니다.
    - 이후 세 번째 줄에서 `_DestructorSafeContainer` 프로토콜에 대해 공부를 하는데 여기서 갑자기 깊게 빠져버리는 현상이 일어나 학습의 방향이 `Array`에서 `_DestructorSafeContainer`로 바뀌어 버립니다.

<strong>이렇게 학습을 하면서 스스로 느끼기에 원래 학습 주제인 배열에서 벗어나는 느낌이 듭니다.<br> 이 학습 방법이 올바른 것인지 궁금합니다.</strong><br>

위와 같이 글을 올리니 불과 얼마되지 않아 정말 많은 View를 달성하게 되었습니다.<br>(현재 24년 1월 15일 오후 12시 34분 기준 7.4K Views 입니다. ㅇㅅㅇ!!)<br>

그리고 댓글은 딱 2가지의 반응으로 나뉘었습니다.<br>

1. Swift의 내부 작동 방식을 이해하지 않고도 Swift를 사용하는 방법을 알 수 있습니다. 앱을 구현하는 것이 궁극적인 목표라면 애플이 제공하는 공식 문서를 보는 것 만으로도 충분합니다.

2. Swift의 내부 작동 방식을 이해하는 것은 Swift를 더 깊이 이해할 수 있는 좋은 방법이며, Swift를 더 효율적으로 사용할 수 있습니다. 깊이있는 Swift 지식을 가지고 있다면 더 효율적인 방법과 더 좋은 코드를 생산해낼 수 있으며 그것을 기반으로 앱은 더 좋은 퍼포먼스를 만들어 냅니다.

그러면서도 공통적으로 말하는 포인트가 있었습니다.<br>

<strong>스위프트가 어떤 일을 하는지 아는 것은 매우 강력하며, 대부분의 사람들이 알지 못하는 방식으로 이해도를 높이는 데 도움을 줄것입니다.</strong><br>

<strong>그러나 가끔은 내부 탐색을 멈추고 실제 개발자 측면에서 배운 내용을 연습하여 '무엇'을 '어떻게' 활용할 수 있는지 알아보는 것이 좋습니다.</strong>라고 말씀해주셨습니다.<br>

"호기심 -> 학습 -> 통찰력 -> 창의력 -> 호기심 -> ... -> " 이런 사이클로 돌아가는 것이 아주 좋다고 칭찬도 아낌없이 해주셨습니다.<br>

그래서 제가 얻은 자각은 <strong>"내부 구현을 보고 학습하는 것은 좋으나 너무 깊게 들어가지 말고 적절한 선을 지켜야 겠다는 생각을 하게 되었습니다."</strong><br>

너무 깊은 곳까지 들어가 학습을 하게 된다면 주제를 잊어버리는 주객전도가 되어버리기 때문입니다.<br>

이렇게 지금 다시 방향을 잡았지만 또 어느날 뒤돌아 봤을 때 다시 방향을 잡는 날이 올 수 도 있습니다.<br>

저는 학습 방향에 올바른 길은 없다고 생각하며 꾸준히 발전할 수 있는 방향만 있다고 생각합니다.<br>

때문에 계속해서 더 발전할 수 있는 방향으로 바꾸어 나가는것이 최선인것 같습니다.<br>

---

## Reference 📚
<a href="https://www.reddit.com/r/swift/comments/19649qm/is_it_helpful_to_directly_look_into_the_swift/">👾 reddit | Is it helpful to directly look into the Swift Repository?</a>
