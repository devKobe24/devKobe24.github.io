---
title: "🌐[Network] 네트워크 세그먼트(Network Segment)."
tags:
    - Network
date: "2024-08-19"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐 네트워크 세그먼트(Network Segment).

## 1️⃣ 세그먼트(Segment).
네트워크 세그먼트(Network Segment)를 알아보기 전에 세그먼트(Segment)가 어떤 뜻을 가지고 있는지 궁금했습니다.
[TTA 정보통신용어사전](https://terms.tta.or.kr/dictionary/dictionaryView.do?word_seq=055119-2)에서는 다음과 같이 세그먼트를 정의하고 있었습니다.
- 1. 서로 구분되는 **기억 장치** 의 연속된 한 영역.
- 2. 어떤 프로그램이 너무 커서 한 번에 **주기억 장치** 에 올라올 수 없이 갈아넣기 기법을 사용하여 쪼개었을 때, 나뉜 각 부분을 가리키는 말.
- 3. 세그먼테이션 방식의 **가상 기억 장치** 에서 사용되는 것으로, 페이징에서 페이지와 비슷하나 길이가 가변이고 **기억 장치** 의 어느 곳에서도 자리할 수 있는 **기억 장소** 영역을 가리키는 말.
    - 한 세그먼트는 프로그램의 논리적인 한 구성 단위를 저장한다.
- 4. 계층 모형의 **데이터베이스** 에서 여러 항목이 모여 레코드에 해당하는 단위.
<img src = "https://github.com/devKobe24/images2/blob/main/network/Segment.png?raw=true">

## 2️⃣ 네트워크 세그먼트(Network Segment).
- 네트워크 세그먼트(Network Segment)는 네트워크 내에서 논리적 또는 물리적으로 분리된 하나의 부분 또는 구역을 의미합니다.
- 네트워크를 여러 세그먼트로 나누는 것은 네트워크의 성능을 최적화하고, 보안을 강화하며, 트래픽 관리를 효율적으로 ㅎ기 위한 일반적인 방법입니다.

### 1️⃣ 네트워크 세그먼트의 주요 개념.
- **1. 물리적 세그먼트**
    - 물리적 네트워크 세그먼트는 실제 네트워크 장비(스위치, 라우터 등)와 물리적 케이블을 통해 분리된 네트워크 구역입니다.
        - 예를 들어, 한 사무실 내에서 여러 층에 있는 네트워크가 각기 다른 물리적 스위치에 연결되어 있다면, 이들 층은 물리적으로 서로 다른 네트워크 세그먼트로 간주될 수 있습니다.

- **2. 논리적 세그먼트**
    - 논리적 네트워크 세그먼트는 물리적 인프라와 무관하게 네트워크를 논리적으로 나누는 것을 의미합니다.
        - 이는 주로 **VLAN(가상 로컬 영역 네트워크)** 이나 **서브넷(Subnet)** 을 사용하여 이루어집니다.
            - 예를 들어, 동일한 물리적 네트워크 내에서 서로 다른 부서의 컴퓨터를 논리적으로 분리하여 독립된 네트워크 세그먼트로 만들 수 있습니다.

### 2️⃣ 네트워크 세그먼트의 주요 목적.
- **1. 보안 강화**
    - 네트워크 세그먼트는 네트워크를 여러 개의 작은 구역으로 나누어, 각 구역의 트래픽을 독립적으로 관리하고 보호할 수 있습니다.
        - 이를 통해 민감한 데이터를 처리하는 부서나 서버를 외부와 분리하여 보안을 강화할 수 있습니다.
- **2. 성능 최적화**
    - 네트워크를 세그먼트로 나누면 네트워크의 트래픽이 특정 구역 내에서만 흐르게 하여, 트래픽 혼잡을 줄이고 전체 네트워크의 성능을 향상시킬 수 있습니다.
        - 예를 들어, 대용량 파일 전송이 필요한 부서의 네트워크 세그먼트를 다른 부서와 분리함으로써, 다른 부서의 네트워크 성능에 영향을 주지 않도록 할 수 있습니다.
- **3. 트래픽 관리**
    - 네트워크 세그먼트를 통해 트래픽을 효과적으로 관리하고, 특정 세그먼트의 트래픽을 제거할 수 있습니다.
        - 네트워크 관리자는 각 세그먼트에 대해 별도의 라우팅 규칙, 방화벽 규칙 등을 적용할 수 있습니다.
- **4. 내결함성 향상**
    - 네트워크가 세그먼트로 분리되어 있으면, 한 세그먼트에서 발생한 문제가 다른 세그먼트로 확산되지 않도록 방지할 수 있습니다.
        - 예를 들어, 한 세그먼트에서 발생한 네트워크 장애가 전체 네트워크에 영향을 미치지 않게 할 수 있습니다.

### 3️⃣ 예시: 서브넷을 이용한 네트워크 세그먼트.
- **서브넷은 네트워크 세그먼트의 한 유형입니다.**
    - 예를 들어, **'10.0.0.0/16'** IP 주소 블록을 가진 VPC에서 **'10.0.1.0/24'** 와 **'10.0.2.0/24'** 라는 두 개의 서브넷을 생성할 수 있습니다.
    - 이 두 서브넷은 동일한 VPC 내에서 서로 독립된 네트워크 세그먼트를 구성합니다.
        - 이로 인해 한 서브넷에서 발생한 네트워크 트래픽은 기본적으로 다른 서브넷에 영향을 미치지 않습니다.

### ✏️ 요약
- 요약하자면, 네트워크 세그먼트는 네트워크를 보다 안전하고 효율적으로 운영하기 위해 분리된 네트워크 구역을 의미하며, 물리적 또는 논리적으로 구성될 수 있습니다.