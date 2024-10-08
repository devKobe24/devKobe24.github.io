---
title: "☁️[AWS] 부하 분산을 위해 여러 서버에 접속을 분배"
tags:
    - AWS
    - Network
date: "2024-08-16"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] 부하 분산을 위해 여러 서버에 접속을 분배.
<img src = "https://github.com/devKobe24/images2/blob/main/AWS/aws-1-2.png?raw=true">
- 많은 사용자가 사용하는 시스템을 구축한다면 **부하 분산** 을 고려해야 합니다.
- 시스템을 사용하는 사용자가 많아질수록 서버의 부담은 커집니다.
- 서버 1대당 한 번에 처리할 수 있는 접속자 수는 정해져 있기 때문에 한 번에 많은 사람이 몰리면 CPU나 메모리의 사용량이 증가해 서버가 느려지고 최악의 경우 서버가 멈출 수도 있습니다.
    - 이는 서비스 사용자에게 큰 영향을 줍니다.
        - 이런 상황을 막기 위해 서버를 여러 대 구성해 각 서버가 처리를 나눠서 할 수 있게 구축해야 합니다.
            - 이렇게 부하를 분산시키기 위해서 일반적으로 **로드 밸런서(Load Balancer)** 라는 장치를 사용합니다.
