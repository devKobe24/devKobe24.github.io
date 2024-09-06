---
title: "🌐[Network] 주소 정보의 관리"
tags:
    - Network
date: "2024-09-05"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] 주소 정보의 관리.
- 일반 사용자가 호스트를 지칭할 때 사용하는 호스트 이름을 도메인 이름(Domain Name)이라 하며, 인터넷에서는 `www.korea.co.kr`과 같은 도메인 이름을 IP 주소로 변환하는 작업이 필요합니다.
- 초기 인터넷에서는 아주 간단한 방법으로 호스트 이름과 IP 주소를 변환하였으나, 지금은 DNS라는 분산 데이터베이스 시스템을 사용해서 보다 체계적인 방법으로 관리하고 있습니다.

## 1️⃣ 호스트 파일.
- 호스트 이름과 IP 주소를 변환하는 간단한 방법은 특정 파일(예: UNIX 시스템의 /etc/hosts)에 호스트 이름과 IP 주소의 조합을 기록하여 관리하는 것입니다.
- 네트워크 응용 프로그램에서는 사용자가 입력한 호스트 이름을 이 파일에서 검색하여 일대일로 대응된 IP 주소 정보를 쉽게 얻을 수 있습니다.
- 호스트 파일은 한 줄에 하나의 호스트 정보가 기록되며, 일반 텍스트 문서 형식으로 보관됩니다.
    - 즉, 아래의 그림을 예로 들면 호스트 이름이 `white.korea.co.kr`인 시스템의 IP 주소는 `211.223.201.27`입니다.

<img src = "https://github.com/devKobe24/images2/blob/main/network/network-hostfile-copy.png?raw=true">

- 네트워크 관리자는 관리 대상이 되는 모든 호스트의 이름, 주소 정보를 주기적으로 갱신하고, 이 정보를 네트워크에 연결된 모든 호스트가 복사하도록 함으로써 정보의 일관성을 유지해야 합니다.
- 위 그림은 네트워크 관리자가 `white.korea.co.kr` 에서 호스트 정보를 갱신할 때 갱신된 정보를 다른 4개의 호스트가 복사하여 저장하는 모습을 보여줍니다.
- 소스트 파일을 갱신하고 복사하는 작업은 보통 시스템 관리자가 수작업으로 했었습니다.
- 호스트가 추가되거나 삭제되면 먼저 네트워크 관리자의 호스트에서 갱신 작업이 이루어집니다
    - 그런데 인터넷이 처음 보급되던 시기에는 호스트 파일 갱신이 생각보다 자주 발생하지 않았기 때문에 호스트 파일을 복사하는 작업도 흔하지 않았습니다.
        - 또한 시스템 관리자가 잦은 변경을 원하지 않아서 급하지 않은 갱신은 부분적으로 늦추기도 했습니다.
            - 하지만 지금은 DNS 서비스가 보편적으로 사용되고 있어 이처럼 호스트 파일로 관리하는 방식은 보조적으로만 사용되고 있습니다.

## 2️⃣ DNS
- 호스트 파일로 주소와 이름 정보를 관리하는 것은 간단하지만 대부분 수동으로 작업해야 한다는 단점이 있습니다.
- 인터넷이 확산되면서 호스트 수가 증가할수록 네트워크 관리자가 호스트 파일을 갱신하고 복사하는 작업에 많은 시간과 노력을 들여야 합니다.
- 특히 지금과 같이 전 세계 컴퓨터가 연결된 네트워크 환경에서는 호스트 파일로 주소와 이름을 변환하는 작업이 사실상 불가능하다고 볼 수 있습니다.
- DNS(Domain Name System)는 이러한 문제점을 해결하기 위하여 고안된 것으로, 주소와 이름 정보를 자동으로 유지하고 관리하는 분산 데이터베이스 시스템입니다.
    - 호스트 주소와 이름 정보는 네임 서버(Name Server)라는 특정한 관리 호스트가 유지하고, 주소 변환 작업이 필요한 클라이언트 네입 서버에서 요청해서 IP 주소를 얻습니다.
- 네트워크가 커지면 네임 서버에 보관되는 정보의 양도 자연스럽게 많아집니다.
- DNS는 하나의 집중화된 네임 서버가 전체 호스트의 정보를 관리하지 않고, 여러 네임 서버에 분산하여 관리하도록 설계되었습니다.
    - 계층 구조로 연결된 네임 서버는 자신이 관리하는 영역에 위치한 호스트 정보만 관리하며, 정보를 상호 교환하는 협력 관계를 통해서 전체 호스트 정보를 일관성 있게 유지합니다.

## 3️⃣ 기타 주소
- 네트워크에서 사용하는 주소는 이를 사용하는 환경에 따라 다양합니다.
- OSI 7계층 모델의 각 계층에서도 목적에 따라 여러 형태의 주소가 사용됩니다.

### 인터넷에서 일반 사용자가 접할 수 있는 대표적인 주소들.
- **MAC 주소**
    - MAC 주소는 계층 2의 MAC(Medium Access Protocol) 계층에서 사용하며, 일반적으로 LAN 카드에 내장되어 있습니다.
    - 물리 계층을 통해 데이터를 전송할 때는 MAC 주소를 이용해서 호스트를 구분합니다.
        - 따라서 네트워크 계층이 하위의 데이터 링크 계층에 데이터 전송을 요청하면 먼저 IP 주소를 MAC 주소로 변환하는 작업이 이루어지고, 이후 MAC 계층이 상대방 MAC 계층에 데이터를 전송할 수 있습니다.

- **IP 주소**
    - IP 주소는 인터넷에서 네트워크 계층의 기능을 수행하는 IP 프로토콜에서 사용되며, 송신자 IP 주소와 수신자 IP 주소로 구분됩니다.
        - 수신자 IP 주소는 IP 패킷이 지나가는 경로를 결정하는 라우팅의 기준이 됩니다.

- **포트 주소**
    - 포트 주소(Port Address)는 전송 계층에서 사용하며, 호스트에서 실행되는 프로세스를 구분해줍니다.
    - 인터넷에서 연결의 완성은 호스트와 호스트 사이가 아닌, 네트워크 응용 프로세스와 네트워크 응용 프로세스 사이입니다.
        - 예를 들어, 내 스마트폰의 메신저 앱과 상대방 스마트폰의 메신저 앱 사이의 연결이 필요하다.
            - 이때, 하나의 IP 주소를 갖는 스마트폰에서 실행되는 여러 네트워크 응용 앱들을 구분하는 주소가 포트 주소입니다.
                - 인터넷의 전송 계층 프로토콜인 TCP와 UDP가 독립적으로 포트 주소를 관리하며, 포트 번호 또는 소켓 주소라는 용어를 사용하기도 합니다.

- **메일 주소**
    - 메일 주소는 응용 계층의 메일 시스템에서 사용자를 구분하려고 사용합니다.
        - `kobe@korea.co.kr` 처럼 사용자 이름과 호스트 이름을 `@` 문자로 구분해 표기합니다.