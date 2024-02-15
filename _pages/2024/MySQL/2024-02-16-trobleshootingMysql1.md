---
title: "🐋[SQL] MySQL Server Start/Stop"
tags:
    - MySQL
date: "2024-02-16"
thumbnail: "/assets/img/thumbnail/database.jpeg"
---

# MySQL Server Start/Stop

**초기 세팅**
처음 MySQL을 설치시 MySQL 웹 사이트에 접속하여 DMG File을 받아서 설치하는 방법과 Homebrew를 활용하여 설치하는 방법 두 가지가 있습니다.
* 저는 MySQL을 Homebrew를 이용해서 설치하지 않고 MySQL 웹 사이트에 접속하여 DMG File을 받아 설치하였습니다.
    * (요것이 약간의 패착?😵‍💫)

정상 설치 이후에 `mysql.server start` 명령어를 terminal에 입력하면 아래 그림과 같은 에러가 발생했습니다.

<img src="https://github.com/devKobe24/images/blob/main/mysql%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%89%E1%85%A9%E1%86%A8%E1%84%8B%E1%85%A6%E1%84%85%E1%85%A5-0.png?raw=true">

* homebrew로 설치했다면 .zshrc 파일에 아마 PATH 환경변수가 잘 추가가 되어 이런 에러가 나지 않았을 것입니다.
    * 만약 이런 에러가 그래도 발생했다면 간단하게 PATH 환경변수를 .zshrc 파일 내부에 `export PATH=$PATH:/usr/local/mysql/bin` 이런식으로 넣어 준 뒤 터미널을 재시작해주면 됩니다.

저는 homebrew로 설치하지 않았으므로 저 명령어가 일단은 안되는게 정상이여서 homebrew로 설치하지 않았을 경우 어떻게 서버를 동작시키는지 알아봤습니다.

* 먼저 `/usr/local/mysql/data/` 로 이동합니다.
* **`sudo`** 명령어를 활용하여 `sudo /usr/local/mysql/support-files/mysql.server start` 명령어를 입력해줍니다.

이와 같이 간단하게 서버를 동작 시킬 수 있었습니다.

반대로 서버를 닫을 땐 `sudo /usr/local/mysql/support-files/mysql.server stop`를 사용하면 됩니다.

---

## 참고 문서

- [How to fix [Errno 13] Permission denied error on MySQL](https://medium.com/@kjavaman12/how-to-fix-errno-13-permission-denied-error-on-mysql-71b35e0efdd1)
- [윤카일의 개발공간 | [mysql] mysql 실행 -> 접속 -> 접속 끊기 -> 종료 (+ 데몬으로 실행 및 종료)](https://yoons-development-space.tistory.com/11)
