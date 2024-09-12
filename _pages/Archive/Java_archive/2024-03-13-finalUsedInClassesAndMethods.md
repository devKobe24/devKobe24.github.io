---
title: "☕️[Java] 클래스와 메서드에 사용되는 final"
tags:
    - Java
    - Programming Language
date: "2024-03-13"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 클래스와 메서드에 사용되는 final.

## 클래스에 `final`
* 상속 끝!
* `final`로 선언된 클래스는 확장될 수 없습니다.
    * 다른 클래스가 `final`로 선언된 클래스를 상속받을 수 없습니다.
        * 예: `public final class MyFinalClass {...}`

## 메서드에 `final`
* 오버라이딩 끝!
* `final`로 선언된 메서드는 오버라이드 될 수 없습니다.
    * 상속받은 서브 클래스에서 이 메서드를 변경할 수 없습니다.
        * 예: `public final void myFinalMethod() {...}`
