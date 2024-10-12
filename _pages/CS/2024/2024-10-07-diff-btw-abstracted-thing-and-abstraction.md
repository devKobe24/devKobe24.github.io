---
title: "💾 [CS] DIP의 정의에서 말하는 '추상화된 것'과 '추상화'의 개념의 차이점."
tags:
    - CS
date: "2024-10-07"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# "💾 [CS] DIP의 정의에서 말하는 '추상화된 것'과 '추상화'의 개념의 차이점."
- **DIP(Dependency Inversion Principle)** 에서 말하는 "추상화된 것"과 **소프트웨어 공학**에서 사용하는 일반적인 "추상화" 개념은 서로 밀접하게 관련된 개념이지만, 문맥에 따라 강조점이 다를 수 있습니다.

## 1️⃣ DIP에서 말하는 "추상화된 것"
- DIP에서 말하는 **"추상화된 것"** 은 **구체적인 구현체가 아닌 인터페이스나 추상 클래스를 지칭합니다.**
- 이 원칙에 따르면, 고수준 모듈(비즈니스 로직)은 저수준(세부적인 구현)에 의존해서 안 되며, 대신 두 모듈 모두 **인터페이스나 추상 클래스** 같은 **추상화된 것에 의존해야 합니다.**
- 이는 DIP에서 핵심적인 개념으로, **구현체 대신 계약(인터페이스)와 상호작용**하도록 유도합니다.

> 🙋‍♂️ [DIP의 정의에서 말하는 '추상화된 것'이란 무엇일까?](https://www.devkobe24.com/CS/2024/2024-10-07-what-is-the-abstracted-thing-mentioned-in-the-definition-of-DIP.html)
> 🙋‍♂️ [소프트웨어 공학에서의 모듈](https://www.devkobe24.com/CS/2024/2024-10-07-modules-in-software-engineering.html)
> 🙋‍♂️ [모듈과 컴포넌트를 레고 블록에 비유해보면?!](https://www.devkobe24.com/CS/2024/2024-10-07-compare-modules-and-components-to-lego-blocks.html)
> 🙋‍♂️ [비즈니스 로직(Business Logic)이란?](https://www.devkobe24.com/CS/2024/2024-09-02-Business-Logic.html)

### 예시
```java
// "추상화된 것"인 인터페이스
interface NotificationService {
    void sendNotification(String message);
}
```

- 위의 `NotificationService` 인터페이스는 DIP의 문맥에서 말하는 "추상화된 것"입니다.
- 고수준 모듈과 저수준 모듈은 이 추상화된 인터페이스를 통해서만 서로 상호작용합니다.

## 2️⃣ 일반적인 "추상화" 개념.
- 소프트웨어 공학에서의 **"추상화"** 는 더 **일반적인 개념**으로, **세부사항을 숨기고 중요한 정보만을 드러내는 설계 기법을 의미합니다.**
- 이를 통해 시스템의 복잡성을 줄이고, 설계나 구현에서 핵심적인 부분에 집중할 수 있게 해줍니다.
- **"추상화"** 는 클래스, 인터페이스, 함수 등 다양한 수준에서 사용될 수 있으며, 주로 복잡한 시스템을 이해하기 쉽게 만들기 위해 사용됩니다.
- **"추상화"** 는 구현 세부 사항을 감추고, 객체나 모듈 간의 상호작용을 간단하고 일관되게 만들어 줍니다.

### 예시.
```java
abstract class Shape {
    abstract void draw();
}
```

- 위의 `Shape` 클래스는 일반적인 추상화의 예로, 다양한 구체적인 도형(원, 사각형 등)의 세부 구현을 감추고 `draw()`라는 공통된 동작을 정의한 것입니다.

## 3️⃣ 차이점.
- **DIP에서의 "추상화된 것"**
    - DIP에서 말하는 "추상화된 것"은 **구현이 아닌 계약을 제공하는 인터페이스나 추상 클래스를 가리킵니다.**
        - 이 계약에 의존함으로써 고수준 모듈과 저수준 모듈의 결합도를 낮춥니다.
    - **즉, 구현이 아닌, 추상적인 계약에 의존함**으로써 유연성을 확보하고 의존성의 방향을 역전시킵니다.
- **일반적인 "추상화"**
    - **일반적인 "추상화"** 는 더 광범위한 개념으로, **시스템 내에서 세부사항을 감추고 본질적인 개념만 드러내는 설계 기법**입니다.
        - 추상 클래스, 인터페이스뿐만 아니라, 복잡한 로직을 간단하게 만들기 위한 여러 기법이 포함됩니다.

## 4️⃣ 공통점
- **둘 다 세부 사항을 숨긴다.**
    - DIP에서의 "추상화된 것"과 일반적인 "추상화" 모두 구체적인 구현 세부 사항을 감추고, 더 중요한 부분(주로 인터페이스나 상호작용)에 집중하도록 설계합니다.
- **유연성 제공**
    - 두 개념 모두 코드의 유연성을 증가시키고, 변경에 쉽게 대응할 수 있는 구조를 만드는데 기여합니다.

## 5️⃣ 결론
- **DIP에서의 "추상화된 것"** 은 DIP 원칙을 적용할 때 구체적인 구현 대신 인터페이스나 추상 클래스와 같은 추상적 계층에 의존하도록 하는 구체적인 의미를 가집니다.
- **일반적인 "추상화"** 는 소프트웨어 설계에서 복잡성을 관리하기 위해 사용하는 넓은 범위의 설계 개념으로, 객체 지향 프로그래밍의 기본 원칙 중 하나입니다.

결국, **DIP에서 말하는 "추상화된 것"은 일반적인 "추상화"의 측정한 적용 사례**라고 볼수 있습니다.
DIP는 추상화를 통해 시스템 간의 결합도를 낮추고 유연성을 높이는 것을 목표로 합니다.