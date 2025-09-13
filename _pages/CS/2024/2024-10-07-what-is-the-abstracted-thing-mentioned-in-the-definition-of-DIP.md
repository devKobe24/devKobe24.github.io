---
title: "💾 [CS] DIP의 정의에서 말하는 '추상화된 것'이란 무엇일까?"
tags:
    - CS
date: "2024-10-07"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# "💾 [CS] DIP의 정의에서 말하는 '추상화된 것'이란 무엇일까?"

우리가 잘 알고있는 **SOLID 원칙** 중 **DIP(의존성 역전 원칙, Dependency Inversion Principle)** 정의는 다음과 같습니다.

- "고수준 모듈은 저수준 모듈에 의존해서는 안 되고, 둘 다 추상화 된 것에 의존해야 합니다."
    - 여기서 **고수준 모듈**은 **"비즈니스 로직"** 을 뜻하고, **저수준 모듈**은 **세부 구현**을 뜻합니다.

그렇다면 **"추상화된 것"** 이란 어떤 의미일까요?

> 🙋‍♂️ [SOLID 원칙](https://www.devkobe24.com/CS/2024/2024-10-03-SOLID.html)
> 🙋‍♂️ [의존성](https://www.devkobe24.com/Backend/Spring/2024-10-04-Dependency.html)

## 1️⃣ DIP의 정의에서 말하는 "추상화된 것"이란?
- **DIP(Dependency Inversion Principle, 의존성 역전 원칙)** 의 정의에서 말하는 **"추상화된 것"** 은 **인터페이스 또는 추상 클래스를 의미합니다.**
- 이 원칙에서 "추상화된 것에 의존해야 한다"는 것은, 구체적인 구현 클래스가 아닌 **인터페이스나 추상 클래스 같은 추상적인 계층을 통해 상호작용해야 한다는 의미입니다.**

## 2️⃣ 구체적인 설명.
- **DIP(Dependency Inversion Principle, 의존성 역전 원칙)** 의 기본 개념은 **고수준 모듈(비즈니스 로직)** 과 **저수준 모듈(세부적인 구현)** 간의 의존성을 **추상화된 계층**으로 뒤집는 것입니다.
- 여기서 말하는 **"추상화된 것"** 은 **구현이 아닌 계약을** 의미하며, 이 계약은 인터페이스나 추상 클래스를 통해 정의됩니다.
    - **고수준 모듈 :** 시스템의 상위 레벨에 위치하는 모듈로, 일반적으로 **비즈니스 로직을 처리합니다.**
    - **저수준 모듈 :** 구체적인 세부 사항이나 기술적인 구현을 담당하는 모듈입니다. 예를 들어, 데이터베이스 처리, 파일 시스템 작업들이 해당됩니다.

> 🙋‍♂️ [비즈니스 로직(Business Logic)이란?](https://www.devkobe24.com/CS/2024/2024-09-02-Business-Logic.html)
> 🙋‍♂️ [소프트웨어 공학에서의 모듈](https://www.devkobe24.com/CS/2024/2024-10-07-modules-in-software-engineering.html)
> 🙋‍♂️ [모듈과 컴포넌트를 레고 불록에 비유해보면?!](https://www.devkobe24.com/CS/2024/2024-10-07-compare-modules-and-components-to-lego-blocks.html)

### 예시
- 먼저 DIP를 위반하는 코드 예시를 보겠습니다.
    - 이 코드는 고수준 모듈이 저수준 모듈의 **구체적인 구현**에 **직접적으로 의존하는 경우**입니다.

```java
class EmailService {
    public void sendEmail(String message) {
        // 이메일 전송 로직
        System.out.println("Email sent: " + message);
    }
}

class NotificationManager {
    private EmailService emailService;
    
    public NotificationManager() {
        this.emailService = new EmailService(); // 저수준 모듈의 구체적 구현에 의존
    }
    
    public void sendNotification(String message) {
        emailService.sendEmail(message);
    }
}
```

- 이 코드에서는 `NotificationManager`(고수준 모듈)가 `EmailService`(저수준 모듈)에 직접 의존하고 있습니다.
- 만약 `NotificationManager`가 다른 알림 방법(예: SMS)을 사용하고 싶다면, `EmailService`와 같은 저수준 모듈을 수정하거나 교체해야 하므로 유연성이 떨어집니다.

### DIP 적용 (추상화된 것에 의존)
- DIP를 적용하면, **고수준 모듈과 저수준 모듈** 모두 **추상화된 계층(인터페이스나 추상 클래스)** 에 의존하게 됩니다.
    - 이렇게 하면 고수준 모듈은 저수준 모듈의 구체적인 구현에 의존하지 않게 되어, 더 유연하고 확장 가능한 코드가 됩니다.

```java
// 추상화된 것: Interface 정의 (추상화된 계층)
interface NotificationService {
    void sendNotification(String message);
}

// 저수준 모듈: 구체적인 구현체
class EmailService implements NotificationService {
    public void sendNotification(String message) {
        System.out.println("SMS sent: " + message);
    }
}

// 저수준 모듈: 또 다른 구체적인 구현체
class SMSService implements NotificatioonService {
    public void sendNotificatioon(String message) {
        System.out.println("SMS sent: " + message);
    }
}

// 고수준 모듈: 추상화된 인터페이스에 의존
class NotificationManager {
    private NotificationService notificationService;
    
    public NotificationManager(NotificationService notificationService) {
        this.notificationService = notificationService // 추상화된 것에 의존
    }
    
    public void sendNotification(String message) {
        notificationService.sendNotification(message);
    }
}
```

#### 설명.
- **추상화된 것**
    - `NotificationService` 인터페이스는 **추상화된 것**입니다.
    - 고수준 모듈(`NotificationManager`)은 이제 구체적인 `EmailService`나 `SMSService`에 의존하지 않고, `NotificationService`라는 추상화된 인터페이스에 의존합니다.
- **구체적인 구현**
    - `EmailService`와 `SMSService`는 각각 `NotificationService` 인터페이스를 구현한 **저수준 모듈**입니다.

#### 결과.
- 이렇게 하면 `NotificatioonManager`는 `EmailService` 또는 `SMSService` 중 어떤 구현체를 사용하든지 상관하지 않습니다.
- 다른 알림 서비스가 필요하다면, 단순히 `NotificationService`를 구현하는 새로운 클래스를 만들고, 이를 `NotificationManager`에 전달하면 됩니다.
- 즉, **고수준 모듈과 저수준 모듈이 모두 추상화된 인터페이스에 의존하게 되어, 코드의 유연성과 확장성이 크게 향상됩니다.**

## 3️⃣ 결론.
- DIP에서 말하는 **"추상화된 것"** 은 **구체적인 구현체가 아닌 인터페이스 또는 추상 클래스를 의미합니다.**
- 고수준 모듈과 저수준 모듈 모두 이 추상화된 계층에 의존함으로써, 각 모듈 간 결합을 줄이고 유연한 시스템을 구축할 수 있습니다.
