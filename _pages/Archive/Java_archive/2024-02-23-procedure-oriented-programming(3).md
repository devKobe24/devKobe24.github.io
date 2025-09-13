---
title: "☕️[Java] 절차 지향 프로그래밍(3)"
tags:
    - Java
    - Programming Language
date: "2024-02-23"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 절차 지향 프로그래밍(3) - 메서드 추출

코드를 보면 다음과 같이 중복되는 부분들이 있습니다.
```java
// 볼륨 증가
data.volume++;
System.out.println("음악 플레이어 볼륨:" + data.volume);

// 볼륨 증가
data.volume++;
System.out.println("음악 플레이어 볼륨:" + data.volume);
```

그리고 각각의 기능들은 이후에 재사용 될 가능성이 높습니다.
* 음악 플레이어 켜기, 끄기
* 볼륨 증가, 감소
* 음악 플레이어 상태 출력

**"메서드를 사용해서 각각의 기능을 구분해봅시다."**

**MusicPlayerMain3**
```java
package oop1;

public class MusicPlayerMain3 {

  public static void main(String[] args) {
    MusicPlayerData data = new MusicPlayerData();
    // 음악 플레이어 켜기
    on(data);
    // 볼륨 증가
    volumeUp(data);
    // 볼륨 증가
    volumeUp(data);
    // 볼륨 감소
    volumeDown(data);
    // 음악 플레이어 상태
    showStatus(data);
    // 음악 플레이어 끄기
    off(data);
  }

  static void on(MusicPlayerData data) {
    data.isOn = true;
    System.out.println("음악 플레이어를 시작합니다.");
  }

  static void off(MusicPlayerData data) {
    data.isOn = false;
    System.out.println("음악 플레이어를 종료합니다.");
  }

  static void volumeUp(MusicPlayerData data) {
    data.volume++;
    System.out.println("음악 플레이어 볼륨:" + data.volume);
  }

  static void volumeDown(MusicPlayerData data) {
    data.volume--;
    System.out.println("음악 플레이어 볼륨:" + data.volume);
  }

  static void showStatus(MusicPlayerData data) {
    System.out.println("음악 플레이어 상태 확인");
    if (data.isOn) {
      System.out.println("음악 플레이어 ON, 볼륨:" + data.volume);
    } else {
      System.out.println("음악 플레이어 OFF");
    }
  }
}
```

각각의 기능을 메서도로 만든 덕분에 각각의 기능이 모듈화 되었습니다.
덕분에 다음과 같은 장점이 생겼습니다.
* **중복 제거:** 로직 중복이 제거되었습니다. 같은 로직이 필요하면 해당 메서드를 여러번 호출하면 됩니다.
* **변경 영향 범위:** 기능을 수정할 때 해당 메서드 내부만 변경하면 됩니다.
* **메서드 이름 추가:** 메서드 이름을 통해 코드를 더 쉽게 이해할 수 있습니다.

> **모듈화:** 쉽게 이야기해서 레고 블럭을 생각하면 됩니다. 필요한 블럭을 가져다 꼽아서 사용할 수 있습니다. 여기서는 음악 플레이어의 기능이 필요하면 해당 기능을 메서드 호출 만으로 손쉽게 사용할 수 있습니다. 이제 음악 플레이어와 관련된 메서드를 조립해서 프로그램을 작성할 수 있습니다.

**절차 지향 프로그래밍의 한계**
지금까지 클래스를 사용해서 관련된 데이터를 하나로 묶고, 또 메서드를 사용해서 각각의 기능을 모듈화했습니다.
덕분에 상당히 깔끔하고 읽기 좋고, 유지보수 하기 좋은 코드를 작성할 수 있었습니다.
하지만 여기서 더 개선할 수는 없을까요?

작성한 코드의 한계는 바로 데이터와 기능이 분리되어 있다는 점입니다.
음악 플레이어의 데이터는 `MusicPlayerData`에 있는데, 그 데이터를 사용하는 기능은 `MusicPlayerMain3`에 있는 각각의 메서드에 분리되어 있습니다.
그래서 음악 플레이어와 관련된 데이터는 `MusicPlayerData`를 사용해야 하고, 음악 플레이어와 관련된 기능은 `MusicPlayerMain3`의 각 메서드를 사용해야 합니다.

데이터와 그 데이터를 사용하는 기능은 매우 밀접하게 연관되어 있습니다.
각각의 메서드를 보면 대부분 `MusicPlayerData`의 데이터를 사용합니다.
따라서 이후에 관련 데이터가 변경되면 `MusicPlayerMain3` 부분의 메서드들도 함께 변경해야 합니다.
그리고 이렇게 데이터와 기능이 분리되어 있으면 유지보수 관점에서도 관리 포인트가 2곳으로 늘어납니다.

**"객체 지향 프로그래밍이 나오기 전까지는 지금과 같이 데이터와 기능이 분리되어 있었습니다."**
* 따라서 지금과 같은 코드가 최선이였습니다.
    * 하지만 객체 지향 프로그래밍이 나오면서 데이터와 기능을 온전히 하나로 묶어서 사용할 수 있게 되었습니다.

**"데이터와 기능을 하나로 온전히 묶는다는 것이 어떤 의미인지 이해하기 위해 간단한 예시 코드를 만들어봅시다"**
> 이 예시 코드는 다음 포스팅에 이어집니다.
