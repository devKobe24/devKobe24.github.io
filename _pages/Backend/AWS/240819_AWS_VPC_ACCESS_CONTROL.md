---
title: "☁️[AWS] VPC 접근 제어 및 통신 로그 확인."
tags:
    - AWS
    - Network
date: "2024-08-19"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] VPC 접근 제어 및 통신 로그 확인.

## 1️⃣ VPC 접근 제어 및 통신 로그 확인.
- VPC에는 서브넷 단위로 접근 제어를 설정할 수 있는 **네트워크 접근 제어 목록(이후 네트워크 ACL)** 이라는 기능이 있습니다.
    - **보안 그룹** 과 조합해 접근 제어 설정이 가능합니다.
- 네트워크 ACL과 보안 그룹과의 차이는 다음 표와 같습니다.
<img src = "https://github.com/devKobe24/images2/blob/main/AWS/AWS-ACL-SG.png?raw=true">
- 기본 설정 상태에서 네트워크 ACL은 모든 통신을 허용합니다.
    - 서브넷 전체에서 네트워크 접근을 허용하거나 거부하려는 경우 보안 그룹에서 추가 설정을 해 더욱 안전한 네트워크를 구축할 수 있습니다.
<img src = "https://github.com/devKobe24/images2/blob/main/AWS/AWS-ACL.png?raw=true">
- 네트워크 ACL이나 보안 그룹에서 허용되거나 거부된 통신 상황은 **VPC 흐름 로그** 라고 하는 VPC 내의 IP 트래픽 상황을 로그로 저장할 수 있는 기능을 사용해 확인할 수 있습니다.
    - 로그는 AWS 모니터링 서비스인 CloudWatch Logs 또는 S3에 저장할 수 있습니다.
        - CloudWatch Logs에 저장하면 로그 내용에 따라 메일 알림을 보낼 수 있게 구성해 감시할 수 있습니다. 요금은 S3가 저렴합니다.
- EC2와 같은 VPC의 자원은 IP 주소마다 **네트워크 인터페이스(ENI, Elastic Network Interface)** 를 가집니다.
    - 로그는 네트워크 인터페이스별로 출력되며 Elastic Load Balancing, RDS, Redshift와 같이 VPC에서 실행되는 모든 서비스 로그도 출력됩니다.
        - 도착지/출발지의 IP 주소와 포트, 전송 허용/거부 등의 정보가 포함됩니다.
<img src = "https://github.com/devKobe24/images2/blob/main/AWS/AWS-VPC-LOG.png?raw=true">
