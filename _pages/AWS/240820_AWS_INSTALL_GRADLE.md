---
title: "☁️[AWS] Amazon Linux CLI에 Gradle 설치하는 방법."
tags:
    - AWS
    - Network
date: "2024-08-20"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] Amazon Linux CLI에 Gradle 설치하는 방법.
- 이 포스트에서는 수동 설치(바이너리 다운로드) 방법을 소개합니다.

## 1️⃣ 필요한 도구 설치.
- Gradle을 수동으로 설치하기 전에 **`'wget'`** 이나 **`'unzip'`** 과 같은 도구가 필요합니다.
```bash
sudo yum install wget unzip -y
```

## 2️⃣ Gradle 바이너리 다운로드.
- Gradle의 최신 릴리스 버전을 다운로드 합니다.
    - 최신 버전은 [Gradle 공식 웹사이트](https://gradle.org/)에서 확인할 수 있습니다.
> 🙋‍♂️ 아래 명령어는 버전 8.10을 예로 들어 설명합니다.
> 필요에 따라 최신 버전을 사용하세요.

```bash
wget https://services.gradle.org/distributions/gradle-8.10-bin.zip -P /tmp
```

## 3️⃣ 다운로드한 파일 압축 해제.
- 다운로드한 Gradle 바이너리를 **`'/opt/gradle'`** 디렉토리에 압축 해제합니다.
```bash
sudo unzip -d /opt/gradle /tmp/gradle-8.10-bin.zip
```

## 4️⃣ Gradle 경로 설정.
- Gradle을 시스템 경로에 추가하기 위해 심볼릭 링크를 설정하거나 환경 변수를 설정합니다.
```bash
sudo ln -s /opt/gradle/gradle-8.10 /opt/gradle/latest
echo 'export PATH=$PATH:/opt/gradle/latest/bin' >> ~/.bash_profile
source ~/.bash_profile
```

## 5️⃣ 설치 확인.
- Gradle이 제대로 설치되었는지 확인하려면 다음 명령어를 사용합니다.
```bash
gradle -v
```

<img src = "https://github.com/devKobe24/images2/blob/main/AWS/AWS-gradle-v.png?raw=true">
