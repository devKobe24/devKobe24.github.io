---
title: "☁️[AWS] Amazon Linux 2023 플랫폼에 MySQL 설치하는 방법."
tags:
    - AWS
    - Network
date: "2024-07-10"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

Amazon Linux 2023 플랫폼에 MySQL이 기본적으로 설정되어 있지 않습니다.

그러므로 따로 설치해줘야 합니다.

기본적인 방법은 아래와 같습니다.

# 1️⃣ RPM 파일 다운로드.
```shell
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm
```

위 명령어를 사용하여 RPM 파일을 다운로드 받아줍니다.

> 🙋‍♂️ 모든 파일의 설정은 ec2 cli에서 설정해야 합니다.

# 2️⃣ GPC 퍼블릭 키 설정
```shell
sudo dnf install mysql80-community-release-el9-1.noarch.rpm -y
```

mysql 설치를 위해 퍼블릭 키를 import하는 과정입니다.

```shell
sudo dnf update -y
```

# 3️⃣ MySQL 설치.

이 부분은 mysql-client와 mysql-server로 나뉩니다.

먼저 mysql-client 설치부터 합니다.

## 1️⃣ mysql-client 설치.
```shell
sudo dnf install mysql-community-client -y
```

이후에 mysql-server를 설치합니다.

## 2️⃣ mysql-server 설치.
```shell
sudo dnf install mysql-community-server -y
```
