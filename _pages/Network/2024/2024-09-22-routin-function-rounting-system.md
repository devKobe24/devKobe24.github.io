---
title: "🌐[Network] 라우팅 기능 - 라우팅 시스템"
tags:
    - Network
date: "2024-09-22"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] 라우팅 기능 - 라우팅 시스템.

- 네트워크 양단에 연결된 호스트들이 전송하는 데이터는 전송 경로 중간에 위치한 라우팅 시스템을 거칩니다.
- 라우팅 시스템은 데이터를 최종 목적지까지 올바른 경로로 중개하는 교환(Switching) 기능을 제공합니다.
- 네트워크의 발전이 전화망에서 시작된 까닭에 데이터의 중개 기능을 의미하는 일부 용어에 교환이라는 표현이 굳어지게 되었습니다.

<img src = "https://github.com/devKobe24/images2/blob/main/network/networ-routing-classification-of-system.png?raw=true">

- 위 그림은 데이터 통신망에서 제공하는 다양한 라우팅 시스템의 종류를 설명합니다.
- 연결형 서비스를 제공하는 회선 교환 시스템은 아날로그 환경의 음성 전화 서비스를 통해 발전했으며, 고정 대역폭의 전송률을 지원하므로 네트워크의 구조가 상대적으로 단순합니다.
- 반면에 비연결형 서비스를 제공하는 패킷 교환 시스템은 디지털 환경의 컴퓨터 네트워크를 통해 발전했으며, 가변 대역의 전송률을 지원해 네트워크 구조가 복잡합니다.
- 이외에도 프레임 릴레이 방식이 있는데, 이는 전송 속도를 향상시키는 기술입니다.

---

- 회선 교환(Circuit Switching) 시스템은 고정 대역으로 할당된 연결을 설정하여 데이터 전송을 시작합니다.
- 따라서 회선에 할당된 고정 크기의 안정적인 전송률로 데이터를 전송할 수 있으며, 연결이 유지되는 동안에는 다른 연결에서 이 대역을 사용할 수 없습니다.
- 데이터의 전송 경로가 연결 설정 과정에서 확정되므로 라우팅 등의 작업이 상대적으로 쉽습니다.
- 대표적인 회선 교환 방식인 전화망에서 음성 전송은 안정적인 속도로 일정하게 상대방에세 전달될 수 있지만, 전송 대역이 낭비된다는 단점이 있습니다.
- 회선 교환 시스템에서는 하나의 연결에 대하여 전송되는 모든 데이터가 동일한 경로로 라우팅됩니다.

---

- 패킷 교환(Packet Switching) 시스템은 컴퓨터 네트워크 환경에서 주로 이용합니다.
- 데이터를 미리 패킷 단위로 나누어 전송하므로 패킷을 기준으로 라우팅이 이루어집니다.
- 패킷 교환 시스템은 데이터 전송을 위한 전용 대역을 따로 할당하지 않기 때문에 가변 크기의 전송률을 지원합니다.
- 패킷 교환에는 모든 패킷의 경로를 일정하게 유지시키는 가상 회선(Virtual Circuit) 방식과 패킷들이 각각의 경로로 전송되는 데이터그램(Datagram) 방식이 있습니다.

> 인터넷을 사용할 때 네트워크의 부하 정도에 따라 전송 속도가 일정하지 않은 이유는 고정적인 전송 대역이 확보되지 않기 때문입니다.
> 따라서 네트워크의 사용량이 많아지면 전송 속도가 느려집니다.

## 1️⃣ 라우팅 시스템.
- 전송 선로를 이용해 데이터를 전송할 때는 전용 회선을 이용하거나 교환 회선을 이용할 수 있습니다.
- 전용 회선 방식에서는 송신 호스트와 수신 호스트가 전용으로 할당된 통신 선로로 데이터를 전송하지만, 교환 회선 방식에서는 전송 선로 하나를 다수의 호스트가 공유합니다.

---

- 일반적으로 전화 서비스와 같은 아날로그 환경의 공중 전화망은 교환 회선 방식을 사용합니다.

<img src = "https://github.com/devKobe24/images2/blob/main/network/network-configuration-based-on-switched-circuits.png?raw=true">

- 위 그림은 교환 회선 방식을 이용하여 네트워크를 구성한 예입니다.
- 교환 회선 방식을 이용해 데이터를 주고받으려면 중간에 위치한 교환 시스템의 중개가 필요합니다.
- 그림에서 뭉게구름으로 표현된 네트워크 안에 있는 교환기와 전송 선로들은 바깥에 있는 호스트들이 데이터를 송수신하기 위해 공유하는 자원입니다.
- 전송 선로는 광케이블 기술 등의 발전으로 물리적인 전송 대역 용량에 비례하여 논리적인 다수의 연결 회선을 지원합니다.
- 호스트 a에서 호스트 f로 데이터를 보내려면 먼저 연결 설정을 통해 전송 경로를 결정해야 합니다.
- 외형상 가장 합리적인 경로는 교환기 1 -> 교환기 3 -> 교환기 5 입니다.
- 만약 교환기 1과 3 사이에 트래픽이 많은 경우라면 교환기 1 -> 교환기 2 -> 교환기 3 -> 교환기 5가 대안이 될 수도 있습니다.
- 어떤 경로가 더 나을지는 전송 시점의 네트워크 혼잡도 등 여러 요인에 따라 달라집니다.
- 특정 전송 선로에 데이터가 집중되지 않으면서 효율적인 경로를 선택할 수 있도록 하는 것이 교환기의 중요한 역할입니다.

---

- 교환 회선을 이용하는 방식은 데이터의 전송 경로와 관련하여 크게 두 가지로 구분됩니다.

<img src = "https://github.com/devKobe24/images2/blob/main/network/network-circuit-switching-and-packet-switching.png?raw=true">

- 하나는 위 그림의 (a)처럼 데이터를 전송하기 전에 통신 양단 사이에 고정된 연결 경로를 설정하는 회선 교환 방식아고, 다른 하나는 (b)처럼 미리 연결을 설정하지 않고, 데이터를 패킷 단위로 나누어 전송하는 패킷 교환 방식입니다.
- 이외에 메시지 교환이라는 중간 형태도 있습니다.
- (b)의 큰 사각형은 뭉개구름 그림의 교환기에 해당하며, 디지털 통신 환경을 지원하는 인터넷은 패킷 교환 방식의 네트워크에 해당합니다.

### 1. 회선 교환.
- 회선 교환(Circuit Switching) 방식은 위 그림의 (a)처럼 통신하고자 하는 호스트가 데이터를 전송하기 전에 연결 경로를 미리 설정하는 방식입니다.
- 연결 설정 과정에서 송수신 호스트 간의 경로가 결정되기 때문에 모든 데이터가 같은 경로로 전달됩니다.
- 회선 교환 방식에서는 고정 대역의 논리적인 전송 선로를 전용으로 할당받으므로, 안정적인 데이터 전송률을 지원합니다.
- 회선 교환 방식에서는 데이터를 패킷 단위로 나누어 전송하지 않기 때문에 패킷이라는 용어를 사용하지 않습니다.

### 2. 메시지 교환.
- 메시지 교환(Message Switching)방식은 데이터를 전송하기 전에 경로를 설정하지 않고, 대신 전송하는 메시지의 헤더마다 목적지 주소를 표시하는 방식입니다.
- 중간의 교환 시스템은 이전 교환 시스템에서 보낸 전체 메시지가 도착할 때까지 받은 메시지를 일시적으로 버퍼에 저장합니다.
- 이후 모든 메시지가 도착하면 다음 교환 시스템으로 전달하는 방식을 사용하므로 데이터 전송이 교환기 단위로 이어집니다.
- 따라서 메시지 교환 방식은 송신 호스트가 전송하는 전체 데이터가 하나의 단위로 교환 처리된다고 볼 수 있습니다.

### 3. 패킷 교환.
- 위 그림의 (b)와 같은 패킷 교환(Packet Switching) 방식은 회선 교환과 메시지 교환의 장점을 모두 이용합니다.
- 송신 호스트는 전송할 데이터를 패킷(Packet)이라는 일정 크기로 나누어 전송하며, 원칙적으로 송수신 호스트 사이의 연결이 존재하지 않으므로 각 패킷은 독립적인 라우팅 과정을 거쳐 수신 호스트에 도착합니다.
- 다만, 패킷 교환 방식에서도 회선 교환 방식과 같은 논리적인 연결 설정 개념을 도입한 가상 회선 방식이 존재합니다.

---

- 인터넷에서 사용하는 패킷 교환 시스템의 장점은 전송 대역의 효율적 이용, 호스트의 무제한 수용, 패킷에 우선순위 부여라는 세 가지로 요약할 수 있습니다.

- **1. 전손 대역의 효율적 이용**
    - 여러 호스트에서 전송한 패킷들이 동적인 방식으로 전송 대역을 공유하기 때문에 전송 선로의 이용 효율을 극대화할 수 있습니다.
    - 이를 반대의 관점으로도 설명할 수 있습니다.
    - 즉, 회선 교환 시스템에서는 호스트 간 연결 시 전송 대역을 전용으로 할당하기 때문에 해당 호스트들이 데이터를 전송하지 않더라도 다른 호스트는 이 전송 대역을 이용할 수 없습니다.
    - 결과적으로 회선 교환 시스템은 전송 선로 이용 면에서 비효율적입니다.
- **2. 호스트의 무제한 수용**
    - 회선 교환 방식에서는 개별 연결 요청에 대해 고정 대역을 할당하므로 전송 대역이 부족하면 새로운 연결 설정 요청을 수용할 수 없습니다.
    - 즉, 모든 회선 연결에 할당된 대역의 합이 전체 네트워크의 전송 용량을 초과할 수 없습니다.
    - 그러나 패킷 교환 방식에서는 전송 대역이 부족해 연결 설정 요청을 수용하지 못하는 경우란 없습니다.
    - 임의의 연결 요청에 고정 대역을 할당하지 않으므로 이론상 호스트를 무한히 수용할 수 있습니다.
    - 전송 대역을 사용하는 호스트의 수가 늘면 네트워크 혼잡도가 높아져 패킷의 전송 지연이 심화될 뿐입니다.
- **3. 패킷에 우선순위 부여**
    - 패킷 교환 방식을 이용하면 데이터의 전송 작업이 패킷 단위로 이루어져 패킷에 우선순위를 부여하기 편리합니다.
    - 즉, 특정 호스트가 전송하는 패킷들을 먼저 전송할 패킷과 나중에 전송해도 되는 패킷으로 구분하여 선택적으로 우선순위를 부여할 수 있습니다.

---

- 패킷 교환 방식에도 단점은 있습니다.
- 패킷을 전송하는 과정에서 회선 교환 방식에 비해 더 많은 지연이 발생합니다.
- 예를 들어, 전송 패킷을 라우터 내부 버퍼에 보관하는 과정에서 지연이 생기고, 기타 여러 종류의 대기 큐를 거치는 과정에서 가변 지연이 생길 수 있습니다.
- 또한 각각의 전송 패킷이 독립적인 경로로 전달되므로, 패킷마다 전송에 걸리는 시간이 일정하지 않을 수 있습니다.
- 패킷별로 지연되는 정도를 나타내는 지연 분포의 형태도 가변적일 수밖에 없는데, 이런 가변적인 전송 지연의 분포를 지터(Jiter)라 합니다.
- 지터 분포는 멀티미티어 데이터와 같이 실시간 처리되는 응용 환경에서 중요합니다.
- 교환기에서 패킷 경로를 선택하는 방식은 두 가지입니다.
- 호스트 사이의 전송 경로를 미리 고정하는 정적 경로(Static Routing)와 네트워크 혼잡도를 비롯한 주변 상황에 따라 전송 경로를 지속적으로 조정하는 동적 경로(Dynamic Routing)가 있습니다.
