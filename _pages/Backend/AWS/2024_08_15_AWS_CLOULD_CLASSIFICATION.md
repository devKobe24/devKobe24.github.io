---
title: "☁️[AWS] 서비스 제공 형태에 따른 클라우드 분류."
tags:
    - AWS
    - Network
date: "2024-08-15"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] 서비스 제공 형태에 따른 클라우드 분류.
- 클라우드 서비스는 제공하는 서비스에 따라 **SasS, PaaS, IaaS** 로 나눌 수 있습니다.
- SaaS(Software as a Service)
    - 응용 프로그램을 서비스로 제공하는 형태입니다.
    - 많은 사람이 사용하는 Gmail, Dropbox, Office365, Zoom이 대표적인 SaaS 입니다.
- PaaS(Platform as a Service), IaaS(Infrastructure as a Service)
    - 응용 프로그램을 만들기 위한 기능을 서비스로 제공합니다.
    - 이 서비스는 직접 응용 프로그램을 개발하는 사용자를 위한 서비스로, 사용자는 제공 받은 기능을 조합해 응용 프로그램을 개발합니다.
- PaaS와 IaaS의 차이는 클라우드 서비스 제공자가 관리하는 범위입니다.
    - PaaS
        - 클라우드 서비스 제공자는 OS 및 미들웨어까지 관리하고, 필수 기능만 사용자에게 제공합니다.
        - AWS에서 **관리형 서비스**로 제공하는 RDS나 DynamoDB, Lambda 등이 여기에 해당합니다.
        - 유지보수는 AWS가 담당하며 사용자는 AWS에서 제공하는 범위 안에서 자유롭게 기능을 이용할 수 있습니다.
    - IaaS
        - 서버 및 네트워크 기능만 제공하며 설정과 관리는 사용자의 몫입니다.
        - AWS의 EC2와 VPC, EBS와 같이 사용자가 자유롭게 설정할 수 있는 서비스가 IaaS에 해당합니다.
<img src = "https://github.com/devKobe24/images2/blob/main/AWS/aws-5.png?raw=true">
