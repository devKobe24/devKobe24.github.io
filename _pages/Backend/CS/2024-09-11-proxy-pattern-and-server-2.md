---
title: "💾 [CS] 프록시 패턴과 프록시 서버 - 2"
tags:
    - CS
date: "2024-09-11"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 프록시 패턴과 프록시 서버 - 2

## 1️⃣ 프록시 패턴(Proxy Pattern).
- 프록시 패턴(Proxy Pattern)은 객체 지향 프로그래밍에서 사용되는 디자인 패턴 중 하나로, 어떤 객체에 대한 접근을 제어하기 위해 그 객체의 대리인 역할을 하는 별도의 객체(프록시, Proxy)를 제공하는 패턴입니다.
    - 이 패턴을 통해 클라이언트는 원래의 객체 대신 프록시 객체를 통해 간접적으로 원래의 객체에 접근할 수 있습니다.

### 1️⃣ 프록시 패턴의 주요 목적.
- **1. 접근 제어 :** 클라이언트가 실제 객체에 직접 접근하는 것을 제한하거나 제어할 수 있습니다.
- **2. 지연 초기화(Lazy Initialization) :** 실제 객체의 생성과 초기화를 필요할 때까지 미룰 수 있습니다.
    - 예를 들어, 메모리나 리소스 소모가 큰 객체를 사용하는 경우 성능을 최적화할 수 있습니다.
- **3. 보호(Protection) :** 특정 조건에서만 실제 객체에 접근할 수 있도록 보안을 강화할 수 있습니다.
- **4. 원격 접근 :** 원격 객체에 로컬 객체처럼 접근할 수 있도록 도와줍니다.

### 2️⃣ 프록시 패턴의 유형.
- **1. 가상 프록시(Virtual Proxy)**
    - 실제 객체의 생성을 지연시켜 성능을 최적화합니다.
        - 예를 들어, 이미지 로딩을 지연시켜 필요할 때만 로드할 수 있습니다.
- **2. 보호 프록시(Protection Proxy)**
    - 접근 권한이 없는 사용자나 클라이언트가 객체에 접근하는 것을 방지합니다.
        - 예를 들어, 특정 사용자만 큭정 기능을 사용할 수 있도록 제한할 수 있습니다.
- **3. 원격 프록시(Remote Proxy)**
    - 원격 서버에 위치한 객체를 로컬에서 사용하는 것처럼 보이도록 합니다.
        - 예를 들어, 분산 시스템에서 원격 메서드를 호출할 때 사용할 수 있습니다.
- **4. 스마트 참조 프록시(Smart Reference Proxy)**
    - 실제 객체에 대한 추가적인 행동을 수행합니다.
        - 예를 들어, 참조 횟수를 기록하거나, 객체에 접근할 때마다 로그를 남길 수 있습니다.

### 3️⃣ 프록시 패턴의 구조.
- 프록시 패턴은 다음과 같은 구성 요소로 이루어집니다.
    - **Subject(주제) :** 프록시와 실제 객체가 구현하는 인터페이스로, 공통된 메서드를 정의합니다.
    - **RealSubject(실제 주제) :** 실제로 작업을 수행하는 객체입니다. 클라이언트가 원래 접근하고자 하는 대상입니다.
    - **Proxy(프록시) :** RealSubject에 대한 참조를 가지고 있으며, RealSubject의 메서드를 호출하거나 접근을 제어하는 역할을 합니다.

### 4️⃣ 예제.
- 예를 들어, 이미지를 로딩하는 프로그램이 있다고 가정해봅시다.
- 이미지를 로드하는 작업은 시간이 오래 걸릴 수 있으므로, 이미지가 실제로 필요할 때까지 로딩을 지연시키고 싶습니다.
    - 이때 가상 프록시를 사용할 수 있습니다.

```java
interface Image {
    void display();
}

class RealImage implements Image {
    private String filename;
    
    public RealImage(String filename) {
        this.filename = filename;
        loadFromDisk();
    }
    
    private void loadFromDisk() {
        System.out.println("Loading " + filename);
    }
    
    @Override
    public void display() {
        System.out.println("Displaying " + filename);
    }
}

class ProxyImage implements Image {
    private RealImage realImage;
    private String filename;
    
    public ProxyImage(String filename) {
        this.filename = filename;
    }
    
    @Override
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(filename);
        }
        realImage.display();
    }
}

public class ProxyPatternExample {
    public static void main(String[] args) {
        Image image = new ProxyImage("test.jpg");
        
        // 실제로 이미지가 필요할 때만 로드합니다.
        image.display(); // Loading test.jpg, Displaying test.jpg
        image.display(); // Displaying test.jpg (이미 로드되었으므로 다시 로드하지 않습니다.)
    }
}
```

- 위 코드에서 `ProxyImage`는 `RealImage`에 대한 프록시 역할을 합니다.
    - `ProxyImage`를 통해 이미지를 로드할 때, 이미지를 실제로 필요할 때만 로드하도록 지연시킬 수 있습니다.
    - 두 번째 `display()` 호출에서는 이미 로드된 이미지가 있기 때문에 다시 로드하지 않습니다.
- 이렇게 프록시 패턴은 클라이언트의 코드 변경 없이 실제 객체의 접근 방식이나 동작을 변경하거나 확장하는 데 유용하게 사용할 수 있습니다.

### 5️⃣ Java에서의 프록시 패턴.
- 프록시 패턴(Proxy Pattern)은 Java에서도 매우 자주 사용되는 디자인 패턴 중 하나입니다.
    - 프록시 패턴은 어떤 객체에 대한 접근을 제어하기 위해 그 객체의 대리인 역할을 하는 별도의 객체(프록시 객체)를 제공하는 패턴입니다.
- Java에서 프록시 패턴은 다양한 상황에서 사용될 수 있으며, 대표적인 예시는 다음과 같습니다.
    - **1. 가상 프록시(Virtual Proxy)**
        - 실제 객체의 생성을 지연시키고, 필요할 때에만 객체를 생성하도록합니다.
            - 예를 들어, 메모리나 리소스가 큰 객체의 생성을 지연시켜 성능을 향상시킬 수 있습니다.
    - **2. 보안 프록시(Protectioon Proxy)**
        - 객체에 대한 접근을 제어하여, 특정 사용자나 조건에 따라 접근 권한을 부여하거나 제한할 수 있습니다.
            - 예를 들어, 사용자 인증이 필요한 시스템에서 특정 서비스나 자원에 접근할 때 사용됩니다.
    - **3. 원격 프록시(Remote Proxy)**
        - 원격 서버에 위치한 객체를 로컬에서 사용하는 것처럼 보이도록 하는 패턴입니다.
            - Java RMI(Remote Method Invocation)가 대표적인 예입니다.
    - **4. 캐싱 프록시(Caching Proxy)**
        - 반복적으로 호출되는 메서드나 객체의 결과를 캐싱하여 성능을 최적화합니다.

### 6️⃣ Java에서 프록시 패턴을 구현하는 방법.
- **프록시 클래스를 직접 구현**
    - 인터페이스를 구현하는 프록시 클래스를 생성하여, 실제 객체에 대한 접근을 제어합니다.
- **Java Dynamic Proxy**
    - Java의 `java.lang.reflect.Proxy` 클래스를 이용하여 런타임에 동적으로 프록시 객체를 생성할 수 있습니다.
        - 이 방법은 인터페이스 기반의 프록시 생성에 유용합니다.
- **CGLIB**
    - 인터페이스가 없는 클래스에 대해서도 프록시를 생성할 수 있도록 지원하는 라이브러리입니다.
        - Spring 프레임워크에서는 주로 AOP(Aspect-Oriented Programming) 기능을 구현할 때 CGLIB을 활용합니다.
- 프록시 패턴은 특히 Spring 프레임워크에서 AOP를 구현하거나, 트랜잭션 관리와 같은 크로스컷팅(Cross-cutting) 관심사를 처리할 때 널리 사용됩니다.

## 2️⃣ 프록시 서버(Proxy Server).
- 프록시 서버(Proxy Server)는 컴퓨터 네트워크에서 클라이언트와 서버 간의 중계자 역할을 하는 서버입니다.
- 프록시 서버는 클라이언트가 요청한 리소스(웹페이지, 파일 등)를 대신 받아서 클라이언트에게 전달하거나, 클라이언트의 요청을 다른 서버로 전달합니다.
    - 이를 통해 여러 가지 중요한 기능을 수행할 수 있습니다.

### 1️⃣ 프록시 서버의 주요 기능.
- **1. 익명성 제공 :** 클라이언트의 IP 주소를 숨기고, 대신 프록시 서버의 IP 주소로 서버에 접근합니다. 이를 통해 클라이언트는 익명성을 유지할 수 있으며, 서버는 클라이언트의 실제 IP 주소를 알지 못합니다.
- **2. 보안 강화 :** 프록시 서버는 클라이언트와 서버 간의 트래픽을 모니터링하고 제어할 수 있습니다. 이를 통해 불필요한 요청을 차단하거나, 특정 웹사이트 접근을 제한할 수 있습니다. 또한, 악성 트래픽을 필터링하여 네트워크 보안을 강화할 수 있습니다.
- **3. 캐싱(Caching) :** 프록시 서버는 자주 요청되는 리소스를 캐싱하여 서버의 부하를 줄이고, 클라이언트의 요청에 빠르게 응답할 수 있습니다. 예를 들어, 동일한 웹페이지가 여러 번 요청되는 경우, 프록시 서버는 이 페이지를 캐시에 저장해 두고, 이후 요청에 대해 캐시에서 직접 응답합니다.
- **4. 콘텐츠 필터링 :** 프록시 서버는 특정 콘텐츠에 대한 접근을 제한하거나 차단할 수 있습니다. 이는 학교, 기업 또는 가정에서 특정 웹사이트나 콘텐츠에 대한 접근을 제어하는 데 유용합니다.
- **5. 로드 밸런싱 :** 프록시 서버는 여러 서버에 걸쳐 트래픽을 분산시켜 로드 밸런싱을 할 수 있습니다. 이를 통해 서버의 부하를 균등하게 나누고, 시스템의 성능과 안정성을 향상시킬 수 있습니다.
- **6. 데이터 압축 :** 프록시 서버는 클라이언트와 서버 간에 주고받는 데이터를 압축하여 네트워크 트래픽을 줄이고, 응답 속도를 향상시킬 수 있습니다.

### 2️⃣ 프록시 서버의 유형.
- **1. 정방향 프록시(Forward Proxy)** 
    - 클라이언트와 서버 사이에서 클라이언트의 요청을 대신 서버로 전달합니다.
    - 주로 클라이언트가 프록시 서버를 통해 외부 네트워크에 접근할 때 사용됩니다.
        - 예를 들어, 웹 브라우저 설정에서 프록시 서버를 저장하면, 모든 웹 트래픽이 이 프록시 서버를 통해 전달됩니다.
- **2. 리버스 프록시(Reverse Proxy)**
    - 서버와 클라이언트 사이에서 서버를 대신하여 클라이언트의 요청을 처리합니다.
    - 주로 웹 서버 앞단에 위치하여 서버의 부하를 줄이고, 보안을 강화하며, 로드 밸런싱을 수행합니다.
    - 리버스 프록시는 클라이언트가 실제 서버의 위치를 알지 못하게 하여 보안을 강화할 수 있습니다.
- **3. 웹 프록시(Web Proxy)**
    - 웹 브라우저를 통해 특정 웹사이트에 접근할 때 사용하는 프록시 서버입니다.
    - 사용자는 웹 프록시 사이트를 통해 차단된 웹사이트에 접근하거나, 익명으로 웹을 탐색할 수 있습니다.
- **4. 트랜스페어런트 프록시(Transparent Proxy)**
    - 클라이언트가 프록시 서버를 사용하고 있다는 사실을 모르게 하면서 트래픽을 중계하는 프록시 서버입니다.
    - 주로 ISP(인터넷 서비스 제공자)나 기업 네트워크에서 트레픽을 모니터링하거나 필터링하는 데 사용됩니다.

### 3️⃣ 프록시 서버의 예.
- **회사 네트워크에서의 프록시 서버**
    - 회사는 직원들이 인터넷에 접근할 때 프록시 서버를 통해 접근하도록 설정할 수 있습니다.
    - 이 프록시 서버는 직원들이 어떤 웹사이트에 접근하는지 모니터링하고, 필요에 따라 특정 사이트에 대한 접근을 차단할 수 있습니다.
- **공공 Wi-Fi의 프록시 서버**
    - 공공 Wi-Fi 네트워크는 프록시 서버를 통해 사용자 트래픽을 모니터링하고, 보안을 강화할 수 있습니다.
    - 이를 통해 악성 사이트에 대한 접근을 차단하거나, 네트워크를 통해 전송되는 데이터를 암호화할 수 있습니다.
- 프록시 서버는 네트워크의 성능, 보안, 그리고 사용자 경험을 개선하기 위한 강력한 도구로 사용됩니다.

### 4️⃣ Java에서의 프록시 서버(Proxy Server) 사용 사례.
- 프록시 서버는 네트워크 환경에서 클라이턴트와 서버 간의 중계자 역할을 하는 서버입니다.
    - Java에서는 다양한 상황에서 프록시 서버를 사용할 수 있으며, 그 활용 방법도 매우 다양합니다.
- **1. HTTP/HTTPS 프록시**
    - Java 애플리케이션이 외부 네트워크에 요청을 보내야 할 때, 프록시 서버를 통해 트래픽을 중계할 수 있습니다.
    - 이를 통해 보안, 로깅, 캐싱, IP 마스킹 등을 수행할 수 있습니다.
```java
System.setProperty("http.proxyHost", "proxy.example.com");
System.setProperty("http.proxyPort", "8080");
System.setProperty("https.proxyHost", "proxy.example.com");
System.setProperty("https.proxyPort", "8080");
```

- 위와 같이 시스템 프로퍼티를 설정하여 Java 애플리케이션이 HTTP 및 HTTPS 요청을 보낼 때 프록시 서버를 사용하도록 할 수 있습니다.

- **2. SOCKS 프록시**
    - Java에서는 SOCKS 프록시를 사용하여 TCP/IP 기반의 모든 연결을 프록시 서버를 통해 중계할 수 있습니다.
    - SOCKS 프록시는 HTTP/HTTPS 프록시보다 더 일반적인 트래픽을 처리할 수 있습니다.
```java
System.setProperty("socksProxyHost", "proxy.example.com");
System.setProperty("socksProxyPort", "1080");
```

- 이렇게 설정하면 Java 애플리케이션이 TCP/IP 연결을 시도할 때 SOCKS 프록시 서버를 통해 연결을 시도합니다.

- **3. RMI(Remote Method Invocation) 프록시**
    - Java RMI는 분산 애플리케이션을 구현하기 위해 원격 메서드 호출을 가능하게 하는 기술입니다.
    - RMI에서 프록시를 사용하여 클라이언트가 원격 객체에 접근할 때 로컬 객체처럼 접근할 수 있도록 합니다.
- **4. Spring Clould Gateway**
    - 마이크로서비스 아키텍처에서 Java로 개발된 서비스들을 연결하고 API 게이트웨이 역할을 수행하는 데 Spring Clould Gateway와 같은 프록시 서버 역할을 하는 프레임워크를 사용할 수 있습니다.
    - 이는 마이크로서비스 간의 통신을 관리하고, 보안, 로깅, 인증/인가를 담당하는 데 사용됩니다.
- **5. Reverse Proxy**
    - Java 웹 애플리케이션 서버는 리버스 프록시 서버 뒤에서 실행될 수 있습니다.
    - 예를 들어, Nginx나 Apache HTTP Server를 프록시로 설정하여 클라이언트의 요청을 Java 웹 애플리케이션으로 전달할 수 있습니다.

### 5️⃣ 프록시 서버 사용의 장점.
- **보안 강화**
    - 클라이언트와 서버 사이의 트래픽을 모니터링하고, 특정 요청을 차단하거나 허용할 수 있습니다.
- **캐싱**
    - 프록시 서버는 자주 요청되는 데이터를 캐시하여 서버 부하를 줄이고 응답 속도를 향상시킬 수 있습니다.
- **트래픽 관리**
    - 프록시 서버는 네트워크 트래픽을 관리하고, 로드 밸런싱과 같은 기능을 통해 시스템의 성능을 최적화할 수 있습니다.
- **IP 마스킹**
    - 클라이언트의 IP 주소를 숨기고, 프록시 서버의 IP 주소를 대신 사용할 수 있습니다.

- Java 애플리케이션에서 프록시 서버를 사용하는 것은 네트워크 환경을 제어하고 보안을 강화하며 성능을 최적화하는데 매우 유용합니다.