---
title: "🌐[Network] LAN 내에서 전달되는 데이터 프레임이란 무엇일까?"
tags:
    - Network
date: "2024-10-12"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] LAN 내에서 전달되는 데이터 프레임이란 무엇일까?

- **LAN(Local Area Network, 근거리 통신망) 내에서 전달되는 데이터 프레임(Data Frame)은 네트워크 계층의 데이터가 데이터 링크 계층(OSI 7계층의 2계층)에서 전송 단위로** 사용되는 구조입니다.
- **프레임(Frame)은 이더넷(Ethernet)과** 같은 LAN(Local Area Network, 근거리 통신망) 기술에서 **데이터 패킷(Data Packet)을** 네트워크 장치 간에 전송하는 데 사용되며, **헤더, 데이터(페이로드,Payload), 트레일러로** 구성됩니다.

> 🙋‍♂️ [이더넷(Ethernet)이란 무엇일까요?](https://www.devkobe24.com/Network/2024/2024-10-09-what-is-the-ethernet.html)
> 🙋‍♂️ [LAN(Local Area Network)이란 무엇일까요?](https://www.devkobe24.com/Network/2024/2024-10-08-what-is-the-local-area-network.html)
> 🙋‍♂️ [OSI 7계층 모델](https://www.devkobe24.com/Network/2024/2024-09-17-OSI-7-Layer.html)

## 1️⃣ 데이터 프레임의 구성 요소.

### 1️⃣ 헤더(Header)
- **목적지 MAC 주소**
    - 데이터가 **어느 장치**로 전송될지 나타냅니다.
        - LAN(Local Area Network, 근거리 통신망) 내에서 연결된 네트워크 장치들(예: 컴퓨터, 프린터, 스위치 등)은 **MAC 주소를 통해 식별됩니다.**

> 🙋‍♂️ [MAC(Media Access Control) 주소란 무엇일까요?](https://www.devkobe24.com/Network/2024/2024-10-09-what-is-the-mac-address.html)

### 2️⃣ 데이터(페이로드, Payload)
- **상위 계층에서 전송된 실제 데이터**가 포함됩니다.
    - 주로 네트워크 계층(예: IP 패킷)의 데이터를 전달하며, 최대 1500바이트의 데이터를 포함할 수 있습니다.

### 3️⃣ 트레일러(Trailer)
- **프레임 체크 시퀀스(FCS, Frame Check Sequence)**
    - 데이터 전송 중 오류를 감지하는 **오류 검사 코드를 포함합니다.**
    - 수신 측에서 데이터 프레임이 손상되지 않았는지 확인하는 데 사용합니다.

## 2️⃣ 데이터 프레임의 동작 원리.

### 1️⃣ 출발지 장치에서 생성.
- **데이터 링크 계층**에서 상위 계층의 데이터를 받아 **프레임**으로 변환합니다.
    - **출발지 MAC 주소와 목적지 MAC 주소가** 프레임에 추가됩니다.

### 2️⃣ LAN 내에서 전송.
- **이더넷 케이블, 스위치** 또는 **무선 엑세스 포인트(WAP, Wireless Access Point)를 통해** 프레임이 LAN(Local Area Network, 근거리 통신망) 내에서 전송됩니다.
    - 스위치(Switch)는 **MAC 주소 테이블**을 사용하여 **목적지 MAC 주소를 분석한 후, 해당 장치로 프레임을 전달합니다.**

### 3️⃣ 수신 측에서 재조립
- **목적지 장치**는 해당 프레임을 수신하고, 오류가 없는지 검사한 후 데이터 링크 계층의 **헤더**와 **트레일러**를 제거하고 상위 계층(네트워크 계층)으로 **데이터를 전달**합니다.

## 3️⃣ 데이터 프레임의 역할

### 1️⃣ 데이터 전달의 기본 단위.
- LAN(Local Area Network, 근거리 통신망) 내에서 **데이터 통신의 기본 단위로 사용됩니다.**
    - 이더넷(Ethernet), Wi-Fi 등 다양한 LAN 기술에서 데이터를 전송할 때 프레임이 사용됩니다.

### 2️⃣ MAC(Media Access Control)주소를 통한 장치 식별.
- **MAC(Media Access Control) 주소**를 사용하여 LAN(Local Access Network, 근거리 통신망) 내에서 **장치를 식별**하고, 데이터 프레임을 해당 장치로 전달합니다.

### 3️⃣ 에러 감지.
- 프레임에 포함된 **FCS(Frame Check Sequence)를** 통해 데이터 전송 중 **오류 감지를** 수행합니다.
    - 오류가 발생한 경우 데이터 프레임은 폐기되며, 필요시 재전송됩니다.

## 4️⃣ 데이터 프레임 전송 과정 예시.
- 1️⃣ 컴퓨터 A에서 컴퓨터 B로 데이터를 전송하려면, 컴퓨터 A는 컴퓨터 B의 **MAC 주소를** 사용하여 **데이터 프레임을 생성합니다.**
- 2️⃣ 이 프레임은 스위치 등 네트워크 장치를 거쳐 **컴퓨터 B**로 전달됩니다.
- 3️⃣ 컴퓨터 B는 자신의 **MAC(Media Access Control) 주소와 일치하는 프레임을 수신하고, 해당 데이터를 처리합니다.**

## 5️⃣ 결론.
- **LAN(Local Area Network, 근거리 통신망) 내에서 데이터 프레임은** 네트워크에서 **데이터를 전송**하기 위한 **기본 단위**입니다.
- 프레임은 데이터 링크 계층에서 **MAC(Media Access Control) 주소를 사용하여 출발지와 목적지 장치를 식별**하며, 데이터를 전송하는 동안 **오류 감지** 기능을 수행합니다.
- 이는 이더넷(Ethernet) 기반 네트워크에서 **데이터를 신뢰성 있게 전송하는 핵심 요소**입니다.
