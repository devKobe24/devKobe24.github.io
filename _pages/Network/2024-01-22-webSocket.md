---
title: 🌐[Network] 웹소켓(WebSocket)
date: 2024-01-22 15:18:00
modified: 2024-01-22 15:18:00
tags: [Network, Server, Back-end]
description: 웹소켓(WebSocket)에 대하여.
---

# 🌐[Network] 웹소켓(WebSocket)?

<strong>웹소켓(WebSocket)</strong>은 웹 브라우저와 서버 간에 양방향 통신을 가능하게 하는 고급 기술입니다.<br>
이는 HTTP 프로토콜에 비해 지속적이고 실시간의 통신을 가능하게 합니다.<br>

## 🌐 웹소켓의 주요 특징.

<strong>1. 양방향 통신 :</strong> <strong>웹소켓</strong>은 서버와 클라이언트 간의 양방향 통신 채널을 제공합니다.<br>
이는 클라이언트와 서버가 서로에게 데이터를 동시에 보낼 수 있음을 의미합니다.<br>
<br>

<strong>2. 실시간 통신 :</strong> <strong>웹소켓</strong>은 실시간 통신을 위해 설계되었습니다.<br>
이는 온라인 게임, 채팅 애플리케이션, 실시간 데이터 스트리밍 등에 매우 유용합니다.<br>
<br>

<strong>3. 지속적인 연결 :</strong> <strong>웹소켓</strong> 연결은 한 번 수립되면 지속적으로 유지됩니다.<br>
이는 브라우저가 서버에 연결을 맺은 후 해당 연결을 지속적으로 유지하며 데이터를 교환한다는 것을 의미합니다.<br>
<br>

<strong>4. 효율적인 데이터 전송 :</strong> <strong>웹소켓</strong>은 HTTP의 오버헤드를 줄여 효율적인 데이터 전송을 가능하게 합니다.<br>
데이터 패킷이 작고, 연결 수립 후에는 추가적인 헤더 없이 데이터를 전송할 수 있습니다.<br>
<br>

## 🌐 작동 원리.

<strong>1. 핸드셰이크 :</strong> <strong>웹소켓</strong> 연결은 HTTP 요청을 통해 시작됩니다.<br>
이 요청은 서버에 웹소켓 연결을 요청하는 <strong>"업그레이드 요청(Upgrade Request)"</strong>을 포함합니다.<br>
서버가 이를 수락하면, HTTP 연결은 <strong>웹소켓</strong> 연결로 <strong>"업그레이드"</strong>됩니다.<br>
<br>

<strong>2. 연결 수립 :</strong> 핸드셰이크 과정이 성공적으로 완료되면, <strong>웹소켓</strong> 연결이 수립됩니다.<br>
이 시점부터 클라이언트와 서버 간에 양방향 데이터 전송이 가능해집니다.<br>
<br>

<strong>3. 데이터 프레임 교환 :</strong> <strong>데이터는 프레임 단위로 교환</strong>됩니다.<br>
각 <strong>프레임</strong>은 텍스트 또는 바이너리 데이터를 포함할 수 있으며, <strong>웹소켓</strong> 프로토콜은 이를 신속하게 전송합니다.<br>
<br>

<strong>4. 연결 종료 :</strong> 클라이언트 또는 서버 어느 한 쪽이 <strong>연결을 종료하기</strong>를 요청할 수 있으며, <strong>연결이 종료되면</strong> 더 이상 데이터를 교환할 수 없습니다.<br>
<br>

## 🌐 사용 사례.

<strong>* 채팅 애플리케이션 :</strong> 실시간으로 메시지를 주고받는데 사용됩니다.<br>
<br>

<strong>* 온라인 게임 :</strong> 실시간 상호작용과 게임 상태의 빠른 업데이트를 위해 사용됩니다.<br>
<br>

<strong>* 금융 애플리케이션 :</strong> 주식 시작의 실시간 가격 업데이트 등에 사용됩니다.<br>
<br>

<strong>* 라이브 스트리밍 :</strong> 실시간 비디오 또는 오디오 스트리밍에 사용될 수 있습니다.<br>
<br>

웹소켓은 웹의 실시간, 대화형 애플리케이션을 위한 강력한 도구로, 기존의 HTTP 기반 통신보다 더 다이나믹하고 효율적인 방식을 제공합니다.
