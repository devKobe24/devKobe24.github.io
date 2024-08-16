---
title: "☁️[AWS] 퍼블릭 IP 주소와 프라이빗 IP 주소"
tags:
    - AWS
    - Network
date: "2024-08-16"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] 퍼블릭 IP 주소와 프라이빗 IP 주소.
- IP 주소는 두 가지로 구분할 수 있습니다.
<img src = "https://github.com/devKobe24/images2/blob/main/AWS/aws-8.png?raw=true">
- 하나는 한국 또는 전 세계에서 '이 주소는 인터넷에서 이 곳'이라고 특정할 수 있는 주소입니다.
    - 이를 **퍼블릭 IP 주소** 또는 **글로벌 IP 주소(공인 IP)** 라고 합니다.
<img src = "https://github.com/devKobe24/images2/blob/main/AWS/aws-9.png?raw=true">
- 다른 하나는 **프라이빗 IP 주소(사설 IP)** 입니다.
    - 퍼블릭 IP 주소는 전 세계에서 식별할 수 있는 주소지만, 프라이빗 주소는 닫힌 네트워크(폐쇄망, 내부 네트워크, 근거리 통신-LAN) 내에서만 식별할 수 있는 IP 주소입니다.
- 프라이빗 IP 주소로 사용할 수 있는 범위는 다음과 같습니다.
    - 10.0.0.0 ~ 10.255.255.255(10.0.0.0/8)
    - 176.16.0.0 ~ 172.31.255.255(176.16.0.0/12)
    - 192.168.0.0 ~ 192.168.255.255(192.168.0.0/16)
        - 퍼블릭 IP 주소는 이 주소를 제외한 나머지 주소입니다.
