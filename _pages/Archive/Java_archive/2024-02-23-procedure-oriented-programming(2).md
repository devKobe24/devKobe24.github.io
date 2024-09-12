---
title: "☕️[Java] 절차 지향 프로그래밍(2)"
tags:
    - Java
    - Programming Language
date: "2024-02-23"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 절차 지향 프로그래밍(2) - 데이터 묶음
**"절차 지향 프로그래밍(1)에 작성한 코드에 클래스를 도입합시다."**

`MusicPlayerData`라는 클래스를 만들고, 음악 플레이어에 사용되는 클래스를 만들고, 음악 플레이어에 사용되는 데이터들을 여기에 묶어서 멤버 변수로 사용해봅시다.

**MusicPlayerData**
```java
package oop1;

public class MusicPlayerData {
  int volume = 0;
  boolean isOn = false;
}
```
* 음악 플레이어에 사용되는 `volume`, `isOn` 속성을 `MusicPlayerData`의 멤버 변수에 포함했습니다.

**MusicPlayerMain2**
```java
package oop1;

public class MusicPlayerMain2 {

  public static void main(String[] args) {
    MusicPlayerData data = new MusicPlayerData();

    // 음악 플레이어 켜기
    data.isOn = true;
    System.out.println("음악 플레이어를 시작합니다.");

    // 볼륨 증가
    data.volume++;
    System.out.println("음악 플레이어 볼륨:" + data.volume);
    // 볼륨 증가
    data.volume++;
    System.out.println("음악 플레이어 볼륨:" + data.volume);
    // 볼륨 감소
    data.volume--;
    System.out.println("음악 플레이어 볼륨:" + data.volume);
    // 음악 플레이어 상태
    System.out.println("음악 플레이어 상태 확인");
    if (data.isOn) {
      System.out.println("음악 플레이어 ON, 볼륨:" + data.volume);
    } else {
      System.out.println("음악 플레이어 OFF");
    }
    // 음악 플레이어 끄기
    data.isOn = false;
    System.out.println("음악 플레이어를 종료합니다.");
  }
}
```
* 음악 플레이어와 관련된 데이터는 `MusicPlayerData` 클래스에 존재합니다. 
    * 이 클래스를 사용하도록 기존 로직을 변경했습니다.
        * 이후에 프로그램 로직이 더 복잡해져서 다양한 변수들이 추가되더라도 음악 플레이어와 관련된 변수들은 `MusicPlayerData data` 객체에 속해있으므로 쉽게 구분할 수 있습니다.
