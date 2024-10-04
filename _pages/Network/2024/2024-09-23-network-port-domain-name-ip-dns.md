---
title: "🌐[Network] 네트워크, 포트, 도메인 이름, IP, DNS"
tags:
    - Network
date: "2024-09-23"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] 네트워크, 포트, 도메인 이름, IP, DNS.

## 1️⃣ 네트워크란 무엇인가요?
- **네트워크(Network)** 는 여러 대의 컴퓨터나 장치들이 서로 연결되어 데이터를 주고받는 시스템을 말합니다.
- 네트워크는 이러한 장치들 간에 정보를 교환하고 자원을 공유하는 것을 가능하게 합니다.
- 인터넷을 포함한 모든 디지털 통신 환경에서 네트워크는 필수적인 요소이며, 네트워크의 규모나 종류에 따라 다양한 방식으로 구현됩니다.

### 1. 네트워크의 주요 개념.
- **1. 장치(노드, Node).**
    - 네트워크에 연결된 컴퓨터, 스마트폰, 서버, 라우터 등 각 장치는 "노드"라고 부르며, 이들은 서로 데이터를 주고받습니다.
- **2. 통신 매체.**
    - 네트워크에서 장치들이 데이터를 주고받기 위해 사용하는 물리적 또는 무선 매체입니다.
    - 대표적인 예로는 유선 네트워크와 무선 네트워크가 있습니다.
        - **유선 네트워크.**
            - 이더넷 케이블, 광섬유 케이블 등을 통해 데이터가 전송됩니다.
        - **무선 네트워크.**
            - 와이파이, 블루투스, 5G와 같은 무선 통신 기술을 통해 데이터를 주고받습니다.
- **3. 프로토콜.**
    - 네트워크에서 통신하기 위한 규약이나 규칙을 말합니다.
    - **TCP/IP(Transmission Control Protocol / Internet Protocol)** 가 대표적인 프로토콜로, 인터넷에서 데이터를 주고 받는데 사용됩니다.
- **4. 네트워크 주소.**
    - 각 장치는 네트워크 상에서 고유한 주소(IP 주소)를 가지고 있으며, 이를 통해 데이터를 정확한 대상에게 전송할 수 있습니다.

### 2. 네트워크의 유형.
- **1. LAN(Local Area Network, 근거리 통신망)**
    - 작은 범위에서 사용되는 네트워크로, 학교, 회사, 가정 내에서 장치들을 연결할 때 사용됩니다.
    - 빠른 속도와 낮은 지연 시간으로 장치들 간에 자원을 쉽게 공유할 수 있습니다.
    - 예: 가정에서 사용되는 와이파이 네트워크, 회사의 내부 네트워크 등.
- **2. WAN(Wide Area Network, 광역 통신망)**
    - 넓은 지리적 범위를 아우르는 네트워크로, 도시, 국가, 전 세계에 걸쳐 여러 LAN을 연결합니다.
    - 대표적인 예로 인터넷이 있으며, 전 세계의 컴퓨터와 장치를 연결합니다.
- **3. MAN(Metropolitan Area Network, 도시권 통신망)**
    - 도시 또는 특정 지역 내에서 여러 LAN을 연결하여 형성된 네트워크입니다.
    - LAN보다 범위는 넓지만 WAN보다는 좁은 범위를 커버합니다.
    - 도시 내 기업 본사의 여러 지점을 연결하는 경우가 이에 해당합니다.
- **4. PAN(Personal Area Network, 개인 통신망)**
    - 개인 장치들 간의 네트워크로, 주로 블루투스 또는 USB로 연결된 스마트폰, 태블릿, 노트북 등의 장치들이 여기에 속합니다.

### 3. 네트워크의 역할.
- **1. 데이터 공유.**
    - 네트워크를 통해 파일, 이미지, 문서 등을 서로 다른 장치로 전송하고, 사용자들이 여러 자원을 함께 사용할 수 있습니다.
        - 예: 네트워크 프린터를 통해 여러 컴퓨터에서 한 대의 프린터를 사용할 수 있습니다.
- **2. 자원 공유.**
    - 네트워크를 통해 서버, 프린터, 스토리지와 같은 하드웨어 자원을 여러 사용자나 장치가 공동으로 사용할 수 있습니다.
- **3. 애플리케이션 지원.**
    - 웹 브라우저, 이메일 클라이언트, 클라우드 서비스와 같은 애플리케이션 네트워크를 통해 통신하고 데이터를 주고 받습니다.

### 4. 네트워크의 구성 요소.
- **1. 라우터(Router)**
    - 네트워크 상에서 데이터를 전달하는 장치로, 여러 네트워크 간의 트래픽을 관리하고 최적의 경로를 찾아 데이터를 전달합니다.
- **2. 스위치(Switch)**
    - 네트워크 내 장치들이 데이터를 주고받을 수 있도록 연결해주는 장치로, 주로 LAN에서 사용됩니다.
- **3. 방화벽(Firewall)**
    - 네트워크 보안을 위해 외부의 불법적인 접근을 차단하고, 네트워크 내부의 데이터를 보호하는 장치입니다.
- **4. 서버(Server)**
    - 네트워크에서 데이터를 제공하거나 처리하는 장치로, 웹 페이지 제공, 데이터 저장, 애플리케이션 실행 등을 담당합니다.

### 5. 네트워크 프로토콜.
- **1. TCP/IP**
    - 인터넷과 대부분의 네트워크에서 사용되는 핵심 프로토콜로, 데이터를 패킷으로 나누어 전달하고 목적지에서 재조합하는 방식입니다.
- **2. HTTP/HTTPS**
    - 웹 브라우저와 서버 간에 데이터를 주고받는 프로토콜입니다.
    - HTTP는 SSL/TLS 암호화를 통해 보안을 강화한 버전입니다.
- **3. FTP**
    - 파일 전송 프로토콜로, 네트워크 상에서 파일을 업로드하거나 다운로드하는 데 사용됩니다.
- **4. DNS**
    - 도메인 이름을 IP 주소로 변환하는 시스템으로, 사람이 기억하기 쉬운 도메인 이름을 통해 해당 IP 주소의 서버에 접근할 수 있게 해줍니다.

### 6. 네트워크의 보안.
- 네트워크는 외부 공격이나 악의적인 사용자로부터 보호하기 위해 다양한 보안 장치와 소프트웨어를 사용합니다.
- 네트워크 보안에는 **암호화, 인증, 방화벽 설정, 침입 탐지 시스템(IDS) 등** 이 포함됩니다.

### 7. 요약.
- 네트워크는 여러 컴퓨터나 장치들이 상호 연결되어 데이터를 주고받을 수 있도록 하는 시스템입니다.
- 네트워크는 장치 간의 데이터 및 자원 공유를 가능하게 하며, LAN, WAN, PAN 등의 다양한 유형으로 구현될 수 있습니다.
- 인터넷 역시 세계 최대읜 네트워크이며, 네트워크는 우리 일상 생활과 비즈니스 환경에서 필수적인 요소로 자리 잡고 있습니다.

## 2️⃣ Port란 무엇인가요?
- 네트워크에서 **포트(port)** 는 컴퓨터나 네트워크 장치에서 특정한 프로세스나 서비스를 구분하기 위해 사용되는 가상의 논리적 통신 경로입니다.
- 네트워크 상에서 하나의 장치가 여러 서비스를 동시에 제공할 수 있기 때문에, **포트 번호** 는 동일한 IP 주소 내에서 서로 다른 서비스들을 구분하는 중요한 역할을 합니다.

### 1. 포트의 주요 개념.
- **1. 포트 번호.**
    - 포트는 숫자로 구분되며, 일반적으로 0번부터 65535번까지의 범위를 가집니다.
    - 포트 번호는 통신하는 애플리케이션이나 프로세스를 구별하는 데 사용됩니다.
        - **0번 ~ 1023번 : 잘 알려진 포트(well-known ports)** 로, 주요 프로토콜이나 서비스가 이 범위의 포트를 사용합니다.
            - 예시
                - HTTP(웹) : 포트 80
                - HTTPS(보안 웹) : 포트 443
                - FTP(파일 전송) : 포트 21
                - SMTP(이메일 전송) : 포트 25
        - **1024번 ~ 49151번 : 등록된 포트(registered ports)** 로, 특정 애플리케이션이 예약하여 사용하는 포트입니다.
        - **49152번 ~ 65535번 : 동적 포트 / 사설 포트(dynamic or private ports)** 로, 임시 연결이나 사용자 정의 애플리케이션에서 사용합니다.
- **2. 포트의 역할.**
    - 네트워크 상에서 클라이언트가 서버에 접속할 때, IP 주소를 통해 서버의 위치를 식별하고, 포트 번호를 통해 해당 서버에서 동작하는 특정 서비스(프로세스)를 구분합니다.
    - 예를 들어, 웹 브라우저가 `example.com`에 접속할 때, 기본적으로 HTTP 요청을 위해 포트 80을 사용하거나 HTTPS 요청을 위해 포트 443을 사용합니다.

### 2. 포트의 사용 예시.
- **클라이언트-서버 통신.**
    - 서버는 특정 포트에서 대기 상태(listening)로 클라이언트의 요청을 기다립니다.
        - 예를 들어, 웹 서버는 보통 포트 80(HTTP) 또는 443(HTTPS)에서 요청을 수신합니다.
    - 클라이언트는 서버의 IP 주소와 함께 특정 포트로 요청을 보냅니다.
    - 이때 클라이언트는 임시 포트를 사용하여 서버와의 통신 경로를 설정합니다.
- **동시 실행 서비스 구분.**
    - 동일한 서버에서 여러 서비스가 동작하는 경우, 각각의 서비스는 다른 포트를 통해 통신을 처리합니다.
        - 예를 들어, 한 서버에서 웹 서버가 포트 80을 사용하면서 동시에 데이터베이스 서버는 포트 3306(MySQL)에서 실행될 수 있습니다.

### 3. 포트 번호를 통한 통신 예시.
- **웹 브라우저와 웹 서버 간의 통신.**
    - 1. 사용자가 웹 브라우저에 `http://example.com`을 입력하면, 브라우저는 DNS 서버를 통해 `example.com`의 IP 주소를 확인합니다.
    - 2. 브라우저는 그 IP 주소로 포트 80(HTTP)을 통해 요청을 보냅니다.
    - 3. 웹 서버는 포트 80에서 클라이언트의 요청을 수신하고, 처리 후 응답을 보냅니다.
    - 4. 이때, 클라이언트는 자신의 컴퓨터에서 임의의 동적 포트(예: 50000번대)를 사용하여 통신을 시작합니다.

### 4. 방화벽과 포트.
- 네트워크 보안에서는 **방화벽(firewall)** 이 중요한 역할을 하며, 포트 기반으로 네트워크 트래픽을 제어합니다.
- 방화벽은 허용된 포트에서만 트래픽을 수신하거나 송신하도록 설정할 수 있습니다.
    - 예를 들어, 웹 서버는 80번과 443번 포트만 열어 두고, 나머지 포트는 차단하여 보안을 강화할 수 있습니다.

### 5. 요약
- 포트는 네트워크 상에서 동일한 IP 주소를 사용하는 여러 서비스나 애플리케이션을 구분하기 위한 숫자 기반의 논리적 경로입니다.
- 포트 번호는 클라이언트와 서버 간의 통신에서 서비스를 식별하고, 각 포트는 특정 애플리케이션이 네트워크 트래픽을 처리할 수 있게 해줍니다.
- 특정 포트는 표준 서비스에 예약되어 있으며, 네트워크 보안에서 방화벽 등을 통해 포트를 관리하고 보호할 수 있습니다.

## 3️⃣ 도메인 이름(domain name)이란 무엇인가요?
- 도메인 이름(domain name)은 인터넷에서 웹사이트나 특정 네트워크 자원을 식별하기 위해 사용되는 고유한 텍스트 기반의 주소입니다.
- 도메인 이름은 사람이 읽을 수 있는 형태로, IP 주소와 연결되어 있습니다.
- 실제로 인터넷에서 모든 장치와 서버는 숫자로 된 IP 주소로 통신하지만, 사람이 기억하고 사용하기 편리하도록 도메인 이름을 사용합니다.

### 1. 도메인 이름의 구성.
- 도메인 이름은 여러 부분으로 구성되며, 각 부분은 점(`.`)으로 구분됩니다.

#### 일반적인 도메인 이름의 구성요소.
- **최상위 도메인(TLD, Top-Level Domain)**
    - 도메인의 가장 마지막 부분으로, 주로 `.com`, `.org`, `.net`, `.gov` 등의 일반적인 TLD 또는 국가 코드 도메인(`.kr`, `.jp`, `.uk` 등)으로 끝납니다.
        - 예: `example.com` 에서 `.com`이 TLD 입니다.
- **두 번째 레벨 도메인(SLD, Second-Level Domain)**
    - TLD 바로 앞에 위치하며, 주로 회사나 단체의 이름 또는 웹사이트의 이름이 들어갑니다.
        - 예: `example.com` 에서 `example`이 SLD입니다.
- **서브도메인**
    - 때때로 SLD 앞에 추가되는 부분으로, 주로 특정 하위 섹션이나 서비스를 나타냅니다.
        - 예: `blog.example.com`에서 `blog`가 서브 도메인입니다.
- 따라서, 도메인 이름은 다음과 같은 구조를 가집니다.

```bash
서브 도메인.두 번째 레벨 도메인.최상위 도메인
```
- 예: `www.example.com`에서
    - `www`는 서브도메인(생략 가능)
    - `example`은 두 번째 레벨 도메인
    - `com`은 최상위 도메인

### 2. 도메인 이름의 역할.
- **1. IP 주소 대체.**
    - 도메인 이름은 사람이 읽기 쉽고 기억하기 쉽도록 IP 주소(예: `192.198.1.1`)를 대신합니다.
    - DNS(Domain Name System) 서버는 도메인 이름을 해당 IP 주소로 변환하는 역할을 하여 웹 브라우저가 올바른 서버에 연결되도록 합니다.
- **2. 식별 및 브랜드.**
    - 도메인 이름은 특정 웹사이트나 서비스의 정체성을 나타내며, 회사나 단체, 개인의 온라인 존재를 대표합니다.
    - 예를 들어, `google.com`은 구글의 웹사이트를 식별하는 데 사용됩니다.
- **3. 웹 주소(URL).**
    - 도메인 이름은 웹 주소(URL)의 중요한 부분을 구성합니다.
    - URL은 도메인 이름과 더불어 특정 페이지나 리소스를 가리킵니다.
    - 예: `https://www.example.com/about`에서 `www.example.com`이 도메인 이름이고, `/about`은 특정 페이지 경로입니다.

### 3. 도메인 이름의 등록.
- 도메인 이름을 소유하려면 도메인 등록 기관을 통해 원하는 도메인을 등록해야 합니다.
- 등록된 도메인은 일정 기간동안(주로 1년 단위) 사용 가능하며, 이후 갱신해야 합니다.
- 도메인 이름은 전 세계적으로 고유해야 하므로, 이미 등록된 도메인은 다른 사람이 사용할 수 없습니다.

### 4. 요약.
- 도메인 이름은 IP 주소를 대신해 인터넷 상의 리소스를 쉽게 식별하고 접근할 수 있도록 만들어진 텍스트 기반의 주소입니다.
- 이를 통해 사람들은 기억하기 쉬운 이름을 사용하여 웹사이트에 접근할 수 있고, 도메인 이름은 네트워크 자원의 온라인 정체성을 나타내는 중요한 역할을 합니다.

## 4️⃣ IP란 무엇인가요?
- IP(Internet Protocol)는 인터넷이나 네트워크 상에서 데이터를 주고 받기 위한 규약(프로토콜)입니다.
- IP는 네트워크 상에서 장치들이 서로를 식별하고 통신할 수 있도록 하는 주소 체계와 데이터 전송 방식을 정의합니다.
- 즉, IP는 인터넷 통신의 기본이 되는 프로토콜로, 모든 인터넷 연결 장치는 고유한 IP 주소를 가지고 있으며, 이 주소를 통해 데이터를 주고 받습니다.

### 1. IP 주소.
- **IP 주소(Internet Protocol Address)** 는 네트워크에 연결된 각 장치(컴퓨터, 스마트폰, 서버 등)를 식별하기 위해 할당된 고유한 숫자입니다.
- IP 주소는 컴퓨터 네트워크 상에서 데이터를 송수신할 때 발신자와 수신자의 주소 역할을 합니다.
- **IPv4** 와 **IPv6** 두 가지 버전이 있습니다.
    - **IPv4**
        - 32비트 주소 체계를 사용하여 약 43억 개의 주소를 제공합니다.
        - IPv4 주소는 보통 4개의 10진수로 표현되며, 예를 들어 "192.168.0.1"과 같은 형태를 띱니다.
    - **IPv6**
        - 128비트 주소 체계를 사용하여 훨씬 더 많은 주소를 제공하여, 점점 더 많은 장치가 인터넷에 연결됨에 따라 IPv6가 필요하게 되었습니다.
        - IPv6 주소는 16진수로 표현되며, 예를 들어"2001:0db8:85a:0000:0000:8a2e:0370:7334" 같은 형태를 가집니다.

### 2. IP의 기능.
- IP는 데이터를 네트워크 상에서 한 장치에서 다른 장치로 전송하는 데 필요한 방법과 경로를 결정합니다.
- 이 과정은 여러 단계를 거쳐 이루어지며, IP는 각 데이터를 작은 패킷으로 나누어 전달하는 역할을 합니다.
    - **패킷 분할**
        - 데이터를 작은 패킷으로 분할하여 전송합니다.
        - 이 패킷들은 각기 다른 경로를 통해 목적지로 전달될 수 있으며, 도착 후 다시 조립되어 원래의 데이터로 복원됩니다.
    - **주소 지정**
        - IP는 발신자와 수신자의 IP 주소를 포함하여 패킷이 어느 위치에서 출발하고 어느 위치로 가야 하는지 명확히 합니다.
    - **라우팅**
        - IP 패킷은 여러 네트워크 장비(라우터 등)를 통해 목적지로 전달됩니다.
        - IP 프로토콜은 이 과정에서 패킷이 최적의 경로로 이동할 수 있도록 라우팅합니다.

### 3. IP의 역할.
- **데이터 전달.**
    - IP는 네트워크 상에서 데이터를 전달하는 가장 기본적인 프로토콜입니다.
    - 이는 웹 페이지 로드, 이메일 송수신, 파일 전송 등 거의 모든 네트워크 작업에서 사용됩니다.
- **네트워크 계층의 역할.**
    - IP는 OSI(Open System Interconnection) 모델의 네트워크 계층(3계층)에서 작동하며, 데이터를 패킷으로 분할하고 이를 전송할 때 사용되는 주소 체계와 경로 결정 방식을 담당합니다.
- **무신뢰성.**
    - IP 자체는 "무신뢰성"이 있는 프로토콜입니다.
    - 즉, 데이터를 전송할 때 손실이 발생할 수 있으며, 데이터를 정확히 전달했는지 확인하지 않습니다.
    - 이러한 신뢰성 보장은 TCP(Transmission Control Protocol)와 같은 상위 계층 프로토콜에서 처리됩니다.

### 4. IP와 TCP.
- **TCP/IP**
    - IP는 일반적으로 TCP와 함께 사용되며, 이 두 프로토콜을 묶어서 **TCP/IP** 프로토콜 스택이라고 부릅니다.
    - TCP는 데이터의 신뢰성을 보장하고, IP는 데이터를 목적지로 전송하는 역할을 합니다.
    - TCP는 데이터를 손실 없이 정확히 전송했는지 확인하고, 필요한 경우 다시 요청하여 데이터의 신뢰성을 유지합니다.

### 5. 요약.
- IP(Internet Protocol)는 네트워크 상에서 데이터를 전송하기 위한 규약으로, IP 주소를 사용하여 장치를 식별하고 데이터를 전송합니다.
- IP는 인터넷 통신의 기본이 되는 중요한 요소로, 패킷을 통해 데이터를 전달하고 이를 최적의 경로로 라우팅하는 역할을 합니다.

## 5️⃣ DNS.
- **DNS(Domain Name System)** 는 사람이 읽을 수 있는 도메인 이름(예: `www.example.com`)을 컴퓨터가 이해할 수 있는 IP 주소(예: `192.198.1.1` 또는 IPv6 주소)로 변환하는 시스템입니다.
- DNS는 인터넷에서 도메인 이름을 기반으로 웹 사이트나 서비스에 접근할 수 있게 하는 핵심 인프라입니다.

### 1. DNS의 주요 역할.
- 인터넷 상의 모든 컴퓨터는 서로 통신하기 위해 IP 주소를 사용합니다.
- 그러나 IP 주소는 사람이 기억하기 어렵기 때문에 도메인 이름을 사용하여 웹 사이트나 서비스를 쉽게 접근할 수 있도록 하는 것이 DNS의 역할입니다.
- DNS는 다음과 같은 주요 기능을 수행합니다.
    - **1. 도메인 이름을 IP 주소로 변환**
        - 사용자가 웹 브라우저에 도메인 이름을 입력하면, DNS는 해당 도메인 이름에 매핑된 IP 주소를 반환하여 사용자가 방문하려는 서버와 통신할 수 있게 합니다.
            - 예: 사용자가 `www.google.com`을 입력하면, DNS 서버는 이 도메인에 해당하는 IP 주소를 찾아 브라우저에 전달합니다.
    - **2. 분산형 데이터베이스**
        - DNS는 전 세계에 걸쳐 분산된 데이터베이스 시스템으로 구성되어 있으며, 도메인 이름과 IP 주소 매핑 정보를 효율적으로 관리하고 제공합니다.

### 2. DNS의 동작 과정.
- DNS는 웹 사이트에 접속할 때 자동으로 작동하는 백그라운드 프로세스입니다. 그 과정은 다음과 같습니다.
    - **1. 도메인 이름 입력**
        - 사용자가 웹 브라우저에 도메인 이름(예: `www.example.com`)을 입력합니다.
    - **2. 로컬 DNS 확인**
        - 사용자의 컴퓨터는 먼저 **로컬 캐시(컴퓨터에 저장된 최근의 DNS 조회 기록)** 를 확인하여 해당 도메인 이름의 IP 주소가 저장되어 있는지 확인합니다.
        - 만약 로컬 캐시에 정보가 없다면, 컴퓨터는 설정된 **DNS 서버** 로 요청을 보냅니다.
    - **3. DNS 서버 요청**
        - 사용자의 컴퓨터는 ISP(인터넷 서비스 제공자)에서 제공한 **DNS 서버** 에 도메인 이름의 IP 주소를 요청합니다.
    - **4. DNS 서버의 재귀적 조회**
        - **루트 DNS 서버**
            - 도메인 이름이 처음 입력되면, DNS 서버는 루트 DNS 서버에 도메인의 최상위 도메인(TLD, 예: `.com`)에 대한 정보를 요청합니다.
        - **TLD 서버**
            - 루트 서버는 TLD 서버(예: `.com` 도메인을 관리하는 서버)의 주소를 반환하고, DNS 서버는 해당 TLD 서버에 요청을 보냅니다.
        - **권한 있는 DNS 서버**
            - TLD 서버는 해당 도메인의 권한 있는 DNS 서버(예: `example.com`에 대한 DNS 서버)의 IP 주소를 반환합니다.
            - 권한 있는 DNS 서버는 최종적으로 해당 도메인의 IP 주소를 DNS 서버에 전달합니다.
    - **5. IP 주소 반환**
        - DNS 서버는 IP 주소를 받아 사용자에게 반환합니다.
        - 그런 다음 브라우저는 이 IP 주소로 해당 서버에 연결하여 웹 페이지를 로드합니다.
    - **6. 캐시 저장**
        - 이 과정에서 조회된 IP 주소는 캐시에 저장되어 이후 동일한 도메인에 대한 요청이 있을 때 더 빠르게 처리됩니다.

### 3. DNS의 주요 구성 요소.
- **1. DNS 클라이언트**
    - 사용자의 컴퓨터나 스마트폰과 같은 장치에서 DNS 요청을 발생시키는 프로그램입니다.
    - 주로 웹 브라우저가 이러한 역할을 수행합니다.
- **2. DNS 서버**
    - 도메인 이름과 IP 주소 매핑 정보를 저장하고 관리하는 서버입니다.
    - 다양한 유형의 DNS 서버가 존재합니다.
        - **재귀 DNS 서버**
            - 사용자의 요청을 받아 필요한 정보를 찾아주는 서버입니다.
            - ISP에서 제공하는 기본 DNS 서버입니다.
        - **권한 있는 DNS 서버(Authoritative DNS Server)**
            - 특정 도메인에 대한 최종적인 정보를 제공하는 서버로, 그 도메인의 IP 주소를 관리합니다.
        - **루트 DNS 서버**
            - 인터넷의 최상위 DNS 서버로, 도메인 이름의 최상위 도메인(TLD)에 대한 정보를 제공합니다.
- **3. DNS 레코드**
    - DNS 서버에 저장된 도메인과 관련된 정보입니다.
    - DNS 레코드는 여러 유형이 있으며, 각각 고유한 목적을 가집니다.
        - **A 레코드**
            - 도메인 이름을 IPv4 주소로 매핑합니다.
        - **AAAA 레코드**
            - 도메인 이름을 IPv6 주소로 매핑합니다.
        - **CNAME 레코드**
            - 하나의 도메인 이름을 다른 도메인 이름에 매핑합니다.
        - **MX 레코드**
            - 이메일 전송을 위해 메일 서버의 도메인 이름을 정의합니다.

### 4. DNS 캐싱.
- DNS는 성능을 향상시키기 위해 캐싱을 사용합니다.
- 캐싱은 DNS 서버나 사용자 장치에 이전에 조회한 DNS 정보를 일정 시간 동안 저장하여, 동일한 요청이 있을 때 더 빠르게 처리할 수 있도록 합니다.
- 캐시된 정보는 TTL(Time To Live)이라는 제한 시간 동안 유효하며, 이 시간이 지나면 새로운 정보를 조회합니다.

### 5. DNS의 중요성.
- **1. 인터넷 사용의 편리성**
    - DNS 덕분에 우리는 복잡한 숫자로 된 IP 주소를 외울 필요 없이, 도메인 이름을 통해 웹 사이트에 접속할 수 있습니다.
- **2. 네트워크 효율성**
    - DNS는 트래픽을 효율적으로 관리하며, 각 요청을 빠르게 처리할 수 있도록 분산된 시스템으로 구성되어 있습니다.
- **3. 보안**
    - DNS는 네트워크 바안에서도 중요한 역할을 합니다.
    - 그러나 DNS 요청이 가로채기(예: DNS 스푸핑)에 취약할 수 있어, 이를 방지하기 위해 **DNSSEC(DNS Security Extension)** 와 같은 보안 기술이 도입되었습니다.

### 6. 요약.
- DNS는 인터넷의 전화번호부와 같은 역할을 하며, 도메인 이름을 IP 주소로 변환하여 사용자가 쉽게 웹 사이트나 서비스를 사용할 수 있게 해줍니다.
- DNS는 분산된 시스템으로 동작하며, 여러 DNS 서버들이 협력하여 사용자의 요청을 처리합니다.
- 이를 통해 인터넷 사용자들은 간편하게 네트워크 자원에 접근할 수 있습니다.