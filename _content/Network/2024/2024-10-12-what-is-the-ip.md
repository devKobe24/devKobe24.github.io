---
title: "🌐[Network] IP(Internet Protocol)란 무엇일까?"
tags:
    - Network
date: "2024-10-12"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] IP(Internet Protocol)란 무엇일까?
- **IP(Internet Protocol)는 컴퓨터 네트워크에서 데이터를 전송하기 위한 주소 체계이자 프로토콜입니다.**
- IP는 **패킷(Packet) 단위**로 데이터를 전송하며, 데이터를 전송할 **발신자와 수신자를 식별하기 위해 고유한 IP 주소를 사용합니다.**
- IP는 **네트워크 계층(Network Layer)에서** 동작하며, 인터넷과 같은 **연결망에서 장치들이 서로 통신할 수 있도록 해줍니다.**

## 1️⃣ IP의 주요 역할

### 1️⃣ 주소 지정.
- IP(Internet Protocol)는 네트워크 상에서 각 장치(컴퓨터, 서버, 라우터 등)에 **고유한 IP 주소**를 부여하여, 주고받는 **송신자와 수신자를 식별합니다.**
- **IP 주소는** 각 장치가 네트워크 상에서 **유일하게 구분될 수 있는 식별자로,** 데이터를 정확한 목적지로 전달하는 데 사용됩니다.

### 2️⃣ 데이터 패킷 전송.
- IP(Internet Protocol)는 **데이터를 작은 패킷(Packet) 단위로 나누어 전송합니다.**
    - 이러한 패킷(Packet)들은 각각 **IP 주소를 통해 발신지에서 목적지까지 최적의 경로로 전달됩니다.**
        - 패킷(Packet)은 서로 다른 경로를 통해 전달될 수 있으며, 목적지에서 다시 조립됩니다.

### 3️⃣ 경로 설정 및 라우팅.
- IP는 네트워크 장비인 **라우터를** 통해 패킷(Packet)의 **전송 경로**를 결정합니다.
    - 라우터는 목적지 IP 주소를 확인하고, 패킷(Packet)이 올바른 경로로 전달되도록 합니다.
        - IP(Internet Protocol)는 패킷(Packet)이 네트워크를 목적지까지 효율적으로 전달되도록 경로를 설정하는 역할을 합니다.

## 2️⃣ IP 주소의 구조.
- **IP(Internet Protocol) 주소는 네트워크 상에서 장치를 구분하는 고유한 숫지 식별자**입니다.
- IP(Internet Protocol) 주소는 **IPv4와 IPv6**의 두 가지 버전으로 나뉩니다.

### 1️⃣ IPv4(Internet Protocol version 4)
- 32비트로 구성된 주소로, 닷(deciaml) 표기법을 사용하여 4개의 숫자로 표현됩니다.
    - 각 숫자는 0~255 범위 내에 있으며 **점(.)으로** 구분됩니다.
        - 예시: `192.169.1.1`
- IPv4는 약 43억 개의 고유 주소를 제공하지만, 인터넷 사용이 폭발적으로 증가하면서 주소가 고갈되고 있어, 이를 대체하기 위해 **IPv6**가 도입되었습니다.

### 2️⃣ IPv6(Internet Protocol version 6)
- 128비트로 구성된 주소로, 16진수를 사용해 8개의 그룹으로 표현됩니다.
    - 각 그룹은 **콜론(,)으로** 구분됩니다.
        - 예시: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- IPv6는 매우 많은(340언델리온) 주소 공간을 제공하여, 미래의 인터넷 사용을 충분히 지원할 수 있습니다.

## 3️⃣ IP의 주요 기능.

### 1️⃣ 데이터 패킷화 및 전송.
- IP는 데이터를 패킷(Packet) 단위로 분할아혀 전송합니다.
    - 각 패킷에는 발신자 IP(Internet Protocol) 주소와 수신자 IP(Internet Protocol) 주소가 포함되며, 패킷은 네트워크 상의 여러 장비(라우터 등)를 거쳐 목적지에 도달합니다.

### 2️⃣ 최적 경로 설정.
- IP는 네트워크 내에서 **최적의 경로를 통해** 데이터를 전달합니다.
    - 패킷(Packet)은 각 경로를 지나며, IP(Internet Protocol) 주소를 기반으로 **라우팅**됩니다.
        - 이를 통해 데이터가 가장 빠르고 효율적으로 전달될 수 있습니다.

### 3️⃣ 비연결성.
- IP는 **비연결성 프로토콜**입니다.
    - 즉, 데이터를 전송할 때 **송신자와 수신자 간에 사전 연결 설정을 하지 않습니다.**
        - 각 패킷(Packet)은 독립적으로 전송되며, 네트워크 상황에 따라 **서로 다른 경로로 전달될 수 있습니다.**

### 4️⃣ 무결성 보장 없음.
- IP(Internet Protocol)는 데이터를 전송할 때 **패킷(Packet)의 신뢰성**이나 **순서 보장**을 하지 않습니다.
    - 패킷(Packet)이 손실되거나, 순서가 뒤바뀔 수 있으며, 이는 **상위 계층의 프로토콜(TCP, Transmission Control Protocol)이 처리합니다.**
        - IP(Internet Protocol)는 단지 데이터를 **전송**하는 **역할을 수행합니다.**

> 🙋‍♂️ [TCP(Transmission Control Protocol)란 무엇일까요](https://www.devkobe24.com/Network/2024/2024-10-12-what-is-the-tcp.html)

## 4️⃣ IP의 동작 방식.

### 1️⃣ 패킷 생성.
- 송신 장치는 데이터를 **작은 패킷(Packet)으로** 나누고, 각 패킷에 **IP 헤더**를 추가합니다.
    - 이 헤더에는 송신자와 수신자의 IP(Internet Protocol) 주소가 포함됩니다.

### 2️⃣ 라우팅.
- 패킷(Packet)이 네트워크를 통해 전송될 때, 각 패킷(Packet)은 라우터(Router)를 거쳐 최종 목적지로 향합니다.
    - 라우터는 패킷(Packet)의 **목적지 IP 주소를 분석하여, 패킷을 적절한 경로로 전송합니다.**

### 3️⃣ 패킷 수신.
- 목적지 장치에서 패킷(Packet)을 수신하면, 패킷(Packet)을 다시 **원래의 데이터로 재조립합니다.**
    - 상위 계층의 프로토콜(TCP(Transmission Control Protocol) / UDP(User Datagram Protocol))은 **패킷의 손신 여부나 순서를 확인하고, 데이터를 정확하게 전달하도록 보장합니다.**

## 5️⃣ IP의 종류.

### 1️⃣ 공인 IP 주소(Public IP Address)
- 공인 IP 주소(Public IP Address)는 **인터넷 상에서 고유한 주소로,** 인터넷 서비스 제공업체(ISP, Internet Service Provider)에 의해 할당됩니다.
    - 이 주소는 전 세계에서 유일하며, 인터넷을 통해 다른 장치들과 통신할 수 있습니다.

### 2️⃣ 사설 IP 주소(Private IP Address)
- 사설 IP 주소(Private IP Address)는 **로컬 네트워크**에서 사용되는 IP(Internet Protocol) 주소 입니다.
    - 같은 사설 IP 주소는 여러 네트워크에서 중복해서 사용할 수 있지만, 인터넷과 직접 통신할 수 없습니다.
        - 사설 IP는 **NAT(Network Address Translation)을** 통해 공인 IP(Public IP)로 변환되어 인터넷에 접근할 수 있습니다.

#### 👉 사설 IP 주소 대역
- `1.0.0.0` ~ `10.255.255.255`
- `172.16.0.0` ~ `172.31.255.255`
- `192.168.0.0` ~ `192.168.255.255`

## 6️⃣ IP 프로토콜의 버전.

### 1️⃣ IPv4(Internet Protocol version 4)
- IPv4는 인터넷이 상용화된 초기부터 사용된 프로토콜로, 32비트 주소 체계를 사용합니다.
    - 43억 개의 IP 주소를 제공하지만, 인터넷 장치의 폭발적 증가로 인해 IP 주소가 부족해지고 있습니다.

### 2️⃣ IPv6(Internet Protocol version 6)
- IPv6는 128비트 주소 체계를 사용하여 거대한 주소 공간을 제공합니다.
    - IPv6는 IPv4의 주소 부족 문제를 해결하기 위해 도입되었으며, 주소 자동 구성 및 향상된 보안 기능을 지원합니다.

## 7️⃣ IP 프로토콜의 역할과 TCP/IP 모델.
- IP는 네트워크 계층(Network Layer)에서 동작하는 프로토콜로, 데이터가 목적지에 도달할 수 있도록 경로 설정과 주소 지정을 처리합니다.
    - IP는 TCP와 함께 동장하여 데이터를 안전하게 전송하는 TCP/IP 모델의 일부입니다.
- **TCP(Transmission Control Protocol)는 전송 계층에**서 데이터를 신뢰성 있게 전송하며, **IP(Internet Protocol)는 네트워크 계층**에서 패킷(Packet)을 올바른 경로로 전송합니다.

> 🙋‍♂️ [OSI 7계층 모델](https://www.devkobe24.com/Network/2024/2024-09-17-OSI-7-Layer.html)

## 8️⃣ IP와 관련된 주요 프로토콜.

### 1️⃣ TCP(Transmission Control Protocol)
- TCP(Transmission Control Protocol)는 신뢰성 있는 데이터 전송을 보장하는 프로토콜입니다.
    - IP(Internet Protocol)는 패킷(Packet)을 목적지로 전달하는 역할을 하지만, TCP(Transmission Control Protocol)는 **패킷의 무결성, 순서 보장, 오류 검출 및 수정을 담당합니다.**

### 2️⃣ UDP(User Datagram Protocol)
- UDP(User Datagram Protocol)는 **비연결형 프로토콜**로, IP(Internet Protocol)와 마찬가지로 데이터를 전송하지만, 신뢰성을 보장하지 않습니다.
    - TCP(Transmission Control Protocol)와 달리 **오버헤드(Overhead)가 적고 빠르기 때문에,** 실시간 스트리밍이나 온라인 게임에서 많이 사용됩니다.

### 3️⃣ ICMP(Internet Control Message Protocol)
- ICMP(Internet Control Message Protocol)는 네트워크 진단 및 오류 처리를 위한 프로토콜입니다.
    - **Ping** 명령어는 ICMP를 사용하여 네트워크 연결 상태를 확인합니다.

## 9️⃣ IP의 사용 사례.
- **인터넷 통신**
    - IP는 인터넷 상의 모든 장치가 서로 통신할 수 있도록 주소를 제공하고, 데이터를 전송하는 핵심 프로토콜입니다.
- **로컬 네트워크(LAN, Local Area Network, 근거리 통신망)**
    - IP는 가정이나 사무실에서 **로컬 네트워크** 내 장치들이 서로 데이터를 주고받을 수 있게 합니다.
- **네트워크 장비 간 통신**
    - 라우터, 스위치, 서버 등 네트워크 장비들이 IP 주소를 통해 서로 통신하고 데이터를 전달합니다.

## 1️⃣0️⃣ 결론.
- **IP(Internet Protocol)는 네트워크 상의 장치들이 데이터를 주고받을 수 있도록 하는 기본 프로토콜입니다.** 
- IP 주소를 통해 각 장치를 고유하게 식별하고, 데이터를 패킷(Packet) 단위로 나누어 전달합니다.
- IPv4와 IPv6 두 가지 버전이 있으며, 경로 설정, 주소 지정, 데이터 패킷화 등을 담당합니다.
- IP는 네트워크 통신의 핵심적인 역할을 수행하며, 인터넷과 로컬 네트워크에서 모든 장치 간 통신을 가능하게 합니다.
