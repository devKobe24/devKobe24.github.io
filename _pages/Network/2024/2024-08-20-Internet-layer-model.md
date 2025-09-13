---
title: "🌐[Network] 인터넷의 계층 모델"
tags:
    - Network
date: "2024-08-20"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] 인터넷의 계층 모델

## 1️⃣ 인터넷의 계층 모델.
<img src = "https://github.com/devKobe24/images2/blob/main/network/ftp.png?raw=true">

- 위 그림은 FTP 프로그램을 이용하는 경우를 예로 들어 인터넷의 계층 구조를 설명하고 있습니다.
- 인터넷에서는 IP(Internet Protocol)가 네트워크 계층의 기능을 수행하며, TCP(Transmission Control Protocol)와 UDP(User Datagram Protocol)는 전송 계층의 기능을 수행합니다.
    - 이들 3개의 프로토콜은 인터넷 환경에서 사용자 데이터를 전송하는 핵심 역할을 합니다.
- 전송 계층 이하의 프로토콜들은 호스트의 운영체게 내부에서 구현되며 FTP, 텔넷, 전자 메일 등과 같은 응용 프로그램은 사용자 프로그램 환경에서 계층 5~7이 합쳐져 구현됩니다.
- 위 그림과 같이 양쪽 호스트에는 동일한 기능을 수행하는 프로토콜 스택(Protocol Stack)이 존재합니다.
    - 프로토콜 스택은 계층 구조로 이루어진 통신 프로토콜의 집합입니다.
<img src = "https://github.com/devKobe24/images2/blob/main/network/Interface-and-Protocol.png?raw=true">

- 각 프로토콜의 동작은 위 그림에서 설명한 것과 같은 원리로 이루어집니다.
    - 그림에 표시되진 않았지만 두 호스트 사이에는 중개 기능을 수행하는 라우터들이 존재할 수 있습니다.
- 인터넷에서는 IP 프로토콜이 중개 기능을 수행하므로 라우터에는 계층 3까지의 프로토콜이 구현되어 있습니다.
- FTP 클라이언트가 FTP 서버에 데이터를 전송하는 과정은 다음과 같습니다.
    - FTP 클라이언트가 FTP 서버에 직접 데이터를 전송하는 것은 불가능하므로 먼저 자신의 하위 TCP에 데이터를 보내야합니다.
    - TCP로 보내진 데이터는 IP 프로토콜과 LAN 카드를 거쳐서 이더넷으로 표현된 전송 매체를 통하여 FTP 서버의 LAN 카드에 전달됩니다.
    - FTP 서버에 도착한 데이터는 송신 순서의 반대인 LAN 카드, IP 프로토콜, TCP 프로토콜을 거쳐서 FTP 서버 프로그램에 도착합니다.
        - FTP 서비스에서 데이터 전송은 양방향 통신을 지원하므로 반대 방향의 전송도 가능합니다.
