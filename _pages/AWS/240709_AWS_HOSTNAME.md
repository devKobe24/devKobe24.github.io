---
title: "☁️[AWS] HOSTNAME 바꾸는 방법"
tags:
    - AWS
    - Network
date: "2024-07-09"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# EC2 인스턴스의 호스트 이름을 바꾸는 방법.

🙋‍♂️ 모든 과정은 EC2에 접속 후에 이루어집니다.

## 1️⃣ 호스트 이름 설정.
**`hostnamectl`** 명령어를 사용하여 호스트 이름을 설정합니다.

```shell
sudo hostnamectl set-hostname {my-new-hostname}
```

## 2️⃣ `/etc/sysconfig/network` 파일 수정.
**`/etc/sysconfig/network`** 파일에 새로운 호스트 이름을 추가합니다.

```shell
sudo vi /etc/sysconfig/network
````

**`HOSTNAME`** 항목을 추가하거나 수정합니다.

```
NETWORKING=yes
HOSTNAME={my-new-hostname}
````

## 3️⃣ `/etc/hosts` 파일 수정.
**`/etc/hosts`** 파일을 열어 호스트 이름을 추가합니다.

```shell
sudo vi /etc/hosts
```

파일 내용은 다음과 같이 수정합니다.

```
127.0.0.1 localhost localhost.localdomain
::1       localhost localhost.localdomain
127.0.0.1 {my-new-hostname}
````

## 4️⃣ 즉시 호스트 이름 변경.
호스트 이름을 즉시 적용합니다.

```shell
sudo hostname {my-new-hostname}
```

## 5️⃣ 셸 프롬프트 구성.
셸 프롬프트에 새로운 호스트 이름이 표시되도록 **`.bashrc`** 파일을 수정합니다.

```shell
vi ~/.bashrc
```

다음 줄을 추가하거나 수정합니다.

```shell
PS1='[\u@\h \W]\$ '
```

변경 사항을 적용합니다.

```shell
source ~/.bashrc
```

## 6️⃣ 인스턴스 재부팅

위의 모든 단계를 수행한 후 인스턴스를 재부팅합니다.

```shell
sudo reboot
```
