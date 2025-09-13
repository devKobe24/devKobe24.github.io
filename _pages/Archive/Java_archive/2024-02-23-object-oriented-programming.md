---
title: "☕️[Java] 객체 지향 프로그래밍"
tags:
    - Java
    - Programming Language
date: "2024-02-23"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 객체 지향 프로그래밍.
지금까지 개발한 음악 플레이어는 데이터와 기능이 분리되어 있습니다.
이제 데이터와 기능을 하나로 묶어서 음악 플레이어라는 개념을 온전히 하나의 클래스에 담아보도록 하겠습니다.
프로그램을 작성하는 절차도 중요하지만 **"지금은 음악 플레이어라는 개념을 객체로 온전히 만드는 것이 더 중요합니다."**
음악 플레이어라는 객체를 지향해봅시다!
그러기 위해서는 프로그램의 실행 순서 보다는 **"음악 플레이어 클래스를 만드는 것 자체에 집중해야 합니다."**
**"음악 플레이어가 어떤 속성(데이터)을 가지고 어떤 기능(메서드)을 제공하는지 이 부분에 초점을 맞추어야 합니다."**
지금부터 우리는 음악 플레이어를 개발하는 개발자가 될 것입니다.
이것을 어떨게 사용할지는 분리해서 생각해봅시다.
쉽게 이야기해서 음악 플레이어를 만들어서 제공하는 개발자와 음악 플레이어를 사용하는 개발자가 분리되어 있다고 생각하면 됩니다.

**음악 플레이어**
* 속성: `volume`, `isOn`
* 기능: `on()`, `off()`, `volumeUp()`, `volumeDown()`, `showStatus()`

이것을 가지고 음악 플레이어를 만들어봅시다.

## 객체 지향 음악 플레이어.

**MusicPlayer**

```java
package oop1;

public class MusicPlayer {

  int volume = 0;
  boolean isOn = false;

  void on() {
    isOn = true;
    System.out.println("음악 플레이어를 시작합니다.");
  }

  void off() {
    isOn = false;
    System.out.println("음악 플레이어를 종료합니다.");
  }

  void volumeUp() {
    volume++;
    System.out.println("음악 플레이어 볼륨:" + volume);
  }

  void volumeDown() {
    volume--;
    System.out.println("음악 플레이어 볼륨:" + volume);
  }

  void showStatus() {
    System.out.println("음악 플레이어 상태 확인");
    if (isOn) {
      System.out.println("음악 플레이어 ON, 볼륨:" + volume);
    } else {
      System.out.println("음악 플레이어 OFF");
    }
  }
}
```

* `MusicPlayer` 클래스에 음악 플레이어에 필요한 속성과 기능을 모두 정의했습니다.
    * 이제 음악 플레이어가 필요한 곳에서 이 클래스만 있으면 온전한 음악 플레이어를 생성해서 사용할 수 있습니다.
    * 음악 플레이어를 사용하는데 필요한 모든 속성과 기능이 하나의 클래스에 포함되어 있습니다.

**MusicPlayerMain4**
```java
package oop1;

public class MusicPlayerMain4 {

  public static void main(String[] args) {
    MusicPlayer player = new MusicPlayer();
    // 음악 플레이어 켜기
    player.on();
    // 볼륨 증가
    player.volumeUp();
    // 볼륨 증가
    player.volumeUp();
    // 볼륨 감소
    player.volumeDown();
    // 음악 플레이어 상태
    player.showStatus();
    // 음악 플레이어 끄기
    player.off();
  }
}
```

**실행 결과**
```
음악 플레이어를 시작합니다.
음악 플레이어 볼륨:1
음악 플레이어 볼륨:2
음악 플레이어 볼륨:1
음악 플레이어 상태 확인
음악 플레이어 ON, 볼륨:1
음악 플레이어를 종료합니다.
```

**"`MusicPlayer`를 사용하는 코드를 봅시다."**
`MusicPlayer` 객체를 생성하고 필요한 기능(메서드)을 호출하기만 하면 됩니다. 
필요한 모든 것은 `MusicPlayer` 안에 들어있습니다.

* `MusicPlayer`를 사용하는 입장에서는 `MusicPlayer`의 데이터인 `volumn`, `isOn` 같은 데이터는 전혀 사용하지 않습니다.
* `MusicPlayer`를 사용하는 입장에서는 이제 `MusicPlayer` 내부에 어떤 속성(데이터)이 있는지 전혀 몰라도 됩니다.
    * `MusicPlayer`를 사용하는 입장에서는 단순하게 `MusicPlayer`가 제공하는 기능 중에 필요한 기능을 호출해서 사용하기만 하면 됩니다.

**캡슐화**
`MusicPlayer`를 보면 음악 플레이어를 구성하기 위한 속성과 기능이 마치 하나의 캡슐에 쌓여있는 것 같습니다.
**"이렇게 속성과 기능을 하나로 묶어서 필요한 기능을 메서드를 통해 외부에 제공하는 것을 캡슐화라 합니다."**

**"객체 지향 프로그래밍"** 덕분에 음악 플레이어 객체를 사용하는 입장에서 진짜 음악 플레이어를 만들고 사용하는 것 처럼 친숙하게 느껴집니다.
* 그래서 코드가 더 읽기 쉬운 것은 물론이고, 속성과 기능이 한 곳에 있기 때문에 변경도 더 쉬워집니다.
    * 예를 들어서 `MusicPlayer` 내부 코드가 변하는 경우에 다른 코드는 변경하지 않아도 됩니다.
        * `MusicPlayer`의 `volume`이라는 필드 이름이 다른 이름으로 변한다고 할 때 `MusicPlayer` 내부만 변경하면 됩니다.
        * 또 음악 플레이어가 내부에서 출력하는 메시지를 변경할 때도 `MusicPlayer` 내부만 변경하면 됩니다.
            * 이 경우 `MusicPlayer`를 사용하는 개발자는 코드를 전혀 변경하지 않아도 됩니다.
                * 물론 외부에서 호출하는 `MusicPlayer`의 메서드 이름을 변경한다면 `MusicPlayer`를 사용하는 곳의 코드도 변경해야 합니다.
