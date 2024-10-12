---
title: "🌐[Network] 라우터와 스위치의 차이점."
tags:
    - Network
date: "2024-10-13"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] 라우터(Router)와 스위치(Switch)의 차이점.
- **라우터(Router)와 스위치(Switch)는** 네트워크 장비로서, 모두 데이터를 전달하고 네트워크 장치들을 연결하는 역할을 하지만, **기능과 동작 방식**에서 중요한 차이점이 있습니다.
- **주로 어느 계층에서 동작하는지, 어떤 정보를 바탕으로 데이터를 전송하는지, 그리고 어떤 네트워크 환경에서 사용되는지에 따라 구분됩니다.**

## 1️⃣ 기본 역할.

### 👉 라우터(Router)
- **다른 네트워크 간의 통신을 담당합니다.**
    - 라우터(Router)는 **IP 주소를 사용하여 패킷(Packet)을 목적지 네트워크로 라우팅(Routing)하며, 주로 LAN(Local Area Network, 근거리 통신망)과 WAN(Wide Area Network, 광역 통신망)** 간의 연결을 담당합니다.
        - 즉, **네트워크와 네트워크를 연결하는 장치입니다.**

> 🙋‍♂️ [IP(Internet Protocol)란 무엇일까?](https://www.devkobe24.com/Network/2024/2024-10-12-what-is-the-ip.html)
> 🙋‍♂️ [WAN(Wide Area Network)이란 무엇일까요?](https://www.devkobe24.com/Network/2024/2024-10-11-what-is-the-wan.html) 
> 🙋‍♂️ [LAN이 합쳐져서 WAN이 되는걸까?](https://www.devkobe24.com/Network/2024/2024-10-11-combine-lan-the-wan.html)
> 🙋‍♂️ [LAN(Local Area Network)이란 무엇일까요?](https://www.devkobe24.com/Network/2024/2024-10-08-what-is-the-local-area-network.html)

### 👉 스위치(Switch)
- **같은 네트워크 내 장치들 간의 통신을 관리합니다.**
    - 스위치(Switch)는 **MAC(Media Access Control) 주소**를 기반으로 데이터를 전송하며, LAN(Local Area Network, 근거리 통신망) 내에서 컴퓨터, 프린터, 서버 등의 장치들이 서로 통신할 수 있도록 **패킷(Packet)을** 전달하는 역할을 합니다.
        - 즉, **장치와 장치를 연결하는 장치입니다.**

> 🙋‍♂️[MAC(Media Access Control)주소란 무엇일까요?](https://www.devkobe24.com/Network/2024/2024-10-09-what-is-the-mac-address.html)

## 2️⃣ 작동하는 계층.

### 👉 라우터(Router)
- **OSI 7계층 모델의 3계층(네트워크 계층)에서 동작합니다.**
    - 라우터(Router)는 **IP(Internet Protocol) 주소**를 바탕으로 패킷(Packet)의 라우팅(Routing)을 수행하며, 서로 다른 네트워크 간의 **데이터 전송 경로를** 설정하고 관리합니다.

### 👉 스위치(Switch)
- **OSI 7 계층 모델의 2계층(데이터 링크 계층)에서 동작합니다.**
    - 스위치(Switch)는 **MAC(Media Access Control) 주소**를 사용하여 데이터를 목적지 장치로 전달합니다.
    - 스위치(Switch)는 **장치 간 데이터 프레임을 전송하고, 네트워크 내에서 패킷(Packet)을 필터링하고 전송할 경로를 설정합니다.**

> 🙋‍♂️ [OSI 7계층 모델](https://www.devkobe24.com/Network/2024/2024-09-17-OSI-7-Layer.html)
> 🙋‍♂️ [OSI 7계층 모델 - 계층별 기능](https://www.devkobe24.com/Network/2024/2024-09-19-features-by-layer.html)
> 🙋‍♂️ [LAN 내에서 전달되는 데이터 프레임이란 무엇일까?](https://www.devkobe24.com/Network/2024/2024-10-12-data-frame.html)

## 3️⃣ 주소 기반 동작.

### 👉 라우터(Router)
- **IP(Internet Protocol) 주소를** 사용하여 네트워크 장치 간에 패킷(Packet)을 전달합니다.
    - **출발지 IP(Internet Protocol) 주소와 목적지 IP(Internet Protocol) 주소를 분석하여, 데이터를 최적의 경로로 라우팅(Routing)합니다.**
        - 라우터(Router)는 네트워크 간의 트래픽을 제어하고, **인터넷과 같은 WAN(Wide Area Network)** 연결을 관리합니다.

### 👉 스위치(Switch)
- **MAC(Media Address Control) 주소를 사용하여 데이터 프레임을 전달합니다.**
    - 스위치(Switch)는 **MAC(Media Access Control) 주소 테이블을 유지하며, 패킷(Packet)의 목적지 MAC(Media Access Control) 주소에 따라 해당 프레임을 적절한 포트로 전송합니다.**
        - 주로 **LAN(Local Area Network, 근거리 통신망)** 내에서 사용됩니다.

> 🙋‍♂️ [네트워크 포트(Network Port)란 무엇일까?](https://www.devkobe24.com/Network/2024/2024-10-12-what-is-the-network-port.html)
> 🙋‍♂️ [네트워크, 포트, 도메인 이름, IP, DNS](https://www.devkobe24.com/Network/2024/2024-10-12-what-is-the-network-port.html)

## 4️⃣ 주요 기능.

### 👉 라우터(Router)
- 서로 다른 네트워크(LAN(Local Area Network, 근거리 통신망)과 WAN(Wide Area Network, 광역 통신망)) 간의 **데이터 패킷(Packet) 전달 및 라우팅(Routing)**
- **인터넷 연결을 공유하고, 다양한 네트워크를 연결하는 역할.**
- **네트워크 트래픽 제어 및 IP(Internet Protocol) 주소 할당(DHCP), 네트워크 주소 변환(NAT)**
- **방화벽(Firewall)** 기능을 통해 보안 제어 가능.

### 👉 스위치(Switch)
- **LAN(Local Area Network, 근거리 통신망)** 내에서 여러 장치 간의 데이터를 **빠르게 전송.**
- **MAC(Media Access Control) 주소 기반**으로 데이터를 필터링하고 **적절한 포트(Port)로 전송.**
- 네트워크(Network) 내 장치 간의 **충돌 방지 및 효율적인 데이터 전송.**
- **VLAN(Virtual LAN)을 설정하여 네트워크 세분화 가능.**

> 📝 VLAN(Virtual LAN, 가상 LAN)
> 
> 하나의 물리적인 네트워크 장비, 예를 들어 스위치 내에서 네트워크를 **논리적으로 분리하여 여러 개의 가상 네트워크를 만드는 기술입니다.**
> VLAN(Virtual LAN, 가상 LAN)은 서로 다른 네트워크 세그먼트에 속하는 장치들을 같은 스위치에 연결되어 있어도 **서로 통신할 수 없도록 분리**하거나, **같은 VLAN에 속한 장치들끼리만 통신할 수 있도록 합니다.**

> 📝 Virtual Network(가상 네트워크)
> 
> 물리적인 네트워크 하드웨어와 상관없이, 소프트웨어적으로 구현된 네트워크를 의미합니다.
> 가상 네트워크는 물리적인 네트워크 인프라를 **추상화**하여, 네트워크의 연결과 관리가 **소프트웨어적으로 처리**되도록 구성됩니다.
> 이러한 가상 네트워크는 서버, 네트워크 장비, 클라이언트 등을 물리적으로 연결하지 않고도 **논리적으로 네트워크를 생성하고 관리할 수 있게 해줍니다.**

## 5️⃣ 사용되는 네트워크 범위.

### 👉 라우터(Router)
- **다른 네트워크 간의 연결**을 관리하므로, 주로 WAN(Wide Area Network, 광역 통신망)이나 인터넷 연결이 필요한 환경에서 사용됩니다.
    - 예를 들어, 가정에서는 라우터(Router)가 **인터넷 서비스 제공업체(ISP, Internet Service Provider)와** 사용자의 네트워크를 연결하는 역할을 수행합니다.

### 👉 스위치(Switch)
- **같은 네트워크 내에서 장치들을 연결합니다.**
    - 주로 LAN(Local Area Network, 근거리 통신망)에서 사용되며, 컴퓨터, 프린터, 서버 등 다양한 장치가 동일한 네트워크 내에서 **데이터를 주고받는 데 사용됩니다.**

## 6️⃣ 전송 방식.

### 👉 라우터(Router)
- **패킷 스위칭(Packet Switching)을** 사용합니다.
    - 라우터(Router)는 패킷(Packet)이 어디로 갈지에 대한 최적의 경로를 계산하고, 각 패킷이 **다른 경로로 이동할 수 있게 합니다.**

### 👉 스위치(Switch)
- **프레임 스위칭(Frame Switching)을** 사용합니다.
    - 스위치(Switch)는 **목적지 MAC(Media Address Control) 주소를 기반으로 데이터 프레임을 전송하고, 네트워크 내 장치 간의 빠른 데이터 전송을 보장합니다.**

## 7️⃣ 브로드캐스트 도메인과 충돌 도메인.

### 👉 라우터(Router)
- **브로드캐스트 도메인(Broadcast Domain)을 분리합니다.**
    - 라우터(Router)는 네트워크를 나누기 때문에, 라우터(Router)를 경우한 네트워크 간에는 브로드캐스트 트래픽이 전달되지 않습니다.

### 👉 스위치(Switch)
- **브로드캐스트 도메인(Broadcast Domain)을** 나누지 않고, **충돌 도메인(Collision Domain)만** 분리합니다.
    - 스위치(Switch)는 연결된 모든 장치가 동일한 브로드캐스트 도메인에 속해 있으므로, 브로드캐스트 트래픽이 스위치(Switch)를 경유하여 네트워크(Network) 전체에 전달됩니다.

> 📝 브로드캐스트 도메인(Broadcast Domain)
> 
> **브로드캐스트 도메인(Broadcast Domain)은** 네트워크 내에서 브로드캐스트 트래픽이 전파되는 범위를 의미합니다.
> 다시 말해, 네트워크 장치가 **브로드캐스트(Broadcast) 메시지(모든 네트워크 장치에 전송되는 메시지)를** 보냈을 때, 그 메시지를 받을 수 있는 네트워크 상의 모든 장치들이 속한 영역을 브로드캐스트 도메인(Broadcast Domain)이라고 합니다.

> 📝 충돌 도메인(Collision Domain)
> 
> **충돌 도메인(Collision Domain)은** 네트워크에서 **두 개 이상의 장치가 동시에 데이터를 전송할 때 충돌이 발생할 수 있는 네트워크 범위를 의미합니다.**
> 충돌이란 **동시에 전송된 데이터 패킷들이 서로 켭치거나 부딪히는 상황을 말하며,** 충돌 도메인 내에서는 한 번에 **하나의 장치만 데이터를 전송해야 충돌을 피할 수 있습니다.**
> 네트워크 장비가 데이터 충돌을 처리할 수 없는 환경에서는 충돌이 발생하면 데이터는 **손실되거나 다시 전송해야 하므로, 네트워크 성능에 영향을 미칠 수 있습니다.**

## 8️⃣ 보안 기능.

### 👉 라우터(Router)
- **보안 기능**이 더 강력합니다.
    - 라우터는 **방화벽(Firewall), NAT(Network Address Translation) 등을 통해 외부 네트워크로부터 보호하고,** 네트워크 트래픽을 모니터링하고 제어할 수 있습니다.

### 👉 스위치(Switch)
- 기본적으로 **보안 기능**이 적습니다.
    - 스위치는 **내부 네트워크**에서 장치 간의 데이터를 전송하므로, 보안 측면에서 라우터만큼 강력한 보호 기능을 제공하지는 않습니다.
        - 그러나 고급 스위치에서는 **VLAN(Virtual LAN, 가상 LAN)을** 통해 네트워크를 분리하고, **포트 보안**을 설정할 수 있습니다.

## 9️⃣ 주요 차이점 요약.

| 기능 | 라우터(Router) | 스위치(Switch) |
| --- | --- | --- |
| 작동 계층 | 네트워크 계층(3계층, IP 주소 사용) | 데이터 링크 계층(2계층, MAC 주소 사용) |
| 사용하는 주소 | IP 주소 | MAC 주소 |
| 기본 역할 | 서로 다른 네트워크를 연결(LAN과 WAN간) | 동일 네트워크 내의 장치들을 연결(LAN 내) |
| 보안 기능 | 방화벽, NAT, 트래픽 제어 등 고급 보안 기능 제공제한적(고급 스위치 VLAN 및 포트 보안 사용 가능) | 제한적(고급 스위치에서 VLAN 및 포트 보안 사용 가능) |
| 브로드캐스트 도메인 |분리| 분리하지 않음|
|주요 사용 환경|WAN, 인터넷 연결, 서로 다른 네트워크 간 데이터 전송| LAN 내에서의 데이터 전송 및 네트워크 장치 연결|
|전송 방식|패킷 스위칭(IP 패킷)|프레임 스위칭(데이터 프레임)|

## 1️⃣0️⃣ 결론.
- **라우터(Router)는** 서로 다른 네트워크 간의 연결을 관리하며, **IP 주소**를 통해 네트워크 간의 경로를 설정하고 데이터를 전송하는 역할을 합니다.
- 반면, **스위치(Switch)는 LAN(Local Area Network, 근거리 통신망)** 내의 장치들을 연결하고 **MAC(Media Access Control) 주소**를 통해 데이터 프레임을 전송합니다.
- 즉, 라우터(Router)는 **네트워크 간의 통신을,** 스위치(Switch)는 **네트워크 내부 장치들 간의 통신을** 담당합니다.
