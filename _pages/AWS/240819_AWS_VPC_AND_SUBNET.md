---
title: "☁️[AWS] VPC 및 서브넷 생성."
tags:
    - AWS
    - Network
date: "2024-08-19"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] VPC 및 서브넷 생성.
- VPC는 EC2 및 기타 AWS 서비스와 마찬가지로 AWS Management Console(GUI) 또는 API를 사용해 생성할 수 있습니다.
    - 관리 콘솔에서 생성하는 경우 화면에서 다음 항목을 설정해야 합니다.
        - VPC 이름(Name 태그)
        - CIDR 블록
        - IPv6 설정
        - 테넌시(전용 하드웨어 사용 여부)
- EC2보다 적은 설정 항목으로 즉시 가상 네트워크를 만들 수 있습니다.
    - IPv6 설정은 기본적으로 비활성화돼 있으며 필요에 따라 설정할 수 있습니다.
    - 테넌시는 라이선스 및 보안 요구 사항으로 하드웨어를 독점하려는 경우에만 독점 옵션을 지정합니다.
<img src = "https://github.com/devKobe24/images2/blob/main/AWS/AWS_GUI.png?raw=true">
- VPC만으로는 EC2와 같은 자원을 네트워크에 만들 수 없습니다.
    - **VPC 안에 더 작은 네트워크 단위인 서브넷을 만들어야 합니다.**
- 서브넷은 하나의 AZ(Availablity Zone, 가용 영역)에 걸쳐 있을 수 없습니다.
    - 즉, 여러 AZ에 자원을 배치해 가용성을 높이려면 서브넷도 여러 개 만들어야 합니다.
- 서브넷에는 생성할 VPC의 CIDR 블록 범위 내에서 CIDR 블록을 지정해야 합니다.
    - 예를 들어 VPC의 CIDR 블록이 10.0.0.0/16인 경우 10.0.0.0/24와 같이 CIDR 블록을 지정합니다.
<img src = "https://github.com/devKobe24/images2/blob/main/AWS/AWS-SUBNET.png?raw=true">
