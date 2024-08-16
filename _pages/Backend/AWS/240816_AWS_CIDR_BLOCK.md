---
title: "☁️[AWS] CIDR(Classless Inter-Domain Routing) 블록으로 IP 주소 범위 결정."
tags:
    - AWS
    - Network
date: "2024-08-16"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] CIDR(Classless Inter-Domain Routing) 블록으로 IP 주소 범위 결정.
<img src ="https://github.com/devKobe24/images2/blob/main/AWS/aws-1-0.png?raw=true">
- 위 그림과 같이 IP 주소를 이용해 네트워크 범위를 정의하는 것을 **CIDR(Classless Inter-Domain Routing) 블록** 이라고 합니다.
- IP 주소를 나타내는 숫자열은 크게 **네트워크** 부분과 **호스트** 부분으로 나눌 수 있습니다.
    - 네트워크 부분은 문자 그대로 네트워크를 정의합니다.
    - 호스트 부분은 그 네트워크 내의 호스트를 정의합니다.
        - 즉, 네트워크 안에서 접속할 수 있는 서버 등을 나타내는 부분입니다.
- 위 그림에서 '/16'을 서브넷 마스크라고 하며 IP 주소를 사용해 네트워크 범위를 정의하다면 보통 위 표기를 사용합니다.
