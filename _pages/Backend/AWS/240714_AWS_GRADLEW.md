---
title: "☁️[AWS] .gradlew 빌드 실패시 확인해야 할 것들"
tags:
    - AWS
    - Network
date: "2024-07-14"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# 1️⃣ Amazon Corretto JDK 8이 올바르게 설치되었는지 확인하기.

```shell
$ls -l /usr/lib/jvm/ava-1.8.0-amazon-corretto.x86_64
```

위 명령어를 통해 내부를 들여다보니 디렉토리 안에는 **`jre`** 디렉토리만 있고, JDK 디렉토리 구조가 포함되어 있지 않아서 **`JAVA_HOME`** 으로 설정할 수 없는 오류가 발생했었습니다.

이는 **`java-1.8.0-amazon-corretto-devel`** 패키지가 올바르게 설치되지 않았음을 의미할 수 있습니다.

## 🙋‍♂️ Amazon Corretto JDK 8 설치 및 확인.

먼저, Amazon Corretto JDK 8이 올바르게 설치되었는지 확인하고, 제대로 설치되지 않았다면 다시 설치합니다.

1. **기존 JDK 제거(필요시)**
```shell
sudo dnf remove -y java-1.8.0-amazon-corretto java-1.8.0-amazon-corretto-devel
```

2. **Amazon Corretto JDK 8 설치**
```shell
sudo dnf install -y java-1.8.0-amazon-corretto-devel
```

3. 설치된 디렉토리 확인.
```shell
ls -l /usr/lib/jvm/
```

4. JDK 디렉토리 구조 확인.
- JDK 디렉토리 구조가 포함되어 있는지 확인합니다.
```shell
ls -l /usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64/
```

## 🙋‍♂️ 올바른 JAVA_HOME 설정 및 빌드
1. JAVA_HOME 및 PATH 설정
```shell
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64
export PATH=$JAVA_HOME/bin:$PATH
```

2. 설정확인
```shell
echo $JAVA_HOME
java -version
javac -version
```

3. Gradl 빌드 실행
```shell
./gradlew clean
./gradlew build
```
