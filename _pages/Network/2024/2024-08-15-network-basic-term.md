---
title: "🌐[Network] 네트워크 기초 용어."
tags:
    - Network
date: "2024-08-15"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] 네트워크 기초 용어.
- 이미 수많은 사람이 익숙하게 사용하고 있는 인터넷(Internet)은 연구소, 기업, 학교 등의 소규모 조직에서 사용하기 시작한 작은 단위의 네트워크(Network)들을 서로 연결하면서 발전하였습니다.
    - 그 과정에서 자연스럽게 연결 방식의 표준화를 요구하게 되었고, 오늘날 전 세계로 확산되어 거대한 인터넷으로 성장하였습니다.
- 네트워크를 이해하려면 시스템, 인터페이스, 전송 매체, 프로토콜, 네트워크, 인터넷과 같은 용어를 먼저 이해해야 합니다.
- 네트워크(Network)는 하드웨어적인 전송 매체(Transmission Media)를 매개로 서로 연결되어 데이터를 교환하는 시스템(System)의 모음이며, 시스템과 전송 매체의 연결 지점에 대한 규격이 인터페이스(Interface)입니다.
- 시스템이 데이터를 교환할 때는 소프트웨어적으로 동작하는 통신 규칙인 프로토콜(Protocol)이 필요합니다.
- 인터페이스와 프로토콜은 서로 다른 시스템을 상호 연동해 동작시키기 위함이니 반드시 연동 형식의 통일이 필요하고, 이를 표준화(Standardization)라 합니다.

<img src = "https://github.com/devKobe24/images2/blob/main/network/network-1.png?raw=true">
- 위 그림은 여러 시스템이 전송 매체로 연결되어 네트워크를 구성한 예입니다.
- 시스템은 반드시 일반 컴퓨터일 필요는 없으며, 보통 컴퓨팅 기능을 보유한 네트워크 장비들을 의미합니다.
- 그림과 같은 네트워크의 가장 바깥쪽에 스마트폰을 포함한 일반 사용자들의 컴퓨터가 연결되어 데이터 교환 작업을 수행합니다.
- 시스템들은 물리적으로 공유하는 전송 매체에 의하여 서로 연결되지만, 시스템이 전송 매체를 통해 데이터를 교환하려면 반드시 표준화된 프로토콜을 사용해야 합니다.
- 우리가 알고 있는 인터넷은 IP(Internet Protocol)라는 네트워크 프로토콜이 핵심적인 역할을 하는 네트워크의 집합체입니다.
    - 여기서 IP는 프로토콜의 의미가 포함된 약자이지만 보통 IP 프로토콜이라 부릅니다.

## 1️⃣ 시스템(System)
- 내부 규칙에 따라 자율적으로 동작하는 대상을 가리킵니다.
    - 자동차, 커피 자판기, 컴퓨터, 마이크로프로세서, 하드디스크 등과 같은 물리적인 대상뿐 아니라, 신호등으로 교통을 제어하는 운영 시스템, Mac OS 등의 운영체제, 프로그램의 실행 상태를 의미하는 프로세스와 같은 소프트웨어적인 대상들도 시스템입니다.
> 🤩 TIP: 네트워크 환경에서 동작하는 임의의 시스템은 다른 시스템과 데이터를 교환하는 기능이 필수적입니다.
- 시스템의 동작에 필요한 외부 입력이 있을 수 있으며, 내부 정보와 외부 입력의 조합에 따른 출력(시스템 실행의 결과물)이 있을 수 있습니다.
- 한편, 작은 시스템이 여러 개 모여 더 큰 시스템을 구성할 수 있으므로 크기를 기준으로 시스템을 나누지는 않습니다.
- 우리가 알고 있는 인터넷은 수많은 소규모 네트워크들이 서로 연동되는 반복적인 과정을 거쳐서 형성된 거대 연합체의 네트워크를 의미합니다.

## 2️⃣ 인터페이스(Interface)
- 시스템과 시스템을 연결하기 위한 표준화된 접촉 지점을 의미하며, 하드웨어적인 관점과 소프트웨어적인 관점이 모두 존재합니다.
    - 하드웨어적인 예로서, 컴퓨터 본체와 키보드를 연결하여 제대로 동작하게 하려면 키보드의 잭을 본체의 정해진 위치에 꽂아야 합니다.
        - 이렇게 하려면 상호 간의 데이터 교환을 위한 RS-232C, USB 등과 같은 논리적인 규격뿐만 아니라, 잭의 크기와 모양 같은 물리적인 규격도 표준화되어야 합니다.
    - 소프트웨어적인 예로서, 프로그래밍 언어에서 함수 설계자는 함수 이름과 매개변수를 표준화하여 정의해야 하고, 함수 사용자는 이 정의에 맞게 함수 이름과 인수를 지정하여 사용할 수 있습니다.
- 인터페이스를 논리적인 상하 구조의 개념으로 이해할 필요는 없지만, 양방향으로 데이터를 주고 받는 경우와 한쪽에서 다른 쪽의 단방향으로 데이터를 보내는 경우로 나눌 수 있습니다.

## 3️⃣ 전송 매체(Transmission Media)
- 시스템끼리 정해진 인터페이스를 연동해 데이터를 전달하려면 물리적인 전송 수단인 전송 매체(Transmission Media)가 반드시 있어야 합니다.
- 전송 매체는 사람의 눈으로 볼 수 있는 동축 케이블을 포함하여 소리를 전파하는 공기, 무선 신호 등 다양하게 존재합니다.
- 인터페이스는 시스템 간의 물리적인 연동을 위한 논리적인 규격이고 인터페이스로 정해진 규격은 전송 매체를 통해 물리적으로 구현되며, 시스템끼리 데이터 전송을 가능하게 합니다.

## 4️⃣ 프로토콜(Protocol)
- 논리적으로 상호 연동되는 시스템이 전송 매체를 통해 데이터를 교환할 때는 표준화된 대화 규칙을 따르는데, 이 규칙을 프로토콜(Protocol)이라 합니다.
- 일반적으로 프로토콜은 상하 관계가 아닌 동등한 위치에 있는 시스템 사이의 규칙이라는 측면이 강조되어 인터페이스와 구분이 됩니다.
- 인터페이스는 위 그림과 같이 두 시스템이 연동하기 위한 특정한 접촉 지점(Access Point)을 의미하는 경우가 많지만, 프로토콜과 비교하여 인용될 때는 상하 개념이 적용됩니다.
    - 즉, 네트워크의 계층 모델 구조에서 인터페이스는 상하 계층 사이의 관계를 다루고, 프로토콜은 동등 계층 사이의 관계를 다룹니다.
        - 일반적으로 프로토콜은 주고받는 데이터의 형식과 그 과정에서 발생하는 일련의 절차적 순서에 무게를 둡니다.

## 5️⃣ 네트워크(Network)
- 통신용 전송 매체로 연결된 여러 시스템이 프로토콜을 사용하여 데이터를 주고받을 때, 이들을 하나의 단위로 통칭하여 네트워크(Network)라 부릅니다.
- 일반적인 컴퓨터 네트워크에서는 물리적인 전송 매체로 연결된 컴퓨터들이 동일한 프로토콜을 이용해 서로 데이터를 주고 받습니다.
- 소규모 네트워크가 모여 더 큰 네트워크를 구성할 수 있는데, 네트워크끼리는 라우터(Router)라는 중개 장비를 사용해서 연결합니다.

## 6️⃣ 인터넷(Internet)
- 전 세계의 모든 네트워크가 유기적으로 연결되어 동작하는 통합 네트워크입니다.
- 인터넷에서 사용되는 시스템, 인터페이스, 전송 매체, 프로토콜들은 그 종류가 매우 복잡하고 다양하지만, 데이터 전달 기능에 한해서는 공통으로 IP(Internet Protocol) 프로토콜을 사용합니다.
    - 즉, ISO의 OSI 7계층 모델에서 계층 3인 네트워크 계층의 기능을 IP 프로토콜이 수행하며 인터넷이라는 용어의 IP의 첫 단어인 Internet에서 유래했습니다.

## 7️⃣ 표준화(Standardization)
- 서로 다른 시스템이 상호 연동해 동작하려면 표준화(Standardization)라는 연동 형식의 통일이 필요합니다.
    - 예를 들어, 프린트 용지를 생각해봅시다.
        - 일반적으로 프린터와 프린트 용지를 만드는 회사는 다릅니다.
        - 하지만 사전에 A4 규격이라는 통일된 틀을 만들어두었기 때문에 서로 다른 회사에서 생산한 프린터와 프린트 용지를 자유롭게 사용할 수 있습니다.
<img src = "https://github.com/devKobe24/images2/blob/main/network/network-2.png?raw=true">
- 현대 산업사회가 눈부시게 성장한 배경에는 증기기관의 개발에 따른 에너지 동력원의 발전이 있었습니다.
- 지금은 인간의 노동력이라는 한계를 넘어 인공지능으로 대표되는 새로운 차원의 사회 발전 단계인 4차 산업혁명이 진행되고 있습니다.
    - 그러나 이와 다른 관점에서 더 근원적인 발전 배경을 살펴보면, 표준화 원리를 바탕으로 한 레고의 조합 개념이 산업 전반에 존재해왔기 때문임을 알 수 있습니다.
