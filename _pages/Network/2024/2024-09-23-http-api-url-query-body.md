---
title: "🌐[Network] HTTP, API, URL, 쿼리, 바디"
tags:
    - Network
date: "2024-09-23"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] HTTP, API, URL, 쿼리, 바디

## 1️⃣ HTTP란 무엇인가요?
- **HTTP(Hypertext Transfer Protocol)** 는 WWW(World Wide Web)에서 정보를 주고받기 위한 기본적인 프로토콜입니다.
- 웹 브라우저와 웹 서버 간의 통신을 가능하게 하는 규약으로, 클라이언트(Client, 사용자의 웹 브라우저)가 서버(Server)에 요청(Request)을 보내고, 서버가 이에 대한 응답(Response)을 보내는 방식으로 동작합니다.

### 1. HTTP의 주요 특징.
- **1. 텍스트 기반 프로토콜.**
    - HTTP는 사람이 읽을 수 있는 텍스트 형식으로 요청과 응답을 주고받습니다.
    - 요청 메시지에는 클라이언트가 서버에 요구하는 작업이, 응답 메시지에는 서버가 요청에 대해 처리한 결과가 포함됩니다.
- **2. 비연결성.**
    - HTTP는 비연결성 프로토콜입니다.
    - 즉, 클라이언트와 서버 간에 요청을 처리할 때만 연결을 맺고, 응답이 완료되면 연결을 끊습니다.
    - 이후 요청이 있을 때마다 새로운 연결을 맺습니다.
    - 이를 통해 자원 사용을 줄일 수 있지만, 지속적인 연결이 필요한 애플리케이션에서는 한계가 있습니다.
- **3. 무상태성.**
    - HTTP는 상태를 유지하지 않는 프로토콜입니다.
    - 즉, 서버는 클라이언트의 요청을 각각 독립적으로 처리하며, 이전 요청의 상태나 정보를 기억하지 않습니다.
    - 이를 해결하기 위해 쿠키(cookie), 세션(session), JWT(Json Web Token)등을 사용하여 상태 정보를 유지합니다.
- **4. 포트 번호.**
    - HTTP는 기본적으로 **80번 포트** 를 사용하며, 보안이 강화된 **HTTPS는 443번 포트** 를 사용합니다.

### 2. HTTP의 동작 방식.
- HTTP는 클라이언트-서버 구조로 동작하며, 주로 웹 브라우저가 클라이언트 역할을 하고, 웹 서버가 서버 역할을 합니다.
- 기본적인 요청과 응답 과정은 다음과 같습니다.
    - **1. 클라이언트 요청.**
        - 사용자가 웹 브라우저에 `http://example.com`과 같은 URL을 입력하면, 클라이언트(웹 브라우저)가 서버에 HTTP 요청을 보냅니다.
        - 이 요청은 웹 페이지나 이미지 파일과 같은 자원을 요구하는 내용입니다.
    - **2. 서버 응답.**
        - 서버는 클라이언트의 요청을 처리한 후, 요청한 자원(예: HTML 문서)을 HTTP 응답으로 돌려줍니다.
    - **3. 웹 페이지 표시.**
        - 클라이언트는 받은 응답을 해석하여 웹 페이지를 렌더링하거나 자원을 표시합니다.

### 3. HTTP 요청 메시지 구조.
- HTTP 요청 메시지는 다음과 같은 구조를 가집니다.
    - **1. 요청 라인(Request Line)**
        - **메서드(Method) :** 클라이언트가 서버에 요청하는 작업을 나타냅니다.
            - **`GET` :** 서버로부터 데이터를 요청.
            - **`POST` :** 서버에 데이터를 전송.
            - **`PUT` :** 서버에 데이터를 업데이트.
            - **`DELETE` :** 서버에서 데이터를 삭제.
        - **URL :** 요청하려는 자원의 위치를 나타냅니다.
        - **HTTP 버전 :** 사용 중인 HTTP 프로토콜의 버전 정보입니다.(예: HTTP/1.1)
            - 예: `GET /index.html HTTP/1.1`
    - **2. 헤더(Header)**
        - 요청에 대한 추가 정보를 제공합니다.
        - 예를 들어, `User-Agent` 헤더는 요청을 보내는 클라이언트 정보를 담고 있으며, `Accept` 헤더는 클라이언트가 수락할 수 있는 데이터 형식을 지정합니다.
    - **3. 본문(Body)**
        - `POST` 나 `PUT`과 같은 메서드를 사용할 때 클라이언트가 서버로 전송할 데이터를 포함합니다.
        - `GET` 요청에서는 본문이 없는 경우가 대부분입니다.

### 4. HTTP 응답 메시지 구조.
- **1. 상태 라인(Status Line)**
    - **HTTP 버전**
        - 서버가 사용하는 HTTP 버전입니다.
    - **상태 코드(Status Code)**
        - 요청 처리 결과를 나타내는 숫자 코드입니다.
    - **상태 설명(Status Text)**
        - 상태 코드에 대한 설명입니다.
    - 예: `HTTP/1.1 200 OK`
- **2. 헤더(Header)**
    - 응답에 대한 부가 정보를 제공합니다.
        - 예를 들어, `Content-Type` 헤더는 응답 본문의 데이터 형식을 지정하며, `Content-Length`는 응답 데이터의 길이를 나타냅니다.
- **3. 본문(Body)**
    - 서버가 클라이언트에게 보내는 실제 데이터가 포함됩니다.
    - 예를 들어, HTML 문서, 이미지, JSON 데이터 등이 본문에 담길 수 있습니다.

### 5. HTTP 상태 코드.
- HTTP 응답에서 상태 코드는 클라이언트가 보낸 요청의 처리 결과를 나타냅니다. 주요 상태 코드는 다음과 같습니다.
    - **1xx (정보) :** 요청이 수신되어 처리 중입을 나타냅니다.
    - **2xx (성공) :** 요청이 성공적으로 처리되었음을 나타냅니다.
        - **200 OK :** 요청이 성공적으로 처리되었음.
    - **3xx (리다이렉션) :** 요청한 자원이 영구적으로 다른 위치로 이동했음을 나타냅니다.
        - **301 Moved Permanently :** 요청한 자원이 영구적으로 다른 위치로 이동함.
        - **302 Found :** 요청한 자원이 일시적으로 다른 위치로 이동함.
    - **4xx(클라이언트 오류) :** 클라이언트의 요청에 오류가 있음을 나타냅니다.
        - **400 Bad Request :** 잘못된 요청.
        - **404 Not Found :** 요청한 자원이 존재하지 않음.
    - **5xx(서버 오류) :** 서버에서 요청을 처리하는 중에 오류가 발생했음을 나타냅니다.
        - **500 Internal Server Error :** 서버에서 발생한 일반적인 오류.
        - **503 Service Unavailable :** 서버가 현재 요청을 처리할 수 없음.

### 6. HTTP의 한계와 HTTPS
- HTTP는 기본적으로 **암호화되지 않은 프로토콜** 입니다.
- 따라서, 데이터가 중간에 가로채기(스니핑) 당할 수 있으며, 민감한 정보가 보호되지 않습니다.
- 이를 해결하기 위해 **HTTPS(Hypertext Transfer Protocol Secure)** 가 도입되었습니다.
- HTTPS는 HTTP에 **SSL/TLS 암호화 계층** 을 추가하여 데이터를 안전하게 주고받을 수 있도록 합니다.
- 이를 통해 웹 사이트와 사용자 간의 통신이 보호됩니다.

### 7. 요약.
- HTTP는 웹에서 클라이언트(주로 웹 브라우저)와 서버 간에 데이터를 주고받기 위한 프로토콜로, 요청(Request)-응답(Response) 구조로 동작하며 텍스트 기반의 메시지를 주고받습니다.
- 비연결성과 무상태성을 가지며, 상태 코드와 헤더를 통해 요청 및 응답에 대한 정보를 제공하여 웹 상에서의 통신을 지원합니다.
- HTTPS는 HTTP의 보안 강화를 위해 도입된 버전입니다.

## 2️⃣ API(Application Programming Interface)
- API(Application Programming Interface)는 응용 프로그램 간에 상호작용할 수 있도록 정의된 규칙과 도구의 모음입니다.
- 즉, API는 소프트웨어 애플리케이션이 서로 통신하고 데이터를 주고받기 위한 인터페이스를 제공합니다.
- API를 사용하면 한 프로그램이 다른 프로그램의 기능을 활용하거나 데이터를 가져올 수 있게 됩니다.

### 1. API의 주요 개념.
- **1. 인터페이스.**
    - API는 두 소프트웨어 간에 중개 역할을 하여, 서로 다른 시스템이나 애플리케이션이 **일정한 규칙** 에 따라 데이터를 교환할 수 있게 해줍니다.
    - 이를 통해 각 시스템이 상호작용하면서도 독립성을 유지할 수 있습니다.
- **2. 표준화된 규칙.**
    - API는 개발자들이 특정 기능을 쉽게 사용할 수 있도록 **표준화된 방식** 으로 제공됩니다.
    - 예를 들어, 웹 API는 HTTP 요청을 통해 데이터를 전송하거나 받는 규칙을 따릅니다.
- **3. 추상화.**
    - API는 내부 구현 방식을 숨기고, 외부에서는 단순한 인터페이스만 제공하여 복잡한 작업을 쉽게 수행할 수 있도록 합니다.
    - 예를 들어, Google Maps API를 사용하면 복잡한 지도 데이터 처리 과정을 몰라도 간단한 명령을 통해 지도 정보를 활용할 수 있습니다.

### 2. API의 유형.
- **1. 웹 API**
    - **REST API(Representational State Transfer API)** 
        - 주로 HTTP 프로토콜을 통해 데이터를 교환하는 웹 API입니다. RESTful API는 URL을 통해 자원(Resource)을 식별하고, HTTP 메서드(GET, POST, PUT, DELETE 등)를 사용하여 데이터를 처리합니다.
    - **SOAP API(Simple Object Access Protocol)** 
        - XML 기반의 프로토콜을 사용하여 데이터 전송을 처리하는 웹 API입니다.
        - REST API보다 복잡하지만, 더 엄격한 표준을 따릅니다.
- **2. 라이브러리 API**
    - 개발자가 특정 라이브러리를 사용할 때 제공되는 함수와 명령어를 의미합니다.
    - 예를 들어, Python의 `math` 라이브러리는 수학 관련 기능을 제공하는 API입니다.
- **3. 운영 체제 API**
    - 운영 체제가 제공하는 기능을 애플리케이션이 사용할 수 있도록 인터페이스를 제공합니다.
    - 예를 들어, Windows API는 파일 시스템 접근, 그래픽 처리, 네트워크 기능 등을 제공합니다.
- **4. 하드웨어 API**
    - 하드웨어 장치(예: 프린터, 그래픽 카드)와 소프트웨어 간의 상호작용을 위한 API입니다.
    - 이를 통해 소프트웨어는 하드웨어의 기능을 직접 조작할 수 있습니다.

### 3. API의 구성 요소.
- **1. 요청(Request)**
    - 클라이언트가 서버에 데이터를 요청하는 과정입니다.
    - 일반적으로 웹 API에서는 HTTP 요청을 사용하며, 요청에는 다음이 포함됩니다.
        - **URL :** 자원의 위치를 지정합니다.
        - **HTTP 메서드 :** 클라이언트가 서버에 수행하고자 하는 작업을 나타냅니다.
            - **`GET` :** 데이터를 가져옵니다.
            - **`POST` :** 새로운 데이터를 생성합니다.
            - **`PUT` :** 기존 데이터를 수정합니다.
            - **`DELETE` :** 데이터를 삭제합니다.
        - **헤더(Header) :** 요청에 대한 추가 정보를 담고 있으며, 인증 정보나 데이터 형식을 정의합니다.
        - **본문(Body) :** 주로 **`POST`** 나 **`PUT`** 요청에서 서버로 전송할 데이터를 포함합니다.
- **2. 응답(Response)**
    - 서버가 클라이언트의 요청에 대해 반환하는 정보입니다.
    - 응답에는 다음이 포함됩니다.
        - **상태 코드 :** 요청이 성공했는지, 오류가 발생했는지를 나타내는 코드입니다.
            - **`200 OK` :** 요청 성공.
            - **`404 Not Found` :** 요청한 자원을 찾을 수 없음.
            - **`500 Internal Server Error` :** 서버 내부 오류.
- **3. 본문(Body)**
    - 서버가 반환하는 실제 데이터로, 보통 JSON, XML, HTML 형식으로 제공됩니다.

### 4. API의 예시.
- **1. 웹 API 예시.**
    - **Google Maps API**
        - 개발자는 Google Maps API를 사용하여 웹 사이트나 앱에 지도를 삽입하고, 위치 기반 데이터를 검색할 수 있습니다.
        - 사용자는 지도 데이터를 직접 관리하지 않아도 Google의 서버에서 제공하는 서비스를 활용할 수 있습니다.
    - **Twitter API**
        - Twitter API는 트위터 데이터(트윗, 사용자 정보 등)에 접근할 수 있는 기능을 제공합니다.
        - 이를 통해 개발자는 트위터와 관련된 애플리케이션이나 분석 도구를 만들 수 있습니다.
    - **라이브러리 API 예시**
        - Java Standard Library
            - Java 프로그래밍 언어에서 기본적으로 제공하는 함수 및 메서드 모음입니다.
            - 예를 들어, `java.util.List` API를 통해 리스트 관련 기능을 쉽게 사용할 수 있습니다.

### 5. API의 장점.
- **1. 재사용성.**
    - API를 사용하면 기존 시스템의 기능을 재사용할 수 있어, 동일한 기능을 처음부터 개발하지 않아도 됩니다.
    - 이를 통해 개발 효율성이 높아집니다.
- **2. 확장성.**
    - API는 서로 다른 애플리케이션이 상호작용할 수 있도록 하여, 확장성과 유연성을 제공합니다.
    - 예를 들어, 다양한 앱이 Facebook API를 사용해 소셜 기능을 통합할 수 있습니다.
- **3. 유지보수.**
    - API는 인터페이스를 제공하는 측(서버, 서비스 제공자)과 이를 사용하는 측(클라이언트)이 독립적으로 유지보수할 수 있게 해줍니다.
    - 즉, API의 구현 세부 사항이 변경되더라도 클라이언트는 동일한 방식으로 API를 호출할 수 있습니다.
- **4. 보안.**
    - API는 데이터나 서비스에 대한 접근을 제한할 수 있는 보안 메커니즘을 제공합니다.
    - API 키, OAuth 등의 인증 방식이 이를 지원합니다.

### 6. API의 보안.
- API는 보안 문제를 해결하기 위해 인증(Authentication) 및 권한 부여(Authorization)를 필요로 합니다.
- 주요 보안 방법은 다음과 같습니다.
    - **API 키**
        - 클라이언트가 서버에 요청할 때 사용하는 고유 키로, API 요청이 올바른 애플리케이션에서 왔는지 확인합니다.
    - **OAuth**
        - 사용자 인증을 위한 프로토콜로, 사용자가 애플리케이션이 자신의 자원에 접근할 수 있도록 승인하는 시스템입니다.
    - **HTTPS**
        - API 통신 시 암호화를 적용하여 데이터가 안전하게 전송되도록 보장합니다.

### 7. 요약.
- API는 소프트웨어 간의 상호작용을 위한 인터페이스로, 애플리케이션이 서로 데이터를 주고받고 기능을 사용할 수 있게 합니다.
- 웹, 라이브러리, 운영 체제 등 다양한 API가 있으며, 개발자가 복잡한 기능을 단순화하고 시스템 간의 통합을 쉽게 구현할 수 있도록 돕습니다.
- API는 재사용성, 확장성, 보안성 측면에서 중요한 역할을 합니다.

## 3️⃣ URL(Uniform Resource Locator)
- **URL(Uniform Resource Locator)** 은 인터넷 상에서 특정 자원의 위치를 나타내는 주소입니다.
- 웹 브라우저는 URL을 사용하여 사용자가 요청한 웹 페이지나 리소스에 접근합니다.
- 쉽게 말해, URL은 인터넷 상에서 자원이 어디에 위치해 있는지를 나타내는 "웹 주소" 입니다.

### 1. URL의 구성 요소.
- URL은 여러 구성 요소로 이루어져 있으며, 각각의 부분은 자원에 접근하기 위한 중요한 정보를 제공합니다.
- 일반적인 URL 형식은 다음과 같습니다.

```
프로토콜://사용자 정보@호스트:포트/경로?쿼리#프래그먼트
```

- **1. 프로토콜(Scheme)**
    - 클라이언트가 자원에 접근할 때 사용할 통신 방식을 나타냅니다.
    - 예: `http`, `https`, `ftp` 등
    - 예: `https://www.example.com`에서 `https`는 HTTPS 프로토콜을 사용해 데이터를 주고받는다는 것을 의미합니다.
- **2. 호스트(Host)**
    - 자원이 위치한 서버의 도메인 이름 또는 IP 주소입니다.
    - 예: `www.example.com`, `198.168.1.1`
    - 예: `https://www.example.com`에서 `www.example.com`은 서버의 도메인 이름입니다.
- **3. 포트 번호(Port) (선택 사항)**
    - 서버에서 사용할 특정 네트워크 포트를 지정합니다.
    - 기본적으로 HTTP는 포트 80, HTTPS는 포트 443을 사용합니다.
    - 포트를 명시하지 않으면 기본 포트가 사용됩니다.
    - 예: `https://www.example.com:8080`에서 `8080`은 포트 번호입니다.
- **4. 경로(Path)**
    - 서버 내에서 특정 자원의 위치를 나타냅니다.
    - 파일 시스템 경로와 유사하게 자원(웹 페이지, 이미지, 파일 등)의 경로를 지정합니다.
    - 예: `https://www.example.com/about` 에서 `/about`은 `example.com` 서버 내의 `about` 페이지를 나타냅니다.
- **5. 쿼리 문자열(Query String)(선택 사항)**
    - 추가적인 매개변수(parameters)를 서버에 전달하기 위해 사용됩니다.
    - 주로 URL 끝에 `?` 뒤에 위치하며, 여러 개의 매개변수는 `&`로 구분됩니다.
    - 예: `https://www.example.com/search?q=example&lang=en`에서 `q=example`과 `lang=en`은 검색어와 언어를 나타내는 쿼리 매개변수입니다.
- **6. 프래그먼트(Fragment)(선택 사항)**
    - 페이지 내 특정 위치를 나타냅니다.
    - `#` 기호 뒤에 위치하며, 브라우저가 해당 위치로 스크롤하도록 지시합니다.
    - 예: `https://www.example.com/docs#section2` 에서 `#section2`는 페이지 내 특정 섹션으로 이동하기 위한 프래그먼트입니다.

### 2. URL 예시.
- 다음은 URL의 실제 예시입니다.

```bash
https://www.example.com:8080/docs/tutorial.html?name=test#section1
```
- 이 URL을 분석하면 다음과 같습니다.
    - **프로토콜 :** `https`(보안된 HTTP 연결)
    - **호스트 :** `www.example.com` (서버의 도메인 이름)
    - **포트 :** `8080` (서버가 사용하는 포트 번호)
    - **경로 :** `/docs/tutorial.html` (서버 내 자원의 경로)
    - **쿼리 문자열 :** `?name=test` (추가 매개변수 `name`이 `test`라는 값을 가짐)
    - **프래그먼트 :** `#section1` (문서 내 특정 색션으로 이동)

### 3. URL의 역할.
- **1. 웹 자원 식별**
    - URL은 전 세계적으로 고유한 자원을 식별하여, 사용자가 원하는 웹 페이지나 리소스에 접근할 수 있게 해줍니다.
    - 이는 텍스트 문서, 이미지, 동영상, API 엔드포인트 등 다양한 형태의 자원일 수 있습니다.
- **2. 데이터 전송**
    - URL은 데이터를 서버로 전송하는 데에도 사용됩니다.
    - 특히 쿼리 문자열을 통해 웹 애플리케이션이 사용자 입력 데이터를 서버로 전송할 수 있습니다.
    - 예를 들어, 검색 엔진에서 검색어를 입력하면 URL에 쿼리 문자열로 서버에 전송됩니다.
- **3. 네트워크 리소스 연결**
    - URL은 단순한 웹 페이지뿐만 아니라, FTP 서버, 메일 서버, 데이터베이스 등의 네트워크 자원과도 연결될 수 있습니다.

### 4. URL과 URI의 차이.
- **URI(Uniform Resource Identifier)** 는 URL을 포함하는 더 넓은 개념으로, 자원을 식별할 수 있는 문자열을 말합니다.
- URI는 두 가지로 구분됩니다.
    - **URL :** 자원의 위치를 나타내는 식별자.
    - **URN(Uniform Resource Name) :** 자원의 이름만을 나타내는 식별자.
        - 예: ISBN 번호(`urn:isbn:041450523`)
- URL은 자원의 위치(주소)를 나타내는 URI의 한 형태입니다.

### 5. URL의 중요성.
- **1. 웹 탐색.**
    - 웹 브라우저에서 URL을 입력함으로써 원하는 웹 페이지에 직접 접근할 수 있습니다.
    - 또한 하이퍼링크로 URL을 연결해 사용자는 쉽게 웹 페이지 간을 이동할 수 있습니다.
- **2. API 호출.**
    - URL은 웹 API 호출 시 중요한 역할을 합니다.
    - API는 URL을 통해 특정 자원에 접근하고, 데이터를 주고받는 방식으로 동작합니다.
    - RESTful API에서는 URL이 자원을 식별하고 작업을 수행하는 중요한 요소입니다.
- **3. 검색 엔진 최적화(SEO).**
    - URL 구조는 검색 엔진에서 웹 페이지를 크롤링하고 인덱싱하는 데 중요한 요소입니다.
    - 잘 구성된 URL은 검색 엔진이 페이지 내용을 이해하는 데 도움을 줍니다.

### 6. 요약.
- URL은 인터넷에서 자원의 위치를 나타내는 주소로, 프로토콜, 도메인 이름, 경로, 쿼리 문자열, 프래그먼트 등 여러 요소로 구성됩니다.
- URL을 통해 사용자는 웹 자원에 접근하고 데이터를 주고받을 수 있으며, 이는 웹 브라우징, API 호출, 데이터 전송 등에 핵심적인 역할을 합니다.

## 4️⃣ 쿼리(Query)
- **쿼리(Query)** 는 클라이언트가 서버에 데이터를 전달할 떄 **URL의 일부** 로 포함되어 서버로 전송되는 데이터를 의미합니다.
- 주로 **GET** 요청에 사용되며, URL 끝에 `?` 기호를 사용하여 쿼리 문자열을 시작하고, 하나 이상의 매개변수(파라미터)를 `&` 기호로 구분하여 서버에 전달합니다.

### 1. 쿼리의 주요 개념.
- **1. GET 요청에서의 쿼리 사용.**
    - GET 요청은 서버에 데이터를 전달할 때 URL에 쿼리 문자열을 포함하여 전송합니다.
    - 쿼리는 URL의 경로 뒤에 붙으며, `?` 로 시작한 후 매개변수(key-value)를 `=`로 연결합니다.
    - 여러 매개변수가 있을 경우 `&`로 구분합니다.
    - 쿼리는 주로 **검색 조건, 필터링 정보, 페이지 번호, 정렬 순서 등** 을 서버에 전달할 때 사용됩니다.

```bash
https://www.example.com/search?q=shoes&color=red&size=10
```

- 예를 들어, 위와 같은 URL을 살펴봅시다.
    - `q=shoes` : `q`라는 매개변수에 `shoes`라는 검색어를 전달합니다.
    - `color=red` : `color` 라는 매개변수에 `red` 라는 값을 전달합니다.
    - `size=10` : `size` 라는 매개변수에 `10` 이라는 값을 전달합니다.

- **2. 쿼리의 구조.**
    - `?`로 시작하여 쿼리 문자열이 시작됩니다.
    - `key=value` 형태로 매개변수와 그 값을 표현합니다.
    - 여러 개의 매개변수를 사용할 경우 `&`로 구분합니다.
    - 기본적인 쿼리 형식은 아래와 같습니다.

```bash
https://www.example.com/resource?key1=value1&key2=value2&key3=value3
```

- **3. 서버에서의 처리.**
    - 서버는 클라이언트가 전달한 쿼리 문자열을 파싱하여 각각의 매개변수 값을 추출합니다.
    - 추출된 값들은 서버에서 요청을 처리하는 데 사용되며, 예를 들어 검색어, 필터링 옵션, 페이지 번호 등을 기반으로 클라이언트가 원하는 결과를 반환합니다.

- **4. 데이터의 제한.**
    - GET 요청은 데이터의 크기 제한이 있을 수 있습니다.
    - 브라우저나 서버는 보통 약 2048자 정도의 URL 길이를 허용하므로, 쿼리로 전송할 수 있는 데이터는 상대적으로 적습니다.
    - 민감한 데이터(비밀번호, 개인정보 등)는 쿼리 문자열로 전송하지 않는 것이 좋습니다.
    - 쿼리는 URL에 노출되며, 브라우저 기록이나 서버 로그에 저장될 수 있기 때문입니다.
    - 이러한 데이터를 전송하려면 POST 요청을 사용하는 것이 일반적입니다.

### 2. 쿼리의 사용 예시.
- **예시 1: 검색.**

```bash
https://www.example.com/search?query=laptop&category=electronics&sort=price
```
- 이 쿼리는 "laptop"이라는 검색어로 전자 제품 카테고리 내에서 가격순으로 정렬된 결과를 요청하는 예입니다.
    - **query :** `laptop`(검색 키워드)
    - **category :** `electronics`(카테고리)
    - **sort :** `price`(가격순 정렬)

- **예시 2: 필터링 및 페이징.**

```bash
https://www.example.com/products?category=shoes&brand=nike&size=9&page=2
```

- 여기서는 신발 카테고리에서 `Nike` 브랜드의 사이즈 9인 제품을 두 번째 페이지에서 요청하는 상황입니다.
    - **category :** `shoes`(제품 카테고리)
    - **brand :** `nike` (브랜드 필터)
    - **size :** `9` (사이즈 필터)
    - **page :** `2` (페이지 번호)

### 3. 쿼리의 장점.
- **URL을 통해 간편하게 데이터 전송.**
    - 쿼리 문자열을 통해 클라이언트는 쉽게 서버에 데이터를 전달할 수 있습니다.
- **직접 링크로 공유 가능.**
    - 쿼리 문자열을 포함한 URL은 바로 공유할 수 있으며, 사용자가 동일한 URL을 통해 동일한 요청을 재현할 수 있습니다.
        - 예를 들어, 사용자가 필터링 옵션이 적용된 검색 결과 페이지의 URL을 다른 사용자와 공유할 수 있습니다.
- **검색 엔진 최적화(SEO).**
    - 검색 엔진이 쿼리를 통해 특정한 페이지 내용을 이해하고 색인할 수 있습니다.

### 4. 쿼리와 POST 요청의 차이점.
- **GET 요청** 에서의 쿼리 문자열은 URL에 노출되며, 브라우저 히스토리나 서버 로그에 저장될 수 있습니다.
    - 이 때문에 민감한 정보를 전송할 때는 적합하지 않습니다.
- **POST 요청** 에서는 데이터를 본문(body)에 담아 전송하므로, URL에 노출되지 않습니다.
- 큰 양의 데이터를 전송하거나 보안이 중요한 정보를 전송할 때는 POST 요청을 사용하는 것이 더 안전합니다.

### 5. 요약.
- 쿼리는 클라이언트가 서버에 데이터를 전달할 때 GET 요청의 URL에 매개변수로 포함되어 전송되는 데이터를 의미합니다.
- 쿼리 문자열은 `key=value` 형태로 전달되며, 여러 매개변수는 `&`로 구분됩니다.
- 검색어, 필터 조건, 페이지 번호 등을 서버로 전송할 때 주로 사용되며 URL에 노출되는 만큼 민감한 정보 전송에는 적합하지 않습니다.

## 5️⃣ 바디(Body)
- **바디(Body)** 는 클라이언트가 서버로 **POST, PUT, PATCH** 와 같은 HTTP 요청을 보낼 때, **데이터를 담아 서버로 전송하는 부분** 입니다.
- 특히, 큰 양의 데이터나 보안이 중요한 데이터를 전송할 때 바디를 사용합니다.
- 바디는 URL에 노출되지 않고 요청 메시지의 본문에 포함되므로, 데이터 전송에 있어 중요한 역할을 합니다.

### 1. 바디를 사용하는 주요 HTTP 메서드.
- **1. POST**
    - 서버에 새로운 데이터를 생성하기 위한 요청을 보낼 때 사용합니다.
    - 요청 바디에 전송할 데이터를 포함하여 서버로 보냅니다.
    - 예: 사용자 등록, 파일 업로드 등.
- **2. PUT**
    - 서버에 기존 데이터를 수정할 때 사용합니다.
    - 요청 바디에 수정할 데이터를 포함하여 서버로 보냅니다.
    - 예: 기존 사용자의 정보 업데이트.
- **3. PATCH**
    - 서버의 일부 데이터를 수정할 때 사용됩니다.
    - 전체 데이터가 아닌, 수정할 부분만을 바디에 포함하여 보냅니다.
- **4. DELETE**
    - 일부 경우에는 서버에 삭제할 자원에 대한 추가 정보를 바디에 담아 보낼 수 있습니다.
    - 하지만 대부분의 DELETE 요청은 바디 없이 실행되기도 합니다.

### 2. 바디를 사용하는 이유.
- **1. 대용량 데이터 전송.**
    - GET 요청에서 사용되는 쿼리 문자열은 데이터 크기에 제한이 있습니다.
    - 반면, 바디를 사용하는 POST나 PUT 요청은 큰 데이터를 안전하게 전송할 수 있습니다.
    - 예를 들어, 대용량 파일이나 이미지 등을 전송할 때 바디를 사용합니다.

- **2. 보안.**
    - 바디에 포함된 데이터는 URL에 노출되지 않으며, 브라우저의 주소 창이나 서버 로그에 기록되지 않습니다.
    - 따라서, 비밀 번호, 개인정보, 결제 정보 등 민감한 데이터를 전송할 때 바디를 사용하는 것이 적절합니다.

- **3. 구조화된 데이터 전송.**
    - JSON, XML, HTML과 같은 구조화된 데이터를 쉽게 전송할 수 있습니다.
    - 바디는 복잡한 데이터 구조를 담아서 서버로 전달할 수 있는 유연한 방법을 제공합니다.

### 3. 요청 바디의 형식(Content-Type)
- 클라이언트가 서버로 데이터를 전송할 때, 요청 바디의 데이터 형식은 **Content-Type** 헤더로 지정됩니다.
- 일반적으로 사용되는 데이터 형식은 다음과 같습니다.
    - **1. application/json**
        - JSON(JavaScript Object Notation) 형식으로 데이터를 전송합니다.
        - RESTful API에서 가장 많이 사용되는 형식입니다.

        ```json
        {
          "username": "example_user",
          "password": "example_password"
        }
        ```
        
    - **2. application/x-www-form-urlencoded**
        - HTML 폼에서 전송되는 기본 형식으로, 키-값 쌍으로 URL 인코딩 방식으로 전송합니다.
        - 데이터는 URL 쿼리 문자열과 유사한 형식으로 바디에 전송됩니다.

        ```json
        username=example_user&password=example_password
        ```
    
    - **3. multipart/form-data**
        - 파일 업로드와 같이 다양한 형식의 데이터를 함께 전송할 때 사용됩니다.
        - 각 파일이나 데이터 항목을 별도의 파트로 구분하여 전송할 수 있습니다.
        - 특히 이미지, 문서와 같은 파일을 전송할 때 많이 사용됩니다.
        
        ```json
        Content-Type: multipart/form-data; boundary=------------------------boundary
        --------------------------boundary
        Content-Disposition: form-data; name="username"

        example_user
        --------------------------boundary
        Content-Disposition: form-data; name="file"; filename="example.png"
        Content-Type: image/png

        (파일 데이터)
        --------------------------boundary--
        ```
    
    - **4. text/plain**
        - 단순 텍스트 데이터를 전송할 때 사용됩니다.
        - 예를 들어, 간단한 메시지를 전송하거나 테스트용으로 사용할 수 있습니다.

### 4. 요청 바디 사용 예시.
- **1. POST 요청 예시 (JSON 데이터 전송)**
    - 클라이언트가 서버로 사용자 등록 요청을 보낼 때, JSON 형식으로 데이터를 전송할 수 있습니다.

    ```json
    POST /register HTTP/1.1
    Host: www.example.com
    Content-Type: application/json
    
    {
        "username": "new_user",
        "email": "user@example.com",
        "password": "user_password"
    }
    ```
    - **Content-Type**
        - 요청 바디가 JSON 형식임을 서버에 알립니다.
    - **바디**
        - 클라이언트가 전송하려는 데이터가 JSON 형식으로 포함됩니다.

- **2. PUT 요청 예시 (폼 데이터 전송)**
    - 사용자의 정보를 수정할 때, 폼 데이터 형식으로 데이터를 전송할 수 있습니다.

    ```json
    PUT /update-profile HTTP/1.1
    Host: www.example.com
    Content-Type: application/x-www-form-urlencoded
    
    username=updated_user&email=updated@example.com
    ```
    - **Content-Type**
        - `application/x-www-form-urlencoded` 는 키-값 쌍의 폼 데이터를 전송하는 형식입니다.
    - **바디**
        - 수정된 사용자 정보를 키-값 쌍으로 포함하고 있습니다.

### 5. 바디의 보안.
- 바디를 사용하여 전송되는 데이터는 URL에 노출되지 않지만, 여전히 네트워크를 통해 전송되는 동안 보안이 필요합니다.
- 이를 위해 HTTPS를 사용하여 통신을 암호화하는 것이 일반적입니다.
- HTTPS를 사용하면 바디에 포함된 데이터가 중간에서 가로채이거나 변조되지 않도록 보호할 수 있습니다.

### 6. 요약.
- 바디는 클라이언트가 서버로 데이터를 전송할 때 요청 메시지의 본문에 포함되는 부분입니다.
- 주로 POST, PUT, PATCH와 같은 요청에서 사용되며, 대용량 데이터나 민감한 정보를 안전하게 전송하는 데 적합합니다.
- 바디에는 JSON, 폼 데이터, 파일 등 다양한 형식의 데이터를 포함할 수 있으며, URL에 노출되지 않아 보안 측면에서 중요한 역할을 합니다.
