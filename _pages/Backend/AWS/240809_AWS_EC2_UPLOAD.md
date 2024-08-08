---
title: "☁️[AWS] 로컬 파일을 EC2 서버에 올리는 방법."
tags:
    - AWS
    - Network
date: "2024-08-09"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] 로컬 파일을 EC2 서버에 올리는 방법.
**"저의 상황을 예로 들어 설명하도록 하겠습니다."**

- 로컬 macBook에서 Amazon Linux 2023 EC2 서버로 파일을 업로드하는 방법 중 가장 일반적인 방법은 **`scp(secure copy)`** 를 사용하는 것 입니다.
    - 이 명령을 통해 안전하게 파일을 복사할 수 있습니다.

> 🙋‍♂️ 아래 설명하는 단계들은 로컬에서 이루어져야 합니다.

## 1️⃣ EC2 인스턴스 연결 정보 확인.
- 먼저 EC2 인스턴스의 퍼블릭 IP 주소와 연결에 사용하는 키 페어 파일(.pem)을 확인합니다.

## 2️⃣ 로컬 macBook에서 `scp` 명령어 사용.
- **`scp`** 명령어를 사용하여 로컬 파일을 EC2 인스턴스로 업로드합니다.
    - 예를 들어, 로컬 파일 **`localfile.txt`** 를 EC2 인스턴스의 **`/home/ec2-user/`** 디렉토리에 업로드하려면 다음과 같이 합니다.

```bash
scp -i /path/to/your-key-pair.pem /path/to/localfile.txt ec2-uesr@<EC2-Instance-Public-IP>:/home/ec2-user/
```

### 👉 예제.
- 키 페어 파일 경로 : **`/Users/bingGu/.ssh/my-key-pair.pem`**
- 로컬 파일 경로 : **`/Users/bingGu/Documents/localfile.txt`**
- EC2 퍼블릭 IP : **`43.201.230.99`**

```bash
/Users/bingGu/Documents/localfile.txt ec2-user@43.201.230.99:/home/ec2-user/
```

## 3️⃣ 디렉토리 업로드.
- 만약 디렉토리를 업로드 하고 싶다면 **`-r`** 옵션을 사용하여 디렉토리를 재귀적으로 업로드할 수 있습니다.
    - 예를 들어, 로컬 디렉토리 **`localdir`** 을 EC2 인스턴스의 **`/home/ec2-user/`** 디렉토리에 업로드하려면 다음과 같이 합니다.

```bash
scp -r -i /path/to/your-key-pair.pem /path/to/localdir ec2-user@<EC2-Instance-Public-IP>:/home/ec2-user/
```

## 4️⃣ 업로드 확인.
- EC2 인스턴스에 SSH로 접속하여 파일이나 디렉토리가 정상적으로 업로드되었는지 확인합니다.

```bash
ssh -i /path/to/your-key-pair.pem ec2-user@<EC2-Instance-Public-IP>
ls /home/ec2-user/
```

## 🎯 Troubleshooting
- **Permission Denied :** 키 파일의 권한이 적절하지 않을 때 발생할 수 있습니다.
    - 다음 명령어로 권한을 조정합니다.
```bash
chmod 400 /path/to/your-key-pair.pem
```

- **Host Key Verification Failed :** 로컬 머신의 SSH 설정에서 이전에 연결한 적 없는 서버에 대해 경고가 뜨는 경우, 다음 명령어로 knows_hosts 파일을 업데이트할 수 있습니다.
```bash
ssh-keygen -R <EC2-Instance-Public-IP>
```

> 🙋‍♂️ 이 단계를 따르면 로컬 macBook에서 Amazon Linux 2023 EC2 인스턴스로 파일을 성골적으로 업로드할 수 있을 것입니다.
