---
title: "☁️[AWS] Amazon Linux 2에 Java8 설치하는 방법."
tags:
    - AWS
    - Network
date: "2024-08-04"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# 1️⃣ EC2 인스턴스에 로그인.
- SSH를 통해 EC2 인스턴스에 로그인 합니다.
```shell
ssh -i "your-key.pem" ec2-user@your-instance-ip
// 또는
ssh "config에 등록한 서비스명" 
```

# 2️⃣ 업데이트 및 패키지 매니저 설치 확인.
- 인스턴스의 패키지 리스트를 업데이트하고 , **`yum`** 패키지 매니저가 최신 상태인지 확인합니다.
```shell
sudo yum update -y
```

# 3️⃣ OpenJDK 8 설치.
- OpenJDK 8은 Amazon Linux 2의 기본 레포지토리에서 사용할 수 있습니다.
    - 다음 명령어를 사용하여 설치할 수 있습니다.
```shell
sudo yum install -y java-1.8.0-openjdk
```

# 4️⃣ 설치된 Java 버전 확인.
- 설치가 성공적으로 완료되었는지 확인하기 위해 Java 버전을 확인합니다.
```java
java -version
```
