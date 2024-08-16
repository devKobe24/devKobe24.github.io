---
title: "☁️[AWS] 라우팅 및 라우팅 테이블."
tags:
    - AWS
    - Network
date: "2024-08-16"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] 라우팅 및 라우팅 테이블.
- 실제 세상의 어떤 주소를 찾아가기 위해서는 그 주소까지의 경로를 알아야 합니다.
- 네트워크 세계에서도 이처럼 어떤 IP 주소를 찾아가기 위해서는 해당 주소까지의 경로를 알아야 합니다.
    - 하지만 사용자는 이런 경로를 알 필요가 없습니다.
        - 대신 **라우터(Router)** 가 최적의 경로를 찾아서 경로를 결정하고 연결합니다.
            - 라우터가 IP 주소까지의 경로를 결정하는 것을 **라우팅(Routing)** 이라고 합니다.
- 인터넷을 통해 원하는 사이트까지 가는 경로는 여러 라우터를 거치게 되므로 어떻게 라우팅하는 것이 효율이 좋을지 결정해야 합니다.
<img src = "https://github.com/devKobe24/images2/blob/main/AWS/aws-1-3.png?raw=true">
- 각 라우터는 소유한 경로 정보를 기반으로 목적지 IP 주소를 향해 이동해야 하는 네트워크를 결정합니다.
    - 이 경로 정보를 **라우팅 테이블(Routing Table)** 이라고 합니다.
        - 라우팅 테이블은 라우터가 소유한 네트워크의 지도와 같은 것입니다.
            - AWS에서는 Amazon VPC의 **라우팅 테이블(Routing Table)** 이라는 기능이 이에 해당합니다.
                - 라우팅 테이블에는 인터넷이나 각 AWS 서비스에 대해 어떤 엔드포인트를 경유할지와 같은 경로 정보를 관리할 수 있습니다.
<img src ="https://github.com/devKobe24/images2/blob/main/AWS/aws-1-4.png?raw=true">
