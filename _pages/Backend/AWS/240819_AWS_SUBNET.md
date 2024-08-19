---
title: "☁️[AWS] 서브넷?"
tags:
    - AWS
    - Network
date: "2024-08-19"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] 서브넷(Subnet).
- AWS애서 서브넷(Subnet)은 VPC(Virtual Private Cloud) 내에서 네트워크를 세분화하고 리소스를 논리적으로 분리하는 방법입니다.
- 서브넷은 VPC 내의 특정한 IP 주소 범위를 갖는 네트워크 세그먼트를 나타내며, AWS에서 네트워크 인프라를 구축하고 관리하는 데 중요하는 역할을 합니다.

## 1️⃣ VPC와 서브넷의 관계.
- **VPC(Virtual Private Cloud)**
    - AWS에서 제공하는 가상 네트워크로, 사용자가 자신의 클라우드 리소스를 배치하고 관리할 수 있는 격리된 네트워크 환경입니다.
    - VPC는 사용자가 직접 IP 주소 범위, 서브넷, 라우팅 테이블, 네트워크 게이트웨이 등을 설정할 수 있도록 합니다.
- **서브넷(Subnet)**
    - 서브넷은 VPC 내에서 특정 IP 주소 범위를 갖는 작은 네트워크 세그먼트입니다.
        - **"각 서브넷은 VPC의 전체 IP 주소 범위 내에서 특정한 부분을 할당받아 사용합니다."**

## 2️⃣ 서브넷의 유형.
- **퍼블릭 서브넷(Public Subnet)**
    - 인터넷 게이트웨이(Internet Gateway)에 연결된 서브넷으로, 이 서브넷에 배치된 리소스(예: EC2 인스턴스)는 퍼블릭 IP 주소를 가지며 인터넷과 직접 통시할 수 있습니다.
        - 웹 서버와 같은 인터넷과 직접 연결이 필요한 리소스를 퍼블릭 서브넷에 배치합니다.
- **프라이빗 서브넷(Private Subnet)**
    - **인터넷 게이트웨이에 연결되지 않은 서브넷으로, 이 서브넷에 배치된 리소스는 퍼블릭 IP 주소가 없으며, 직접적으로 인터넷과 통신할 수 없습니다.**
        - 데이터베이스 서버와 같이 인터넷과 직접 연결될 필요가 없는 리소스를 프라이빗 서브넷에 배치합니다.
            - 인터넷에 접속해야 할 경우, **NAT 게이트웨이 또는 NAT 인스턴스** 를 통해 간접적으로 인터넷에 접근할 수 있습니다.

## 3️⃣ 서브넷의 가용 영역(Availability Zone).
- 각 서브넷은 특정 가용 영역(AZ)에 속합니다.
    - 이는 가용 영역마다 독립적인 네트워크 세그먼트를 구성할 수 있게 해줍니다.
        - 예를 들어, VPC 내에 여러 가용 영역이 있을 때, 각각의 가용 영역에 서브넷을 만들어 고가용성 아키텍처를 구축할 수 있습니다.

## 4️⃣ 라우팅 테이블과 서브넷.
- 각 서브넷은 하나의 라우팅 테이블(Routing Table)과 연결됩니다.
    - **라우팅 테이블은 네트워크 트래픽이 어떻게 라우팅되는지를 결정합니다.**
        - 예를 들어, 퍼블릭 서브넷은 라우팅 테이블에 인터넷 게이트웨이로 가는 경로가 설정되어 있지만, 프라이빗 서브넷은 그런 경로가 없습니다.

## 5️⃣ 서브넷의 보안 그룹 및 네트워크 ACL.
- **보안 그룹(Security Groups)**
    - 서브넷에 배치된 리소스(예: EC2 인스턴스)에 대한 인바운드 및 아웃바운드 트래픽을 제어하는 가상 방화벽입니다.
    - 보안 그룹은 상태 기반으로 동작하며, 인스턴스 수준에서 적용됩니다.
- **네트워크 ACL(Network Access Control List)**
    - 서브넷 수준에서 적용되는 보안 레이어로, 서브넷 내의 모든 리소스에 대한 트래픽을 제어합니다.
    - 네트웨크 ACL은 상태 비저장(State-less)으로, 각각의 요청과 응답을 별도로 처리합니다.

## 6️⃣ 서브넷의 사용 사례.
- **웹 서버 및 애플리케이션 서버 배치**
    - 퍼블릭 서브넷에 배치하여 외부에서 접근 가능한 웹 서버와 애플리케이션 서버를 운영합니다.
- **데이터베이스 서버 및 내부 애플리케이션 배치**
    - 프라이빗 서브넷에 배치하여 외부 접근이 제한된 안전한 네트워크 환경에서 데이터베이스 서버나 비공개 애플리케이션을 운영합니다.
- **멀티 AZ 아키텍처**
    - 각 가용 영역에 서브넷을 만들어 장애가 발생해도 시스템이 지속적으로 운영될 수 있는 고가용성 아키텍처를 구축합니다.

## 7️⃣ 서브넷과 CIDR 블록.
- 각 서브넷은 CIDR(Classless Inter-Domain Routing) 블록으로 정의된 IP 주소 범위를 가집니다.
    - 예를 들어 **`'10.0.1.0/24'`** 와 같은 형태의 CIDR 블록이 서브넷의 IP 주소 범위를 정의합니다.
        - 이 CIDR 블록에 따라 서브넷 내에서 사용 가능한 IP 주소의 범위가 결정됩니다.

> 🎯 서브넷을 잘 설계하고 구성하는 것은 AWS에서 네트워크 인프라를 최적화하고 보안을 강화하는 데 매우 중요합니다.