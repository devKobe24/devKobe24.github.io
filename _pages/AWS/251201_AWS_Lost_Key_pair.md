---
title: "☁️[AWS] Bastion Host Key Pair를 잃어버렸을 때 해결 방법 4가지 🤔"
tags:
  - AWS
  - Network
  - EC2
  - Security
date: "2025-12-01"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

SSH Key Pair를 분실했을 때 Bastion Host에 다시 접근하는 방법을 4가지로 정리했습니다.

---

## 방법 1. AWS Systems Manager Session Manager 사용 (권장)

**가장 쉽고 안전한 방법**으로, 브라우저 기반으로 EC2에 접속할 수 있습니다.

### 전제조건

- EC2에 **SSM Agent 설치**
- EC2 IAM Role에 **AmazonSSMManagedInstanceCore** 정책 연결
- EC2가 인터넷 또는 VPC Endpoint를 통해 SSM 서비스와 통신 가능

### 접속 방법

1. AWS Console → **Systems Manager** 이동
2. 좌측 메뉴에서 **Session Manager** 선택
3. **Start Session** 클릭
4. Bastion Host EC2 인스턴스 선택
5. 브라우저 기반 터미널로 접속 완료

### 새 SSH 키 등록

```bash
# SSH 디렉토리 생성
mkdir -p ~/.ssh
chmod 700 ~/.ssh

# 새 Public Key 추가
echo "새로운_Public_Key_내용" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

### 장점
- 키 없이 즉시 접속 가능
- 네트워크 방화벽 규칙 변경 불필요
- 접속 로그가 CloudTrail에 자동 기록

### 단점
- 사전에 SSM Agent 설치와 IAM 역할 설정 필요

---

## 방법 2. EBS 볼륨 분리 후 직접 수정 (가장 확실한 방법)

기존 EC2의 루트 볼륨을 다른 EC2에 연결하여 `authorized_keys` 파일을 직접 수정하는 방법입니다.

### 작업 단계

#### 1단계: Bastion Host EBS 볼륨 분리

1. AWS Console에서 Bastion Host 인스턴스 **중지(Stop)**
2. **Volumes** 메뉴로 이동
3. 루트 볼륨(보통 `/dev/xvda` 또는 `/dev/sda1`) 선택
4. **Actions → Detach Volume** 실행

#### 2단계: 다른 EC2 인스턴스에 볼륨 연결

1. **동일 가용 영역(AZ)**의 정상 작동하는 EC2 인스턴스 선택
2. 분리한 볼륨을 **Attach Volume**
3. Device 이름은 `/dev/xvdf` 등으로 지정

#### 3단계: 볼륨 마운트 및 키 수정

```bash
# 마운트 포인트 생성
sudo mkdir /mnt/bastion

# 볼륨 마운트 (파티션 번호 확인 필요)
sudo mount /dev/xvdf1 /mnt/bastion

# authorized_keys 파일 수정
sudo nano /mnt/bastion/home/ubuntu/.ssh/authorized_keys
# 또는 ec2-user, centos 등 실제 사용자명에 맞게 경로 조정

# 새 Public Key 추가 후 저장
```

#### 4단계: 볼륨 재연결

```bash
# 볼륨 언마운트
sudo umount /mnt/bastion
```

1. AWS Console에서 볼륨 **Detach**
2. Bastion Host에 원래 Device 이름(`/dev/xvda`)으로 **Attach**
3. Bastion Host 인스턴스 **시작(Start)**
4. 새 키로 SSH 접속 확인

### 장점
- SSM Agent나 추가 설정 없이 가능
- 100% 확실하게 키 등록 가능
- 루트 볼륨 직접 접근으로 다양한 문제 해결 가능

### 단점
- 인스턴스 중지로 인한 서비스 다운타임 발생
- 수동 작업이 많아 시간 소요

---

## 방법 3. EC2 Instance Connect 사용

Amazon Linux 2, Ubuntu 등 특정 AMI에서 사용 가능한 일회성 SSH 연결 방법입니다.

### 전제조건

- **Amazon Linux 2** 또는 **Ubuntu** 기반 인스턴스
- 보안 그룹에서 **22번 포트(SSH)** 허용
- IAM 권한에 `ec2-instance-connect:SendSSHPublicKey` 포함

### 접속 방법

1. AWS Console → EC2 인스턴스 선택
2. **Connect** 버튼 클릭
3. **EC2 Instance Connect** 탭 선택
4. **Connect** 클릭하면 브라우저에서 터미널 실행

AWS가 임시 Public Key를 자동으로 전송하여 60초간 유효한 SSH 세션을 생성합니다.

### 영구 키 추가

```bash
# SSH 디렉토리 생성
mkdir -p ~/.ssh

# 새 Public Key 추가
echo "새로운_Public_Key_내용" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

### 장점
- 별도 설치 없이 지원되는 AMI에서 즉시 사용
- 간단한 웹 기반 접속

### 단점
- Amazon Linux 2, Ubuntu 등 특정 OS만 지원
- SSH 포트가 인터넷에 열려있어야 함

---

## 방법 4. AMI 생성 후 새 인스턴스로 교체

Bastion Host가 **상태 비저장(Stateless)** 서버라면 가장 깔끔한 해결 방법입니다.

### 작업 단계

1. 기존 Bastion Host에서 **AMI(Amazon Machine Image) 생성**
   - Actions → Image and templates → Create image
2. 새 AMI로 **새 인스턴스 시작**
   - 이번에는 **새 Key Pair** 선택
3. 기존 설정 복제
   - 동일한 보안 그룹 연결
   - Elastic IP 재할당 (필요시)
   - 동일한 Subnet 및 네트워크 구성
4. 새 인스턴스 테스트 후 기존 인스턴스 종료

### 장점
- 깔끔하게 새 인스턴스로 시작
- 복잡한 파일 수정 작업 불필요
- AMI로 백업도 동시에 진행

### 단점
- 새 인스턴스로 IP 변경 가능 (Elastic IP 미사용 시)
- Stateful한 데이터가 있다면 추가 마이그레이션 필요

---

## 상황별 권장 방법

| 상황 | 추천 방법 | 이유 |
|------|----------|------|
| SSM Agent 설치됨 | **방법 1 (Session Manager)** | 가장 빠르고 안전 |
| SSM 미설치, 다운타임 허용 | **방법 2 (EBS 분리)** | 가장 확실한 복구 |
| Amazon Linux/Ubuntu | **방법 3 (Instance Connect)** | 간편한 일회성 접속 |
| Stateless 서버 | **방법 4 (AMI 재생성)** | 깔끔한 재구축 |

---

## 예방 조치

향후 Key Pair 분실을 방지하기 위한 권장 사항:

1. **SSM Agent를 기본 설치**하여 Session Manager 활용
2. Key Pair를 **안전한 위치에 백업** (암호화된 저장소)
3. **여러 개의 Public Key** 등록 (팀원별 또는 백업용)
4. **IAM 역할 기반 접근**으로 키 의존도 감소
5. 중요 서버는 **정기적으로 AMI 백업** 생성

---

## 참고 자료

- [AWS Systems Manager Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)
- [EC2 Instance Connect](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Connect-using-EC2-Instance-Connect.html)
- [EBS Volume 관리](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volumes.html)
