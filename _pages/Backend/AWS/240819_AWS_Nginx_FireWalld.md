---
title: "☁️[AWS] Amazon Linux 2023에 Nginx 설치하는 방법."
tags:
    - AWS
    - Network
date: "2024-08-19"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] Amazon Linux 2023에 Nginx 설치하는 방법.

## 1️⃣ 패키지 목록 업데이트
- 먼저, 서버의 패키지 목록을 업데이트 합니다.
```bash
sudo dnf update -y
```

> 🎯 참고 dnf search {검색어}
> 예를 들어 dnf search java 라고 명령어를 실행하면 java라는 단어가 들어간 목록을 모두 보여줍니다.

## 2️⃣ Nginx 설치
- 다음 명령어로 Nginx를 설치합니다.
```bash
sudo dnf install nginx -y
```
- 위 명령어는 Amazon Linux 2023의 기본 패키지 저장소에서 Nginx를 다운로드하고 설치합니다.

## 3️⃣ Nginx 서비스 시작 및 활성화
- Nginx 설치가 완료되면, 다음 명령어를 사용하여 Nginx 서비스를 시작하고, 서버가 부팅될 때 자동으로 시작되도록 설정할 수 있습니다.
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

## 4️⃣ 방화벽 설정(선택 사항)
- 기본적으로 Amazon Linux 2023에는 firewalld가 설치되어 있지 않습니다.
    - 하지만 방화벽을 사용하는 경우 HTTP와 HTTPS 트래픽을 허용하도록 설정해야 합니다.

### 1️⃣ firewalld 설치.
- 먼저 **`'firewalld'`** 를 설치해야 합니다.
```bash
sudo dnf install firewalld -y
```

### 2️⃣ firewalld 서비스 시작 및 활성화
- 설치가 완료되면, **`'firewalld'`** 서비스를 시작하고 부팅 시 자동으로 시작되도록 설정합니다.
```bash
sudo systemctl start firewalld
sudo systemctl enable firewalld
```

### 3️⃣ HTTP 및 HTTPS 서비스 허용
- 이제 HTTP와 HTTPS 트래픽을 허용하도록 방화벽 규칙을 추가합니다.
```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
```

### 4️⃣ 방화벽 재시작
- 새로운 규칙이 적용되도록 방화벽을 재시작합니다.
```bash
sudo firewall-cmd --reload
```

### 5️⃣ 방화벽 상태 확인(선택 사항)
- 방화벽의 상태를 확인하려면 다음 명령어를 사용할 수 있습니다.
```bash
sudo firewall-cmd --list-all
```
