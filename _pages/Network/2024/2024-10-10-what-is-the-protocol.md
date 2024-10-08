---
title: "🌐[Network] 프로토콜(Protocol)이란 무엇일까요?"
tags:
    - Network
date: "2024-10-10"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] 프로토콜(Protocol)이란 무엇일까요?
- **프로토콜(Protocol)** 은 **컴퓨터나 네트워크 장치간에 데이터를 주고 받기 위한 일련의 규칙과 절차를 정의한 표준입니다.**
    - 쉽게 말해, **통신하는 장치들이 서로 이해하고 데이터 교환을 원활히 할 수 있도록 정해진 약속**을 말합니다.
- 프로토콜은 데이터 형식, 통신 속도, 오류 처리 방식, 메시지 구조 등 통신에 필요한 모든 요소를 정의합니다.

## 1️⃣ 프로토콜의 주요 역할.

### 1️⃣ 통신 규칙 제공.
- 서로 다른 장치나 시스템이 데이터를 주고받기 위해 따라야 할 규칙을 제공합니다.
    - 각 장치나 시스템이 동일한 프로토콜을 따름으로써, 서로 원활하게 정보를 주고받을 수 있습니다.

### 2️⃣ 데이터 형식 정의.
- 데이터를 어떻게 표현하고 전송할지에 대한 형식을 정의합니다.
    - 예를 들어, 메시지의 시작과 끝을 어떻게 구분할지, 데이터의 크기나 구조는 어떻게 할지 등을 결정합니다.

### 3️⃣ 오류 검출 및 수정.
- 데이터 전송 중 발생할 수 있는 오류를 감지하고, 이를 처리하는 방식을 정의합니다.
    - 통신 중 데이터가 손상되거나 유실될 경우 이를 해결하기 위한 절차가 포함됩니다.

### 4️⃣ 동기화
- 데이터를 주고 받는 송신자와 수신자 간의 동기화를 처리하여, 두 장치가 서로의 데이터를 올바르게 이해하고 처리할 수 있도록 돕습니다.

## 2️⃣ 프로토콜의 종류.
- 네트워크 통신에서 사용되는 프로토콜은 **OSI(Open Systems Interconnection) 모델**이나 **TCP/IP 모델**의 계층에 따라 나눌 수 있습니다.
    - 각 계층은 서로 다른 프로토콜을 사용하여 특정 기능을 수행합니다.

### 1️⃣ 네트워크 계층별 프로토콜.
- 프로토콜은 네트워크 통신의 각 단계에 해당하는 다양한 역할을 담당합니다.

### 👉 응용 계층(Application Layer)
- **HTTP(HyperText Transfer Protocol)**
    - 웹 브라우저와 웹 서버 간에 데이터를 주고받기 위한 프로토콜로, 웹 페이지의 전송을 담당합니다.
- **FTP(File Transfer Protocol)**
    - 컴퓨터 간에 파일을 전송하는 데 사용되는 프로토콜입니다.
- **SMTP(Simple Mail Transfer Protocol)**
    - 이메일 전송을 위한 프로토콜입니다.
- **DNS(Domain Name System)**
    - 도메인 이름을 IP 주소로 변환하는 프로토콜입니다.

### 👉 전송 계층(Transport Layer)
- **TCP(Transmission Control Protocol)**
    - 신뢰성 있는 연결형 데이터 전송 프로토콜로, 데이터를 패킷으로 나누어 전송하고, 오류가 발생하면 재전송을 통해 안정성을 보장합니다.
- **UDP(User Datagram Protocol)**
    - 비연결형 프로토콜로, 빠른 데이터 전송이 가능하지만, 데이터 전송의 신뢰성은 보장하지 않습니다.
    - 주로 실시간 스트리밍이나 게임에서 사용됩니다.

### 👉 인터넷 계층(Internet Layer)
- **IP(Internet Protocol)**
    - 데이터를 패킷으로 나누어 목적지까지 전달하는 역할을 하는 프로토콜로, 인터넷에서 기본적인 데이터 전송을 담당합니다.
- **ICMP(Internet Control Message Protocol)**
    - 네트워크 장비 간의 진단이나 오류 메시지를 전송하는 데 사용됩니다.
        - 예: **ping** 명령.

### 👉 네트워크 인터페이스 계층(Network Interface Layer)
- **Ethernet**
    - 로컬 영역 네트워크(LAN)에서 사용되는 물리적 네트워크 프로토콜로, 네트워크 카드 간 데이터 전송을 처리합니다.
- **Wi-Fi**
    - 무선 네트워크에서 사용되는 통신 프로토콜로, 무선 장치 간 데이터를 주고받을 때 사용됩니다.

### 2️⃣ 보안 관련 프로토콜
- **SSL/TLS(Secure Sockets Layer / Transport Layer Security)**
    - 인터넷 상에서 데이터를 안전하게 암호화하여 전송하는 프로토콜입니다.
    - HTTPS는 SSL/TLS를 사용하여 HTTP 통신을 암호화합니다.
- **SSH(Secure Shell)**
    - 원격 시스템에 안전하게 접속하기 위한 프로토콜로, 데이터를 암호화하여 전송합니다.

## 3️⃣ 프로토콜 예시: HTTP 프로토콜
- **HTTP(HyperText Transfer Protocol)** 는 클라이언트와 서버간의 **웹 페이지** 데이터를 주고받는 데 사용하는 프로토콜입니다.
- 클라이언트(예: 웹 브라우저)가 HTTP 요청을 보내면, 서버는 그에 대한 응답을 HTTP 형식으로 반환합니다.
    - **클라이언트의 요청(Request) :** `GET /index.html HTTP/1.1`
    - **서버의 응답(Response) :** `HTTP/1.1 200 OK`
        - 이때, HTTP 프로토콜은 요청과 응답의 구조, 상태 코드(200, 404 등), 메서드(GET, POST 등) 등 모든 통신 규칙을 정의합니다.

## 4️⃣ 프로토콜의 필요성.

### 1️⃣ 호환성 확보.
- 다양한 장치와 시스템 간의 호환성을 보장하기 위해 **표준화된 프로토콜**이 필요합니다.
    - 프로토콜이 없다면 서로 다른 장치나 시스템 간에 데이터를 주고받는 것이 매우 어려워질 것입니다.

### 2️⃣ 통신 표준 제공.
- 프로토콜은 **데이터 형식, 에러 처리 방식, 전송 속도** 등 여러 요소를 표준화하여 네트워크 상의 모든 장치가 동일한 규칙을 따르도록 합니다.
    - 이를 통해 전 세계적으로 인터넷 통신이 가능해집니다.

### 3️⃣ 효율적인 통신.
- 프로토콜은 데이터 전송을 최적화하고, 오류 발생 시 복구할 수 있는 메커니즘을 제공합니다.
    - 또한 통신 과정에서 발생할 수 있는 여러 문제를 해결할 수 있는 방법을 정의하여 안정적이고 효율적인 데이터 전송을 보장합니다.

## 5️⃣ 결론.
- **프로토콜(Protocol)** 은 장치들 간의 통신을 위한 **규칙과 표준**을 정의한 중요한 요소입니다.
- 프로토콜이 없다면 네트워크 상의 다양한 시스템 간의 데이터 교환은 불가능할 것입니다.
- 인터넷과 네트워크 통신에서 프로토콜은 데이터의 정확한 전달을 보장하고, 네트워크 장치 간의 상호 운용성을 유지하기 위한 필수적인 구성 요소입니다.
