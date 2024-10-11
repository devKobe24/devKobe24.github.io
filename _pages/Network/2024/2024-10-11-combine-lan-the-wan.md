---
title: "🌐[Network] LAN이 합쳐져서 WAN이 되는걸까?"
tags:
    - Network
date: "2024-10-11"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] LAN이 합쳐져서 WAN이 되는걸까?

- 네트워크 개념에서 **LAN(Local Area Network, 근거리 통신망)이 WAN(Wide Area Network, 광역 통신망)으로** 단순히 **합쳐지는 것**은 아닙니다.
    - LAN과 WAN은 서로 다른 규모와 기능을 가진 네트워크 유형이며, 그 관계는 단순한 결합 이상의 개념입니다.

## 1️⃣ LAN과 WAN의 차이와 관계.

### 1️⃣ LAN(Local Area Network, 근거리 통신망)
- **LAN(Local Area Network, 근거리 통신망)은 지리적 범위**(예: 집, 사무실, 학교) 내에서 컴퓨터, 프린터, 서버 등 장치들을 연결하는 네트워크입니다.
- 고속 데이터 전송을 제공하며, 주로 **이더넷(Ethernet)** 또는 **Wi-Fi**를 사용하여 장치 간의 통신을 처리합니다.

### 2️⃣ WAN(Wide Area Network, 광역 통신망)
- **WAN(Wide Area Network, 광역 통신망)은 넓은 지리적 범위**(예: 여러 도시, 국가, 심지어 전 세계)를 커버하는 네트워크입니다.
- WAN(Wide Area Network, 광역 통신망)은 LAN(Local Area Network, 근거리 통신망)보다 느릴 수 있으며, 여러 **LAN(Local Area Network, 근거리 통신망)을 연결하여 원거리 통신을 가능하게 합니다.**
- WAN(Wide Area Network, 광역 통신망)은 ISP(Internet Service Provider, 인터넷 서비스 제공업체) 또는 **전용선을 통해** 연결되며, 네트워크 장치 간의 물리적 거리가 매우 멀 수 있습니다.

> 🙋‍♂️ [WAN(Wide Area Network)이란 무엇일까요?](https://www.devkobe24.com/Network/2024/2024-10-11-what-is-the-wan.html)
> 🙋‍♂️ [이더넷(Ethernet)이란 무엇일까요?](https://www.devkobe24.com/Network/2024/2024-10-09-what-is-the-ethernet.html)
> 🙋‍♂️ [LAN(Local Area Network)이란 무엇일까요?](https://www.devkobe24.com/Network/2024/2024-10-08-what-is-the-local-area-network.html)

## 2️⃣ LAN과 WAN의 관계.

### 👉 LAN(Local Area Network, 근거리 통신망)이 WAN(Wide Area Network, 광역 통신망)에 연결된다.
- 여러 **LAN(Local Area Network, 근거리 통신망)을 WAN(Wide Area Network, 광역 통신망)으로** 연결하여 **광범위한 네트워크를 형성합니다.**
    - 예를 들어, 한 기업의 각 지사(각 지사는 LAN(Local Area Network, 근거리 통신망)을 형성)를 WAN(Wide Area Network, 광역 통신망)을 통해 연결하여 본사와 다른 지사 간에 통신할 수 있게 합니다.

### 👉 WAN(Wide Area Network, 광역 통신망)은 LAN(Local Area Network, 근거리 통신망)을 포함할 수 있다.
- **WAN(Wide Area Network, 광역 통신망)은 여러 LAN(Local Area Network, 근거리 통신망)을 포함할 수 있습니다.**
    - 즉, WAN(Wide Area Network, 광역 통신망)의 일부는 여러 개의 LAN(Local Area Network, 근거리 통신망)을 연결하여, 전세계에 분산된 장치들이 서로 통신할 수 있게 만듭니다.

## 3️⃣ 예시

### 1️⃣ 회사 네트워크.
<img src = "https://github.com/devKobe24/images2/blob/main/network/network-lan-company-example.png?raw=true">

- 한 회사가 **서울, 부산, 뉴욕**에 지사를 두고 있다고 가정합니다.
    - 각 지사는 자체적인 **LAN(Local Area Network, 근거리 통신망)을** 가지고 있는데, 이 LAN(Local Area Network, 근거리 통신망)은 지사 내의 컴퓨터, 프린터, 서버 등을 연결합니다.

<img src = "https://github.com/devKobe24/images2/blob/main/network/network-wan-company-example.png?raw=true">

- 각 지사는 **WAN(Wide Area Network, 광역 통신망)을** 통해 본사와 다른 지사들과 연결됩니다.
    - WAN(Wide Area Network, 광역 통신망)을 통해 서울 지사의 직원이 부산 지사의 서버에 접근하거나, 뉴욕 지사와 파일을 공유할 수 있습니다.

### 2️⃣ 인터넷(Internet).
- 인터넷(Internet)은 세계 최대의 **WAN(Wide Area Network, 광역 통신망)으로, 여러 LAN(Local Area Network, 근거리 통신망)과 WAN(Wide Area Network, 광역 통신망)을 연결하여 전 세계의 컴퓨터들이 서로 통신할 수 있도록 만듭니다.**
    - 내가 집에서 인터넷에 접속하는 경우, 집의 **LAN**(예: Wi-Fi 네트워크)이 인터넷(WAN, Wide Area Network)을 통해 전 세계의 다른 컴퓨터와 연결됩니다.

## 4️⃣ WAN(Wide Area Network, 광역 통신망)의 구현 방법.
- **WAN(Wide Area Network, 광역 통신망)을** 구현하기 위해서는 **ISP(Internet Service Provider, 인터넷 서비스 제공자), 위성 링크, 전용선, 광섬유 등의 기술이 사용됩니다.**
    - LAN(Local Area Network, 근거리 통신망)은 일반적으로 **스위치, 라우터, 케이블을** 통해 설정되지만, WAN(Wide Area Network)은 훨씬 더 복잡한 기술 인프라를 필요로 합니다.
- **VPN(가상 사설망)도** WAN(Wide Area Network, 광역 통신망)을 구축하는 방법 중 하나로, 여러 LAN을 공용 네트워크(인터넷, Internet)를 통해 **안전하게 연결할 수 있도록 해줍니다.**

## 5️⃣ 결론.
- **LAN(Local Area Network, 근거리 통신망)이 여러개 합쳐져서 WAN(Wide Area Network, 광역 통신망)이 되는 것은 아닙니다.**
    - 그러나 **LAN(Local Area Network, 근거리 통신망)은 WAN(Wide Area Network, 광역 통신망)의 일부가 될 수 있으며, WAN(Wide Area Network, 광역 통신망)은 여러 LAN(Local Area Network, 근거리 통신망)을 연결하여 광범위한 통신망을 형성합니다.**
- **WAN(Wide Area Network, 광역 통신망)은** 주로 먼 거리의 네트워크를 연결하기 위한 것이고, **LAN(Local Area Network, 근거리 통신망)은** 가까운 거리에서 효율적으로 자원을 공유하고 데이터를 전송하기 위해 설계된 네트워크입니다.
