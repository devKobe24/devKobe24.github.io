---
title: "☕️[Java] 스택과 큐 자료구조"
tags:
    - Java
    - Programming Language
date: "2024-03-03"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 스택과 큐 자료구조.

자바 메모리 구조 중 스택 영역에 대해 알아보기 전에 먼저 스택(Stack)이라는 자료 구조에 대해서 알아봅시다.

## 스택 구조
다음과 같은 1, 2, 3 이름표가 붙은 블럭이 있다고 가정해봅시다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%B3%E1%84%90%E1%85%A2%E1%86%A8%E1%84%80%E1%85%AE%E1%84%8C%E1%85%A91.png?raw=true">

* 이 블럭을 다음과 같이 생긴 통에 넣는다고 생각해봅시다.
    * 위쪽만 열려있기 때문에 위쪽으로 블럭을 넣고, 위쪽으로 블럭을 빼야 합니다.
        * 쉽게 이야기해서 넣는 곳과 빼는 곳이 같습니다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%B3%E1%84%90%E1%85%A2%E1%86%A8%E1%84%80%E1%85%AE%E1%84%8C%E1%85%A92.png?raw=true">

* 블럭은 1 -> 2 -> 3 순서대로 넣을 수 있습니다.

이번에는 넣은 블럭을 빼봅시다.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%B3%E1%84%90%E1%85%A2%E1%86%A8%E1%84%80%E1%85%AE%E1%84%8C%E1%85%A93.png?raw=true">

* 블럭을 빼려면 위에서 부터 순서대로 빼야합니다.
    * 블럭은 3 -> 2 -> 1 순서로 뺄 수 있습니다.

**정리하면 다음과 같습니다.**
* 1(넣기) -> 2(넣기) -> 3(넣기) -> 3(빼기) -> 2(빼기) -> 1(빼기)

### 후입 선출(LIFO, Last In First Out)
여기서 가장 마지막에 넣은 3번이 가장 먼저 나옵니다.
* 이렇게 가장 먼저 나오는 것을 "후입 선출"이라 하고, 이런 자료 구조를 "스택"이라 합니다.

### 선입 선출(FIFO, First In First Out)
* 후입 선출과 반대로 가장 먼저 넣은 것이 가장 먼저 나오는 것을 선입 선출이라고 합니다.
    * 이런 자료 구조를 "큐(Queue)"라 합니다.

## 큐(Queue) 자료 구조
<img src="https://github.com/devKobe24/images/blob/main/%E1%84%8F%E1%85%B2%E1%84%80%E1%85%AE%E1%84%8C%E1%85%A91.png?raw=true">

**정리하면 다음과 같습니다.**
* 1(넣기) -> 2(넣기) -> 3(넣기) -> 1(빼기) -> 2(빼기) -> 3(빼기)
    * 이런 자료 구조는 각자 필요한 영역이 있습니다.
        * 예를 들어 선착순 이벤트를 하는데 고객이 대기해야 한다면 큐 자료 구조를 사용해야 합니다.
