---
title: "📚[Backend Development] 스프링 부트의 핵심 개념 - 1. 의존성 주입(DI, Dependency Injection)🙌"
tags:
    - Backend Ddevelopment
    - Spring Boot
    - Server
    - Framework
date: "2025-08-02"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# ✅ 1. 스프링 부트의 핵심 개념.
자바에서는 프로그램을 만들 때 기능별로 클래스를 나누고,
이 클래스들이 서로를 **생성하고 호출하며 동작**합니다.
이 과정에서 클래스 간에는 자연스럽게 **"의존성 관계"** 가 생기게 됩니다.


<img src = "https://github.com/devKobe24/images2/blob/main/spring-boot.png?raw=true">

스프링 부트는 이런 복잡한 관계를 더 쉽게 관리하기 위해
다음과 같은 핵심 개념을 사용합니다:

---

### 🛠️ **핵심 개념 정리**

**🔁 의존성 주입(DI, Dependency Injection)**
➞ 필요한 객체를 직접 생성하지 않고, 외부에서 **주입받는 방식**

> 예: "내가 직접 재료를 사오는 대신, 셰프가 다 준비해주는 느낌 🍳"

**🧭 제어의 역전 (IoC, Inversion of Control)**
➞ 객체를 누가 만들고 연결할지 결정하는 **제어 권한을 스프링에게 넘김**

> 예: "모든 순서를 내가 짜는 게 아니라, 스프링이 대신 요리를 조율 🎛️"

**📦 스프링 컨테이너**
➞ 위 두 개념을 실제로 관리하고 **실행하는 핵심 공간**

> 스프링이 모든 객체를 넣고 꺼내면서 앱을 굴리는 비밀 창고 🔐

---

### 🚀 왜 중요한가요?

앞으로 우리가 만드는 모든 **스프링 부트 애플리케이션**은
개발자가 작성한 클래스들과 스프링이 제공하는 클래스들이
이 **스프링 컨테이너 안에서 연결되고 작동합니다.**

> 따라서 **의존성 주입(DI), 제어의 역전(IoC),** 그리고 이를 바탕으로 동작하는
> **스프링 컨테이너** 구조를 이해하는 것은
> 스프링 부트를 제대로 사용하는 데 꼭 필요한 첫걸음입니다. ✌️

---

## ✅ 1. 의존성 주입 (DI, Dependency Injection) 쉽게 이해하기

### ✅ 클래스 간의 관계, 바로 "의존성"

자바 프로그램을 짜다 보면,
클래스 A가 어떤 기능을 수행하려면 클래스 B가 필요할 때가 많습니다.

> 🔁 이럴 때 우리는 **"A는 B에 의존한다"** 라고 표현합니다.

예를 들어,
CoffeeMaker 클래스가 커피를 만들기 위해 EspressoMachine이라는 클래스를 사용한다면,
CoffeeMaker ➞ EspressoMachine 방향으로 **의존성이 존재**하는 것이죠.

---

### 📌 용어 정리: 의존성과 의존성 주입의 차이.

| 용어 | 설명 | 예시 |
| -------- | -------- | -------- |
| 의존성 관계 (Dependency Relationship) | 한 객체가 다른 객체의 기능을 사용해야 할 때 성립하는 기능적 관계 | CoffeeMaker가 EspressoMachine의 brew()를 호출함 |
| 의존성 주입 (Dependency Injection, DI) | 의존 객체를 직접 생성하지 않고, 외부에서 전달받는 설계 방식 | 생성자, 메서드, 필드 등을 주입받음 |

> ✅ 즉, **의존성 주입 없이도 의존성 관계는 존재할 수 있습니다.**

---

### 🧑‍💻 직접 객체를 만들어 사용하는 구조.

<img src="https://github.com/devKobe24/images2/blob/main/DI.001.jpeg?raw=true">

먼저 커피를 추출해주는 EspressoMachine 클래스부터 만들어봅시다:

```java
public class EspressoMachine {
    public String brew() {
        return "Brewing coffee with Espresso Machine";
    }
}
```

> 이 클래스는 커피를 "내리는 기능"만 담당하는 **에스프레소 추출기**입니다 ☕️

---

### 🛠️ 커피 메이커가 에스프레소 머신을 '직접' 생성하면?

<img src="https://github.com/devKobe24/images2/blob/main/DI.002.jpeg?raw=true">

이제 이 EspressoMachine을 사용하는 CoffeeMaker 클래스를 보겠습니다.

```java
public class CoffeeMaker {
    private EspressoMachine espressoMachine;

    public CoffeeMaker() {
        this.espressoMachine = new EspressoMachine();  // ❗ 직접 생성
    }

    public void makeCoffee() {
        System.out.println(espressoMachine.brew());
    }
}
```

- ✅ 이 구조는 CoffeeMaker가 EspressoMachine을 사용하므로 **의존성 관계(Dependency Relationship)는 존재**합니다.
- ❌ 하지만 EspressoMachine을 직접 생성하므로 **의존성 주입(Dependency Injection, DI)은 적용되지 않은 구조**입니다.

> 🛠️ 의존성 객체를 직접 만들면 변경・확장이 어렵고,테스트도 불편해집니다.
> 👉 그래서 스프링에서는 **의존성 주입 방식(Depenedency Injection, DI)** 을 통해 이 문제를 해결합니다!

---

### 🚀 실행 흐름

<img src="https://github.com/devKobe24/images2/blob/main/DI.003.jpeg?raw=true">

```java
public class Main {
    public static void main(String[] args) {
        CoffeeMaker coffeeMaker = new CoffeeMaker();
        coffeeMaker.makeCoffee();
    }
}
```

```plaintext
# 실행 결과
Brewing coffee with Espresso Machine
```

커피가 잘 내려졌네요! ☕️👍
하지만 이 구조는 아직 **의존성 주입(Dependency Injection, DI)가 적용되지 않았기 때문에 확장성과 유연성이 떨어지는 구조**입니다.

---

### 🔁 클래스 간의 단단한 결합? 느슨한 결합으로 바꾸자!

작성한 코드에서 CoffeeMaker가 커피를 만들기 위해 EspressoMachine이 필요하다는 건,
바로 두 클래스 사이에 **“의존성 관계(Dependency Relationship)”** 가 있다는 뜻입니다.

---

#### ❗️ 그런데 직접 생성하면 어떤 문제가 생길까요?

일반적으로 자바에서는 클래스가 필요한 의존 객체를 **직접 생성**합니다.
그런데 이 구조는 다음과 같은 **단점이 있습니다:**

> ⚠️ 의존 객체가 바뀌면, 그 객체를 사용하는 코드도 같이 바뀌어야 한다는 점!

---

#### 🧪 예시: 진한 커피 ➞ 부드러운 커피로 바꾸고 싶다면?

이번엔 진한 커피 대신 **부드러운 드립 커피**를 만들어보려 합니다.
새로운 클래스 DripCoffeeMachine()을 만들고, 동일하게 brew() 기능을 구현합니다.

<img src="https://github.com/devKobe24/images2/blob/main/DI_4.jpeg?raw=true">

그리고 CoffeeMaker가 EspressoMachine 대신
DripCoffeeMachine을 사용하도록 모든 코드를 수정합니다.

<img src="https://github.com/devKobe24/images2/blob/main/DI_5.jpeg?raw=true">

```plaintext
#실행 결과
Brewing coffee with Drip Coffee Machine
```

결과는 잘 나왔지만...
💥문제는 바뀐 커피 머신에 맞춰
CoffeeMaker 내부 코드를 **전부 수정해야 한다는 점**입니다.

> 이처럼 한 클래스의 변경이 다른 클래스에도 영향을 주는 관계를
> 👉 **단단한 결합(Tight Coupling)** 이라고 합니다.

---

#### 💣 기능이 더 늘어나면?
- brew() 뿐만 아니라
- grind(), steam() 같은 기능도 CoffeeMaker가 사용하도 있었다면?

➞ 새로운 머신을 만들 때마다
CoffeeMaker까지 수정해야 하는 상황이 벌어집니다. 😓

---

### ✅ 해결책: 느슨한 결합(Loose Coupling) + 인터페이스

> **"CoffeeMaker"가 특정 머신을 직접 알 필요가 없게 만들면 어떨까?**

---

#### 🧩 Step 1: 공통 인터페이스 만들기

brew() 기능은 머신마다 다르지만 **동일한 규격을 가지므로**
CoffeeMachine이라는 인터페이스를 정의합니다.

<img src="https://github.com/devKobe24/images2/blob/main/DI_6.jpeg?raw=true">

---

#### 🧩 Step 2: 각 머신이 인터페이스를 구현

EspressoMachine과 DripCoffeeMachine 모두
CoffeeMachine 인터페이스를 구현합니다.

<img src="https://github.com/devKobe24/images2/blob/main/DI_7.jpeg?raw=true">

<img src="https://github.com/devKobe24/images2/blob/main/DI_8.jpeg?raw=true">

---

#### 🧩 Step 3: CoffeeMaker가 더 이상 직접 만들지 않음

이제 CoffeeMaker는 어떤 머신이든 상관없이
**CoffeeMachine 인터페이스만 보고 프로그래밍합니다.**

```java
public class CoffeeMaker {
    private CoffeeMachine coffeeMachine;

    public void setCoffeeMachine(CoffeeMachine coffeeMachine) {
        this.coffeeMachine = coffeeMachine;
    }

    public void makeCoffee() {
        System.out.println(coffeeMachine.brew());
    }
}
```

<img src="https://github.com/devKobe24/images2/blob/main/DI_9.jpeg?raw=true">

---

#### 🧩 Step 4: 외부에서 원하는 머신을 주입

메인 클래스에서 원하는 머신을 선택해서 CoffeeMaker에 전달합니다.

<img src="https://github.com/devKobe24/images2/blob/main/DI_10.jpeg?raw=true">

이제는 새로운 머신(MokaCoffeeMachine 등)을 만들어도
**CoffeeMaker** 코드는 **단 1줄도 건드릴 필요가 없습니다! ✌️**

---

### 📝 마무리 요약: 느슨한 결합(Loose Coupling) + 의존성 주입(DI, Dependency Injection)의 힘

- ✅ 클래스 간 **의존성 관계(Dependency Relationship)** 는 그대로 유지하되
- ✅ 직접 생성하는 대신 **외부에서 주입**받고
- ✅ 공통 **인터페이스**로 연결하면
- ✅ 언제든지 유연하게 확장 가능한 구조가 됩니다!

---

> 💡 이처럼 클래스를 느슨하게 연결하고,
> 필요한 객체를 외부에서 전달해 사용하는 구조를
> 👉 **의존성 주입(Dependency Injection, DI)** 이라고 합니다.

---

### 🎯 스프링 부트에서 DI(Dependency Injection, 의존성 주입)는 기본입니다.

스프링 부트 애플리케이션에서는
우리가 직접 객체를 생성하지 않고도
스프링이 **알아서 객체를 생성하고, 주입해줍니다. 🤖**

그 비밀은 바로 이 DI 구조에 있습니다.
