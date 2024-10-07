---
title: "☁️[AWS] NAT 게이트웨이."
tags:
    - AWS
    - Network
date: "2024-08-19"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] NAT 게이트웨이.

## 1️⃣ NAT 게이트웨이.
- **"프라이빗 서브넷에 있는 EC2와 같은 자원은 일반적으로 인터넷과 통신할 수 없습니다."**
    - 하지만 인터넷에 있는 소프트웨어 패키지의 다운로드와 같이 VPC 내에서 인터넷과 통신해야 하는 경우가 있습니다.
        - 이때 사용할 수 있는 것이 **NAT 게이트웨이** 입니다.
- 인터넷 게이트웨이와 달리 인터넷에서 VPC로 통신할 수 없는 단방향 통신이므로 AWS 외부와 더 안전하게 통신할 수 있습니다.
- NAT는 **Network Address Translation** 의 약자로 **프라이빗 IP 주소를 퍼블릭 IP 주소로 변환** 하는 것을 의미합니다.
- **"NAT 게이트웨이는 프라이빗 서브넷의 IP 주소를 NAT 게이트웨이의 퍼블릭 IP로 변환해 인터넷과 통신할 수 있게 해줍니다."**
- NAT 게이트웨이를 이용해 통신하기 위해서는 외부 통신을 수행하는 서브넷의 라우팅 테이블에 경로 정보(라우팅 정보)를 등록해야 합니다.
<img src = "https://github.com/devKobe24/images2/blob/main/AWS/AWS-NAT.png?raw=true">
