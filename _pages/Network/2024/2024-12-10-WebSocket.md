---
title: "🌐[Network] 웹소켓(WebSocket) 프로토콜이란 무엇일까요?"
tags:
    - Network
date: "2024-12-10"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] 웹소켓(WebSocket) 프로토콜이란 무엇일까요?
- **웹소켓(WebSocket)은** 클라이언트(예: 브라우저)와 서버 간의 **양방향(full-duplex)** 통신을 가능하게 하는 프로토콜입니다.
- 웹소켓(WebSocket)은 HTTP(Hypertext Transfer Protocol)와는 다른 프로토콜로, 표준화된 방식으로 실시간 데이터 교환을 지원합니다.
- 웹소켓(WebSocket)은 초기 연결만 HTTP로 시작하고, 이후에는 **하나의 연결을 유지하며 양방향 데이터 전송**을 지원합니다.
    - 이를 통해 웹 애플리케이션에서 **실시간 상호 작용**을 쉽게 구현할 수 있습니다.

## 1️⃣ 웹소켓 프로토콜의 특징.
### 1️⃣ 양방향 통싱(Full-Duplex)
- 클라이언트와 서버가 **동시에 데이터를 주고받을 수 있습니다.**
- HTTP처럼 클라이언트의 요청-응답 구조가 아니라, 연결이 유지되는 동안 **언제든지 데이터 전송이 가능합니다.**

### 2️⃣ 지속적인 연결(Persistent Connection)
- 한번 연결이 설정되면 클라이언트와 서버 간에 **지속적인 연결이 유지됩니다.**
- 재연결의 오버헤드가 없어, 효율적인 데이터 교환이 가능합니다.

### 3️⃣ 낮은 오버헤드
- HTTP보다 훨씬 가벼운 헤더를 사용하므로 **데이터 전송량이 적고, 속도가 빠릅니다.**
    - 특히, 실시간 애플리케이션에서 효율적입니다.

### 4️⃣ 표준 프로토콜
- **RFC 6455**에서 표준으로 정의됩니다.
- 대부분의 최신 브라우저와 서버 프레임워크에서 지원합니다.

## 2️⃣ 웹소켓의 작동 방식.
### 1️⃣ 핸드셰이크(Handshake)
- 클라이언트와 서버 간의 초기 연결은 HTTP 요청을 사용하여 이루어집니다.
- 클라이언트가 서버에 **업그레이드 요청을 보냅니다:**
```http
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
```

- 서버는 이를 승인하여 응답합니다:
```http
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```

### 2️⃣ 연결이 업그레이드됨.
- HTTP 연결이 **WebSocket 연결로 업그레이드되며, 이후에는 지속적인 양방향 통신이 가능합니다.**

### 3️⃣ 데이터 전송.
- 연결이 설정되면, 클라이언트와 서버는 **메시지를 자유롭게 송수신할 수 있습니다.**

### 4️⃣ 연결 종료.
- 필요에 따라 클라이언트나 서버가 연결을 종료(Close)할 수 있습니다.

## 3️⃣ 웹소켓의 장점.
### 1️⃣ 실시간 통신.
- HTTP의 요청-응답 모델과 달리, **실시간 상호작용**을 지원합니다.
- 실시간 채팅, 게임, 주식 가격 업데이트 등에서 효율적입니다.

### 2️⃣ 효율성.
- HTTP에 비해 **오버헤드가 적고, 대역폭 소비가 낮습니다.**
- 지속적인 연결로 인해 다수의 메시지를 빠르게 교환 가능.

### 3️⃣ 확장성.
- 높은 트래픽에서도 **효율적으로 작동**하므로, **대규모 실시간 애플리케이션에 적합**합니다.

## 4️⃣ 웹소켓과 HTTP의 차이.

| 특징 | HTTP | WebSocket |
| -------- | -------- | -------- |
| 통신 방식 | 요청-응답(Request-Response) | 양방향(Full-Duplex) |
| 연결 유지 | 요청마다 새 연결 생성 | 연결 유지(Persistent) |
| 오버헤드 | 비교적 높은 헤더 오버헤드 | 낮은 헤더 오버헤드 |
| 사용 사례 | 일반 웹 페이지, REST API | 실시간 채팅, 게임, 스트리밍 |

## 5️⃣ 웹소켓의 사용 사례.
### 1️⃣ 실시간 채팅 애플리케이션.
- 예: Slack, Discord

### 2️⃣ 라이브 업데이트.
- 주식 사격, 스포츠 경기 점수.

### 3️⃣ 실시간 게임.
- 멀티플레이어 게임에서 빠른 데이터 교환.

### 4️⃣ IoT 애플리케이션.
- 장치 상태 모니터링 및 제어.

### 5️⃣ 협업 도구.
- 문서 편집, 화상 회의 등.

## 6️⃣ 웹소켓 사용 예제.
### 1️⃣ JavaScript 클라이언트 코드(브라우저 클라이언트)
```javascript
const socket = new WebSocket('ws://localhost:8080/ws/chat');

// 연결 열림
socket.onopen = () => {
    console.log('WebSocket 연결 열림');
    socket.send('Hello from client!');
};

// 메시지 수신
socket.onmessage = (event) => {
    console.log('서버로부터 받은 메시지:', event.data);
};

// 연결 닫힘
socket.onclose = () => {
    console.log('WebSocket 연결 닫힘');
};

// 에러 발생
socket.onerror = (error) => {
    console.error('WebSocket 에러:', error);
};
```

### 2️⃣ Spring Boot WebSocket 서버 코드.
#### 1️⃣ 의존성 추가.
- Spring Boot 프로젝트의 build.gradle 파일에 WebSocket 관련 의존성을 추가합니다.

```groovy
implementation 'org.springframework.boot:spring-boot-starter-websocket'
```

#### 2️⃣ WebSocket Config 설정.
- WebSocket 엔드포인트와 핸들러를 설정하는 `@Configuration` 클래스를 작성합니다.

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.socket.config.annotation.EnableWebSocket;
import org.springframework.web.socket.config.annotation.WebSocketConfigurer;
import org.springframework.web.socket.config.annotation.WebSocketHandlerRegistry;

@Configuration
@EnableWebSocket
public class WebSocketConfig implements WebSocketConfigurer {
    
    @Override
    public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
        // WebSocket 핸들러 등록
        registry.addHandler(new MyWebSocketHandler(), "/ws/chat")
                .setAllowedOrigins("*"); // CORS 설정
    }
}
```

#### 3️⃣ WebSocket 핸들러 작성.
- WebSocket 연결을 처리하는 핸들러를 작성합니다.
- 클라이언트에서 받은 메시지를 로그에 출력하고 다시 클라이언트로 전달합니다.
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.socket.TextMessage;
import org.springframework.web.socket.WebSocketHandler;
import org.springframework.web.socket.WebSocketMessage;
import org.springframework.web.socket.WebSocketSession;
import org.springframework.web.socket.handler.TextWebSocketHandler;

public class MyWebSocketHandler extends TextWebSocketHandler {
    
    private static final Logger logger = LoggerFactory.getLogger(MyWebSocketHandler.class);
    
    @Override
    public void afterConnectionEstablished(WebSocketSession session) throws Exception {
        logger.info("클라이언트 연결됨: {}", session.getId());
    }
    
    @Override
    public void handleTextMessage(WebSocketSession session, TextMessage message) throws Exception {
        logger.info("받은 메시지: {}", message.getPayload());
        
        // 클라이언트로 메시지 다시 전송(Echo)
        session.sendMessage(new TextMessage("서버 응답: " + message.getPayload()));
    }
    
    @Override
    public void afterConnectionClosed(WebSocketSession session, org.springframework.web.socket.CloseStatus staus) throws Exception {
        logger.info("클라이언트 연결 종료: {}", session.getId());
    }
}
```

#### 4️⃣ Spring Boot Application 실행.
- Spring Boot 애플리케이션을 실행하면 `/ws/chat` 엔드포인트로 WebSocket 연결을 받을 수 있습니다.

#### 5️⃣ 동작 확인.
- 1. Spring Boot 애플리케이션을 실행합니다.
- 2. 브라우저에서 WebSocket 클라이언트를 실행합니다.
- 3. 서버에서 클라이언트로 메시지를 전송하고, 클라이언트가 받은 메시지를 확인합니다.

#### 6️⃣ 결과.
- 1. 클라이언트 콘솔.
```bash
WebSocket 연결 열림
서버로부터 받은 메시지: 서버 응답: Hello from client!
```

- 2. 서버 로그
```bash
클라이언트 연결됨: f9351a2c-7f2e-4bd7-b930-bddf37e78e0d
받은 메시지: Hello from client!
클라이언트 연결 종료: f9351a2c-7f2e-4bd7-b930-bddf37e78e0d
```
