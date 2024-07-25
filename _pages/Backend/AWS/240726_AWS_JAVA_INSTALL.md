---
title: "☁️[AWS] Amazon Linux 2023에 Java8 설치하는 방법."
tags:
    - AWS
    - Network
date: "2024-07-26"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# 1️⃣ 시스템 패키지 업데이트
- 먼저 시스템 패키지를 업데이트 합니다.

```shell
sudo dnf update -y
```

# 2️⃣ Amazon Corretto 8 저장소 추가.
- Amazon Corretto는 Amazon에서 제공하는 무료, 멀티플랫폼, 생산성 사용 준비가 된 OpenJDK 배포판입니다.
    - Correttio 8은 Java 8과 호환됩니다.

```shell
sudo dnf install -y java-1.8.0-amazon-corretto
```

# 3️⃣ Java 8 설치 확인
- Java 8이 설치되었는지 확인하려면 다음 명령어를 사용합니다.

```shell
java -version
```

- 이 명령어를 실행하면 Java 버전 정보가 출력됩니다. **`openjdk version "1.8.0_xxx"`** 와 같이 Java 8이 설치된 것을 확인할 수 있습니다.

# 4️⃣ 기본 Java 버전 설정

- 시스템에 여러 버전의 Java가 설치되어 있을 수 있습니다.
    - 기본으로 사용할 Java 버전을 설정하려면 **`alternatives`** 명령어를 사용합니다.
- 먼저 현재 사용 가능한 Java 버전을 확인합니다.

```shell
sudo alternatives --config java
```
