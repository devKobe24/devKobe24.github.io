---
title: "☕️[Java] 접근 제어자 이해 1"
tags:
    - Java
    - Programming Language
date: "2024-02-27"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 접근 제어자

## 접근 제어자 이해 1.

* 자바는 `public`, `private` 같은 접근 제어자(access modifier)를 제공합니다.
    * 접근 제어자를 사용하면 해당 클래스 외부에서 특정 필드나 메서드에 접근하는 것을 허용하거나 제한할 수 있습니다.

이런 접근 제어자가 왜 필요할까요?
* 예제를 통한 접근 제어자가 필요한 이유를 알아봅시다.

여러분은 스피커에 들어가는 소프트웨어 개발자 입니다.
스피커의 음량은 절대로 100을 넘으면 안된다는 요구사항이 있습니다(**100을 넘어가면 스피커의 부품들이 고장납니다.**)

스피커 객체를 만들어봅시다.
스피커는 음량을 높이고, 내리고, 현재 음량을 확인할 수 있는 단순한 기능을 제공합니다.
요구사항 대로 스피커의 음량은 100까지만 증가할 수 있습니다.
절대 100을 넘어가면 안됩니다.

**Speaker**
```java
package access;

public class Speaker {
  int volume;

  Speaker(int volume) {
    this.volume = volume;
  }

  void volumeUp() {
    if (volume >= 100) {
      System.out.println("음량을 증가할 수 없습니다. 최대 음량입니다.");
    } else {
      volume += 10;
      System.out.println("음량을 증가합니다.");
    }
  }

  void volumeDown() {
    volume -= 10;
    System.out.println("volumeDown 호출");
  }

  void showVolume() {
    System.out.println("현재 음량: " + volume);
  }
}
```
* 생성자를 통해 초기 음량 값을 지정할 수 있습니다.
* `volumeUp()` 메서드를 봅시다. 음량이 한 번에 10씩 증가합니다.
    * 단 음량이 100을 넘게되면 더는 음량을 증가하지 않습니다.

**SpeakerMain**
```java
package access;

public class SpeakerMain {

  public static void main(String[] args) {
    Speaker speaker = new Speaker(90);
    speaker.showVolume();

    speaker.volumeUp();
    speaker.showVolume();

    speaker.volumeUp();
    speaker.showVolume();
  }
}
```

**실행 결과**
```
현재 음량: 90
음량을 증가합니다.
현재 음량: 100
음량을 증가할 수 없습니다. 최대 음량입니다.
현재 음량: 100
```

초기 음량 값을 90으로 지정했습니다.
그리고 음량을 높이는 메서드를 여러번 호출했습니다.
기대한 대로 음량은 100을 넘지 않았습니다.
프로젝트는 성공적으로 끝났습니다.

오랜 시간이 흘러 업그레이드 된 다음 버전의 스피커를 출시하게 되었습니다.
이때 새로운 개발자가 급하게 기존 코드를 이어받아서 개발하게 되었습니다.
참고로 새로운 개발자는 기존 요구사항을 잘 몰랐습니다.
코드를 실행해보니 이상하게 음량이 100이상 올라가지 않았습니다.
소리를 더 올리면 좋겠다고 생각한 개발자는 다양한 방면으로 고민했습니다.
`Speaker` 클래스를 보니 `volume` 필드를 직접 사용할 수 있었습니다.
`volume` 필드의 값을 200으로 설정하고 이 코드를 실행한 순간 스피터의 부풀들이 과부하가 걸리면서 폭발했습니다.

**SpeakerMain - 필드 직접 접근 코드 추가**
```java
package access;

public class SpeakerMain {

  public static void main(String[] args) {
    Speaker speaker = new Speaker(90);
    speaker.showVolume();

    speaker.volumeUp();
    speaker.showVolume();

    speaker.volumeUp();
    speaker.showVolume();

    // 필드에 직접 접근
    System.out.println("volume 필드에 직접 접근 수정");
    speaker.volume = 200;
    speaker.showVolume();
  }
}
```

**실행 결과**
```
현재 음량: 90
음량을 증가합니다.
현재 음량: 100
음량을 증가할 수 없습니다. 최대 음량입니다.
현재 음량: 100
volume 필드에 직접 접근 수정
현재 음량: 200
```

* `Speaker` 객체를 사용하는 사용자는 `Speaker`의 `volume` 필드와 메서드에 모두 접근할 수 있습니다.
    * 앞서 `volumeUp()`과 같은 메서드를 만들어서 음량이 100을 넘지 못하도록 개발했지만 소용이 없습니다.
        * 왜냐하면 `Speaker`를 사용하는 입장에서는 `volume` 필드에 직접 접근해서 원하는 값을 설정할 수 있기 때문입니다.

**"이런 문제를 근본적으로 해결하기 위해서는 `volume` 필드의 외부 접근을 막을 수 있는 방법이 필요합니다."**
