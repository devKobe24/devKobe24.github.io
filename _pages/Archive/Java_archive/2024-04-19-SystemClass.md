---
title: "☕️[Java] System 클래스"
tags:
    - Java
    - Programming Language
date: "2024-04-19"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# System 클래스
`System` 클래스는 시스템과 관련된 기본 기능들을 제공합니다.

```java
package lang.system;

import java.util.Arrays;

public class SystemMain {

  public static void main(String[] args) {
    // 현재 시간(밀리초)를 가져옵니다.
    long currentTimeMillis = System.currentTimeMillis();
    System.out.println("currentTimeMillis = " + currentTimeMillis);

    // 현재 시간(나노초)를 가져옵니다.
    long currentTimeNano = System.nanoTime();
    System.out.println("currentTimeNano = " + currentTimeNano);

    // 환경 변수를 읽습니다.
    System.out.println("getenv = " + System.getenv());

    // 시스템 속성을 읽습니다.
    System.out.println("properties = " + System.getProperties());
    System.out.println("Java version = " + System.getProperty("java.version"));

    // 배열을 고속으로 복사합니다.
    char[] originalArray = { 'h', 'e', 'l', 'l', 'o' };
    char[] copiedArray = new char[5];
    System.arraycopy(originalArray, 0, copiedArray, 0, originalArray.length);

    // 배열 출력
    System.out.println("copiedArray = " + copiedArray);
    System.out.println("Arrays.toString = " + Arrays.toString(copiedArray));

    // 프로그램 종료
    System.exit(0);
  }
}
```

**실행 결과**
```
currentTimeMillis = 1713485140558

currentTimeNano = 694481339313708

getenv = {HOMEBREW_PREFIX=/opt/homebrew, MANPATH=/Users/kobe/.nvm/versions/node/v20.10.0/share/man:/opt/local/share/man:/opt/homebrew/share/man::, COMMAND_MODE=unix2003, INFOPATH=/opt/homebrew/share/info:...

properties = {java.specification.version=21, sun.jnu.encoding=UTF-8, java.class.path=/Users/kobe/Desktop/practiceJavaMidPart1/java-mid1/out/production/java-mid1, java.vm.vendor=Oracle Corporation, sun.arch.data.model=64, java.vendor....

Java version = 21.0.2

copiedArray = [C@77459877
Arrays.toString = [h, e, l, l, o]

Process finished with exit code 0
```

- **표준 입력, 출력, 오류 스트림:** `System.in`, `System.out`, `System.err`은 각각 표준 입력, 표준 출력, 표준 오류 스트림을 나타냅니다.
- **시간 측정:** `System.currentTimeMillis()`와 `System.nanoTime()`은 현재 시간을 밀리초 또는 나노초 단위로 제공합니다.
- **환경 변수:** `System.getProperties()`를 사용해 현재 시스템 속성을 얻거나 `System.getProperty(Stringkey)`로 특정 속성을 얻을 수 있습니다. 시스템 속성은 자바에서 사용하는 설정 값입니다.
- **시스템 종료:** `System.exit(int status)` 메서드는 프로그램을 종료하고, OS에 프로그램 종료의 상태 코드를 전달합니다.
    - 상태 코드`0` : 정상 종료
    - 상태 코드 `0`이 아님: 오류나 예외적인 종료
- **배열 고속 복사:** `System.arraycopy`는 시스템 레벨에서 최적화된 메모리 복사 연산을 사용합니다. 직접 반복문을 사용해서 배열을 복사할 때 보다 수 배 이상 빠른 성능을 제공합니다.
