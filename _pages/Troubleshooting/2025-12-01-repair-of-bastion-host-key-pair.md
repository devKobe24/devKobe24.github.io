---
title: "🔍[Troubleshooting] 🚀 Bastion Host의 Key Pair 복구하기"
tags:
  - Troubleshooting
  - Backend Development
  - AWS
date: "2025-12-01"
thumbnail: "/assets/img/thumbnail/troubleshooting.jpg"
---

# 🚀 Bastion Host의 Key Pair 복구하기

## 📋 문제 상황

**현재 조건:**
- Ubuntu 24.04 사용
- SSM IAM Role 없음 → SSM Session Manager 접속 불가능
- Status Check 정상 (EC2 콘솔에서 3/3 검사 통과)
- **Key Pair 분실** → 기존 방식으로 SSH 접속 불가

**해결 방법:**
> EBS 볼륨을 분리하여 다른 EC2에 연결한 후 `authorized_keys`를 수정하는 방식이 유일하고 확실한 방법입니다.

---

## ✅ 복구 프로세스

### 1️⃣ Bastion Host 중지

AWS Console → EC2 → Bastion Host 선택 → **Stop**

> ⚠️ **주의:** Terminate가 아닌 Stop을 선택하세요!

---

### 2️⃣ 루트 볼륨(EBS) 분리

1. EC2 상세 화면 → **Storage** 탭
2. Root volume 선택 → **Actions** → **Detach Volume**
3. Detach 완료 대기 (약 5~10초)

---

### 3️⃣ 임시 EC2 인스턴스 생성

**생성 조건:**
- **동일한 AZ에 생성** (Attach를 위해 필수)
- 인스턴스 타입: `t3.micro` (Ubuntu 24.04)
- 새로운 Key Pair 생성 및 다운로드

> 💡 이 EC2는 볼륨 파일 수정을 위한 임시 작업용입니다.

---

### 4️⃣ 분리한 볼륨을 임시 EC2에 연결

1. **Volumes** → Detached 상태의 EBS 선택
2. **Actions** → **Attach Volume**
3. Instance: 임시 EC2 선택
4. Device name: `/dev/sdf` (자동 설정)

---

### 5️⃣ 임시 EC2에 SSH 접속 및 볼륨 마운트

**① 볼륨 연결 확인**
```bash
lsblk
```

출력 예시:
```
nvme0n1      → 임시 EC2의 루트 볼륨
nvme1n1      → Bastion Host에서 가져온 볼륨
├─nvme1n1p1  → Bastion Host의 루트 파티션 (마운트 대상)
```

**② 마운트 디렉토리 생성**
```bash
sudo mkdir -p /mnt/bastion
```

**③ Bastion Host 루트 파티션 마운트**
```bash
sudo mount /dev/nvme1n1p1 /mnt/bastion
```

> 📌 장치 이름은 `lsblk` 결과에 따라 `/dev/xvdf1` 또는 `/dev/nvme1n1p1`로 다를 수 있습니다.

---

### 6️⃣ authorized_keys 파일 수정

**① SSH 디렉토리 확인**

Bastion Host의 사용자가 `ubuntu`인 경우:
```bash
ls -al /mnt/bastion/home/ubuntu/.ssh
```

**② authorized_keys 파일 열기**
```bash
sudo nano /mnt/bastion/home/ubuntu/.ssh/authorized_keys
```

**③ 새로운 Public Key 추가**

파일 맨 아래에 새로운 Public Key를 추가합니다.

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC... your_email@example.com
```

- **CTRL + O** → 저장
- **CTRL + X** → 종료

**④ 권한 설정 (필수)**
```bash
sudo chown ubuntu:ubuntu /mnt/bastion/home/ubuntu/.ssh/authorized_keys
sudo chmod 600 /mnt/bastion/home/ubuntu/.ssh/authorized_keys
sudo chmod 700 /mnt/bastion/home/ubuntu/.ssh
```

> ⚠️ **중요:** `authorized_keys`는 반드시 600 권한이어야 SSH가 작동합니다.

---

### 7️⃣ 볼륨 분리 및 재연결

**① 마운트 해제**
```bash
sudo umount /mnt/bastion
```

**② AWS Console에서 볼륨 재연결**
1. 임시 EC2에서 **Detach Volume**
2. Bastion Host에 **Attach Volume**
   - Device: `/dev/sda1` (자동 설정)

---

### 8️⃣ Bastion Host 시작 및 SSH 접속 테스트

**① Bastion Host 시작**
```
AWS Console → Bastion Host → Start
```

**② SSH 접속**
```bash
ssh -i ~/.ssh/your_new_key.pem ubuntu@<Bastion-Host-Public-IP>
```

🎉 **복구 완료!**

---

## 🔥 FAQ: .pem 파일과 Public Key

### Q. "제가 생성한 키페어는 .pem 파일인데, authorized_keys에 무엇을 넣어야 하나요?"

**A. .pem 파일은 Private Key이므로, Public Key를 추출해서 넣어야 합니다.**

#### 🔐 이해하기

| 항목 | 설명 |
|------|------|
| `.pem` 파일 | Private Key (개인키) - 절대 서버에 넣으면 안 됨 |
| `authorized_keys` | Public Key (공개키)만 저장하는 곳 |

#### 📝 .pem에서 Public Key 추출 방법

**로컬 터미널에서 실행:**
```bash
ssh-keygen -y -f your_key.pem > your_key.pub
```

**예시:**
```bash
ssh-keygen -y -f bastion.pem > bastion.pub
```

**Public Key 확인:**
```bash
cat bastion.pub
```

출력 예시:
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC... 
```

이 내용을 `authorized_keys`에 추가하면 됩니다.

#### 🎯 최종 SSH 접속

```bash
ssh -i bastion.pem ubuntu@<Bastion-Host-Public-IP>
```

---

## 📌 체크리스트

복구 과정을 완료했다면 아래 항목을 확인하세요:

- [ ] Bastion Host가 Stop 상태인가? (Terminate 아님)
- [ ] 임시 EC2가 **동일한 AZ**에 있는가?
- [ ] 볼륨이 제대로 마운트되었는가? (`lsblk`로 확인)
- [ ] Public Key를 `authorized_keys`에 추가했는가?
- [ ] 권한이 올바른가? (파일: 600, 디렉토리: 700)
- [ ] 마운트를 해제했는가? (`umount`)
- [ ] 볼륨이 Bastion Host에 `/dev/sda1`로 재연결되었는가?
- [ ] Bastion Host가 시작되었는가?
- [ ] SSH 접속이 성공했는가?

---

## 🚨 주의사항

1. **절대 Private Key(.pem)를 서버에 올리지 마세요**
   - 보안상 심각한 위험입니다.

2. **authorized_keys 권한은 반드시 600**
   - 그렇지 않으면 SSH가 거부됩니다.

3. **볼륨 Detach/Attach 시 반드시 동일한 AZ 확인**
   - 다른 AZ의 인스턴스에는 연결할 수 없습니다.

4. **임시 EC2는 작업 완료 후 삭제 가능**
   - 비용 절감을 위해 작업 완료 후 Terminate하세요.

---

## 🎓 정리

이 방법은 SSM Session Manager를 사용할 수 없고, 기존 Key Pair를 분실했을 때 **100% 확실하게 복구할 수 있는 방법**입니다. 

핵심은 **EBS 볼륨을 분리하여 다른 EC2에서 파일 시스템에 접근하고, `authorized_keys`를 직접 수정하는 것**입니다.

복구 후에는 **새로운 Key Pair를 안전하게 보관**하고, 가능하다면 **SSM Session Manager를 위한 IAM Role을 추가**하여 향후 유사한 상황을 예방하는 것이 좋습니다.
