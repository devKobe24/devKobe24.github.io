---
title: 🛠️[개발 도구 및 환경] MySQL root 비밀번호 재설정하기 for MacOS.
tags:
    - Development tools
    - Enviroments
date: "2025-01-21"
thumbnail: "/assets/img/thumbnail/tools.jpg"
---

# 🛠️[개발 도구 및 환경] MySQL root 비밀번호 재설정하기 for MacOS.
## 📌 Intro.
- ↘︎ 필자는 `8.0.39` 버전을 사용중이며 `8.0.39` 버전을 기준으로 설명합니다.
    - ↘︎ 다른 버전은 비밀번호 재설정이 다를 수 있습니다 ⚠️

## ✅1️⃣ MySQL 서버 확인 및 중지하기.
- ↘︎ MySQL 서버가 실행 중인지 확인합니다.
```bash
ps aux | grep mysql
```

- ↘︎ 예상 결과:
```bash
_mysql           15308   0.1  1.3 412253456 449312 s001  S     8:23AM   0:02.13 /path/to/mysqld --basedir=/path/to/mysql --datadir=/path/to/mysql/data --plugin-dir=/path/to/mysql/plugin --user=mysql --log-error=/path/to/mysql/logs/mysql.err --pid-file=/path/to/mysql/data/mysql.pid
user1             6103   0.0  0.4 415725424 141392   ??  S     3:47AM   0:26.89 /path/to/IntelliJ IDEA.app/bin/java -Djava.rmi.server.hostname=127.0.0.1 -Duser.timezone=UTC ...
user1             15467   0.0  0.0 410733328   1632 s001  S+    8:32AM   0:00.00 grep --color=auto mysql
root             15224   0.0  0.0 410726688   2672 s001  S     8:23AM   0:00.01 /bin/sh /path/to/mysql/bin/mysqld_safe --datadir=/path/to/mysql/data --pid-file=/path/to/mysql/data/mysql.pid
```

### ✅1️⃣ MySQL 서버가 실행 중일 경우.
- ↘︎ 1. `kill -9 '프로세스의 PID'`
    - ↘︎ 예: `kill -9 15308`
        - ↘︎ 위와 같이 프로세스의 PID를 죽여 서버를 중지시킵니다.
- ↘︎ 2. `sudo pkill mysqld`
    - ↘︎ 위 명령어를 사용하여 모든 프로세스를 죽여 서버를 중지시킵니다.
- ↘︎ 3. `/path/to/mysql/support-files/mysql.server stop`
    - ↘︎ 위 명령어를 사용하여 mysql.server를 중지시킵니다.
- ↘︎ 4. 나만의 스크립트 사용.
    - ↘︎ 자신만의 스크립트가 있다면 그 스크립트를 사용하여 server를 중지시킵니다.

## ✅2️⃣ MySQL 서버 `--skip-grant-tables` 명령어로 시작하기.
- ↘︎ 서버를 `--skip-grant-tables` 옵션을 주어 시작합니다.
```bash
/path/to/mysql/support-files/mysql.server start --skip-grant-tables
```
- ↘︎ 위 옵션의 경우 `root` 인증 없이 MySQL 서버에 접근할 수 있도록 해줍니다.

## ✅3️⃣ root 계정에 접근 및 비밀번호를 `null`로 바꾸기.
### ✅1️⃣ root 계정에 접근하기.
- ↘︎ `mysql -u root` 명령어를 사용하여 `root` 계정에 접근합니다.
```bash
mysql -u root
```

### ✅2️⃣ 비밀번호를 `null`로 바꾸기.
- 📌 비밀번호를 `null`로 바꾸는 이유
    - ↘︎ `MySQL 8.0.X` 부터는 `ALTER USER`를 사용하여 비밀번호를 변경해야 합니다.
    - ↘︎ 그러나 `--skip-grant-tables` 옵션은 `ALTER USER` 가 실행되지 않으므로 비밀번호를 `null`로 바꾸고 빈 비밀번호에 정상 접근하여 비밀번호를 재설정하는 방법을 사용하기 위해 `null`로 일단 바꿉니다.

### ✅3️⃣ FLUSH PRIVILEGES.
- ↘︎ `FLUSH PRIVILEGES`를 사용하여 권한 설정을 해줍니다.

## ✅4️⃣ 서버 재시작 및 비밀번호 변경.
### ✅1️⃣ 서버 재시작
- ↘︎ `/path/to/mysql/support-files/mysql.server restart;`로 서버를 재시작 합니다.

### ✅2️⃣ 루트 권한으로 접근.
- ↘︎ `mysql -u root`를 사용하여 root 권한으로 접근합니다.

### ✅3️⃣ 비밀번호 변경.
- ↘︎ `ALTER USER 'root'@'localhost' identified with caching_sha2_password by 'new_password';`
    - ↘︎ 위와 같이 비밀번호를 변경합니다.

## ✅5️⃣ 확인하기.
- ↘︎ `mysql -u root -p`
    - ↘︎ 바뀐 비밀번호를 입력하고 잘 접속이 되는지 확인합니다.
