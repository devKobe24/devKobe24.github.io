---
title: "☕️[Java] 접근 제어자 이해 2"
tags:
    - Java
    - Programming Language
date: "2024-02-27"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 접근 제어자 이해 2
* 이 문제를 근본적으로 해결하는 방법은 `volume` 필드를 `Speaker` 클래스 외부에서는 접근하지 못하게 막는 것 입니다.

**Speaker - volume 접근 제어자를 private으로 수정**
```java
package access;

public class Speaker {
    
    private int volume; // private 사용
    ...
}
```

* `private` 접근 제어자는 모든 외부 호출을 막습니다.
    * 따라서 `private`이 붙는 경우 해당 클래스 내부에서만 호출할 수 있습니다.

**volume 필드 - private 변경 후**
* `volume` 필드를 `private`을 사용해서 `Speaker` 내부에 숨겼습니다.
    * 외부에서 `volume` 필드에 직접 접근할 수 없게 막은 것입니다.
        * `volume` 필드는 이제 `Speaker` 내부에서만 접근할 수 있습니다.

`SpeakerMain` 코드를 다시 실행해보겠습니다.
```java
// 필드에 직접 접근
System.out.println("volume 필드 직접 접근 수정");
speaker.volume = 200; // private 접근 오류
```

IDE에서 `speaker.volume = 200` 부분에 오류가 발생하는 것을 확인할 수 있습니다.
* 실행해보면 다음과 같은 컴파일 오류가 발생합니다.

**컴파일 오류 메시지**
```
java: volume has private access in access.Speaker
```
* `volume` 필드는 `private`으로 설정되어 있기 때문에 외부에서 접근할 수 없다는 오류입니다.

**volume 필드 직접 접근 - 주석 처리**
```java
// 필드에 직접 접근
System.out.println("volume 필드에 직접 접근 수정");
//speaker.volume = 200; //private 접근 오류
speaker.showVolume();
```

* 이제 `Speaker` 외부에서 `volume` 필드에직접 접근하는 것은 불가능합니다.
    * 이 경우 자바 컴파일러가 컴파일 오류를 발생시킵니다.
        * 프로그램을 실행하기 위해서 `volume` 필드에 직접 접근하는 코드를 주석 처리합니다.

만약 `Speaker` 클래스를 개발하는 개발자가 처음부터 `private`을 사용해서 `volume` 필드의 외부 접근을 막아두었다면 어떠했을까요?

새로운 개발자도 `volume` 필드에 직접 접근하지 않고, `volumeUp()` 과 같은 메서드를 통해서 접근했을 것입니다.
* 결과적으로 `Speaker`가 폭발하는 문제는 발생하지 않았을 것입니다.

> **참고:** 좋은 프로그램은 무한한 자유도가 주어지는 프로그램이 아니라 적절한 제약을 제공하는 프로그램입니다.
