---
title: "🌐[Network] RESTful API란 무엇일까요?"
tags:
    - Network
date: "2024-10-10"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] RESTful API란 무엇일까요?
- **RESTful API는 REST(Representational State Transfer)** 아키텍처 스타일을 기반으로 한 **웹 API**를 의미합니다.
- RESTful API는 클라이언트와 서버 간의 **통신을 효율적이고 일관성 있게 설계하기 위해 HTTP 프로토콜을 활용하여 데이터를 주고 받는 방식**입니다.
- **RESTful**은 특정 기술이나 프로토콜을 지칭하는 것은 아니지만, **REST 아키텍처의 원칙을 준수하는 API를 설명할 때 사용하는 용어입니다.**

## 1️⃣ REST(Representational State Transfer)의 기본 개념.
- REST는 **Roy Fielding**이 2000년 박사 논문에서 소개한 **분산 시스템을 설계하기 위한 아키텍처 스타일**입니다.
    - **RESTful API**는 이 원칙을 따르는 API로, **클라이언트-서버** 구조, **무상태성, 캐싱 가능성, 계층화된 시스템과 같은 REST의 주요 제약을 따릅니다.**

## 2️⃣ REST의 주요 원칙 및 RESTful API의 특징.

### 1️⃣ 자원(Resource) 기반.
- REST에서는 **모든 것을 자원**으로 간주합니다.
    - 예를 들어, 사용자는 하나의 자원, 게시물은 또 다른 하나의 자원으로 취급됩니다.
- 각 자원은 **고유한 URI(Uniform Resource Identifer)를** 통해 식별됩니다.
    - 예를 들어, `https://api.example.com/user/1`는 `id=1`인 사용자 자원을 나타냅니다.

### 👉 URI 설계 예시
- `GET /users` -> 모든 사용자 목록 가져오기
- `GET /users/1` -> 특정 사용자(id=1) 가져오기
- `POST /users/1` -> 새 사용자 생성
- `PUT /users/1` -> 사용자 정보 업데이트
- `DELETE /users/1` -> 사용자 삭제

### 2️⃣ HTTP 메서드 사용.
- RESTful API는 **HTTP 메서드를 자원의 작업과 연관시키기 위해 사용합니다.**
    - 주요 메서드로는 **GET, POST, PUT, DELETE**가 있습니다.
        - **GET :** 자원을 조회할 때 사용합니다.
        - **POST :** 자원을 생성할 때 사용합니다.
        - **PUT :** 자원을 업데이트할 때 사용합니다.
        - **DELETE :** 자원을 삭제할 때 사용합니다.
- 이러한 HTTP 메서드의 사용은 요청의 목적을 명확히 하고, 클라이언트와 서버 간의 상호작용을 일관성 있게 만듭니다.

### 3️⃣ 무상태성(Stateless)
- RESTful API는 **무상태(Stateless)해야 합니다.**
    - **즉, 서버는 클라이언트의 상태를 저장하지 않습니다.**
        - 클라이언트가 보낸 각각의 요청은 **독립적**이어야 하며, 요청에 의존해서는 안 됩니다.
- 클라이언트는 매 요청마다 필요한 모든 정보를 서버에 보내야 하며, 서버는 해당 요청만을 처리하고 응답을 반환합니다.

### 4️⃣ 캐시 가능(Cacheable)
- RESTful API에서 응답은 **캐시 될 수 있어야 합니다.**
    - 서버는 응답에 대한 **캐싱 가능 여부를 클라이언트에게 명확히 전달합니다.**
- 클라이언트가 자원의 불필요한 요청을 반복하지 않도록 하기 위해, 서버는 HTTP 헤더에 `Cache-Control`이나 `Expires`와 같은 캐시 관련 정보를 포함하여 응답을 보낼 수 있습니다.

### 5️⃣ 계층화된 구조.
- RESTful 아키텍처는 **계층화된 시스템을 지원합니다.**
    - 클라이언트는 중간 서버(프록시, 게이트웨이 등)를 통과하더라도, 요청과 응답이 처리되는 방식에 대해 신경 쓸 필요가 없습니다.
    - 서버는 여러 계층을 통해 보안, 로드 밸런싱 등을 처리할 수 있습니다.

### 6️⃣ 일관된 인터페이스.
- RESTful API는 **일관된 인터페이스를 제공해야 합니다.**
    - 이는 자원에 접근하고 조작하는 방식이 명확하고 일관적이어야 하며, 클라이언트가 각 자원에 대해 동일한 방식으로 작업을 수행할 수 있어야 한다는 의미입니다.

### 7️⃣ 표현(Representation) 전송.
- 자원 자체는 서버에 저장되며, 클라이언트는 서버에서 자원의 **표현(Representation)** 을 전송받습니다.
    - 일반적으로 RESTful API는 **JSON**이나 **XML** 형식으로 자원의 상태를 표현하며, 이를 클라이언트에 전달합니다.

#### 👉 예시: 서버가 사용자 자원을 클라이언트에게 JSON 형식으로 응답할 때.
```json
{
    "id": 1,
    "name": "Kobe",
    "email": "kobe@example.com"
}
```

## 3️⃣ RESTful API의 HTTP 메서드와 자원의 관계.

### 1️⃣ GET
- 자원을 조회하는 데 사용됩니다.
- 예: `GET /users/1`은 `id=1`인 사용자를 조회하는 요청입니다.

### 2️⃣ POST
- 새로운 자원을 생성하는 데 사용됩니다.
- 예: `POST /users`는 새로운 사용자를 생성하는 요청입니다.
- 요청 본문에는 생성할 자원의 정보가 포함됩니다.

### 3️⃣ PUT
- 기존 자원을 업데이트하는 데 사용됩니다.
- 예: `PUT /users/1`은 `id=1`인 사용자 정보를 업데이트하는 요청입니다.

### 4️⃣ DELETE
- 자원을 삭제하는 데 사용됩니다.
- 예: `DELETE /users/1`은 `id=1`인 사용자를 삭제하는 요청입니다.

## 4️⃣ RESTful API의 예

### 👉 사용자 자원을 관리하는 RESTful API 예시.

#### 1️⃣ GET 요청 (모든 사용자 조회)
```bash
GET /users
```

#### 응답
```json
[
    {"id": 1, "name": "Kobe", "email": "kobe@example.com"},
    {"id": 2, "name": "Eric", "email": "eric@example.com"}
]
```

#### 2️⃣ POST 요청 (새 사용자 생성)
```bash
POST /users
```

#### 요청 본문.
```json
{
    "name": "Kobe",
    "email": "kobe@example.com"
}
```

#### 응답.
```bash
PUT /users/3
```

#### 3️⃣ PUT 요청 (사용자 정보 업데이트)
```bash
PUT /users/3
```

#### 요청 본문
```json
{
    "name": "Kobe updated",
    "email": "kobe.updated@example.com"
}
```

#### 응답
```json
{
    "id": 3,
    "name": "Kobe updated",
    "email": "kobe.updated@example.com"
}
```

#### 4️⃣ DELETE 요청 (사용자 삭제)
```bash
DELETE /users/3
```

#### 응답.
```json
{
    "message": "User deleted successfully"
}
```

## 5️⃣ RESTful API의 장점.

### 1️⃣ 확장성.
- RESTful API는 서버와 클라이언트 간의 결합도를 낮춰, 확장 가능하고 유연한 시스템을 설계할 수 있습니다.

### 2️⃣ 표준화.
- HTTP 프로토콜의 표준을 기반으로 하여, 다양한 클라이언트와 쉽게 연동이 가능합니다.

### 3️⃣ 경량성.
- JSON 같은 경량 포맷을 사용해 데이터 전송을 최적화할 수 있습니다.

### 4️⃣ 캐싱 가능.
- HTTP 캐싱 메커니즘을 통해 성능을 향상시키고 네트워크 부하를 줄일 수 있습니다.

## 6️⃣ 결론.
- **RESTful API는** 웹 애플리케이션에서 서버와 클라이언트 간의 상호작용을 일관성 있고 효율적으로 처리하기 위한 **API 설계 방식**입니다.
- HTTP 프로토콜을 기반으로 자원을 CRUD 방식으로 관리하며, 무상태성, 자원 기반 접근, 일관된 인터페이스 등의 REST 원칙을 따릅니다.
- RESTful API는 다양한 클라이언트(모바일, 웹 등)와 통신할 수 있는 **확장 가능하고 유연한 시스템을 구축하는 데 매우 유용합니다.**
