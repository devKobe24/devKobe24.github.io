---
title: "☕️[Java] Main 클래스 생성 후 오류 대처."
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-07-31"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] Main 클래스 생성 후 오류 대처.

## 1️⃣ 메인 클래스 생성.
- 모든 프로젝트에는 메인 클래스가 있어야 합니다.
    - 직접 만든 클래스를 메인 클래스로 사용하기 위해 다음과 같이 코드를 입력했다고 가정해봅시다.

```java
// PortfolioBolgApplication.java

package com.devkobe.portfolioBlog;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class PortfolioBolgApplication {
    public static void main(String[] args) {
        SpringApplication.run(PortfolioBolgApplication.class, args);
    }
}
```

- 코드 작성이 끝났다면 클래스 왼쪽에 실행 아이콘 ▶︎을 누르고, `[RUN]` 버튼을 눌러 클래스를 실행했을 경우 콘솔창에서 애플리케이션이 실행되면 성공입니다.

## 2️⃣ 실패했을 경우.

```shell
Process 'command...bin/java 'finshed with non-zero exit value 1'
```
- 콘솔창에 위와 같은 오류 발생시에는 다음과 같이 순차적으로 해결하면 됩니다.
    - 1. Settings 로 들어갑니다.
    - 2. Build, Excecution, Deplyment 카테고리를 찾아 펼칩니다.
    - 3. 하위에 Gradle을 클릭합니다.
    - 4. 'Build and run using' 이라는 색션을 찾습니다.
        - 이것의 선택값이 **'Gradle(default)'** 일 것입니다. 
            - 이 값을 **'IntelliJ IDEA'** 로 바꿔 프로젝트를 다시 시작합니다.
