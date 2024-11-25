---
title: "💾 [CS] Linux 명령어"
tags:
    - CS
date: "2024-11-25"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] Linux 명령어
## 1️⃣ 작업관리자 명령어.
```bash
ps aux
```
- 현재 실행중인 프로그램 목록을 확인할 수 있습니다.

```bash
ps aux | grep java
```
- 현재 실행중인 프로그램 중 java가 들어가는 프로그램을 확인할 수 있습니다.

## 2️⃣ 종료 명령어.
```bash
kill -9 {프로그램 번호}
```
- 해당 프로그램을 종료시킵니다.

<img src = "https://github.com/devKobe24/images2/blob/main/program_number.png?raw=true">
```bash
kill -9 207870
```
- `java -jar library-app/build/libs/library-app-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev`를 종료시킵니다.

## 3️⃣ 파일의 내용물을 확인해보는 방법.
### 1️⃣ 파일에 직접 들어가 내용물을 읽는 방법.
#### 1️⃣ 파일에 직접 들어가는 명령어.
```bash
vi
```
- vi: 리눅스 편집기인 vim을 사용하여 파일을 엽니다.

```bash
vi nohup.out
```
- `nohup.out` 파일을 리눅스 편집기인 `vim`을 사용하여 엽니다.

### 2️⃣ 파일에 들어가지 않고, 현재 접속한 터미널에서 내용물을 확인하는 방법.
#### 1️⃣ 파일에 있는 내용물을 모두 출력하는 명령어.
```bash
cat
```
- cat: 파일에 있는 내용물을 모두 출력하는 명령어.
    - 파일 내용물의 양이 많지 않고, 실시간 업데이트가 잘 되지 않는 파일을 확인할 때 사용합니다.

```bash
cat nobup.out
```
- `nohup.out` 파일에 있는 내용물을 모두 출력합니다.

#### 2️⃣ 파일의 끝부분을 확인하는 방법.
```bash
tail
```
- tail: 현재 파일의 끝 부분을 출력하는 명령어입니다.

```bash
tail nohup.out
```

#### 3️⃣ 파일의 끝 부분을 실시간으로 출력해주는 명령어.
```bash
tail -f
```
- tail -f: 현재 파일의 끝 부분을 실시간으로 출력해줍니다.

```bash
tail -f nohup.out
```
