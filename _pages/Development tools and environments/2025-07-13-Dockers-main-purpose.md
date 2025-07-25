---
title: 🛠️[개발 도구 및 환경] Docker의 주요 목적.
tags:
    - Development tools
    - Enviroments
    - Docker
date: "2025-07-13"
thumbnail: "/assets/img/thumbnail/tools.jpg"
---

# 🛠️[개발 도구 및 환경] Docker의 주요 목적.
⭐️ Docker의 주요 목적은 **"어디서든 동일하게 실행되는, 가볍고 빠르고 이식성 높은 애플리케이션 환경을 만드는 것"** 입니다.

이해하기 쉬운 비유를 통해 설명해 드리겠습니다.

**🧐 "내 컴퓨터에서는 잘되는데..." 문제의 해결사.**

개발자들이 가장 흔하게 겪는 문제가 있습니다.
```
개발자 A가 자신의 노트북에서 열심히 코드를 짜서 기능을 완성했습니다.
하지만 그 코드를 동료 개발자 B의 노트북이나, 테스트 서버, 실제 운영 서버로 옮기면 온갖 오류가 발생합니다.

* "어? 저는 파이썬 3.8 버전인데, 거기는 3.9 버전이네요."
* "필요한 라이브러리가 설치되지 않았어요."
* "운영체제가 달라서 경로 설정이 꼬였어요."
```

이것이 바로 **"내 컴퓨터에서는 잘 되는데...(It works on my machine...)" 문제입니다.**

**📦 Docker의 해결책: 해상 운송 컨테이너**

Docker는 이 문제를 **해상 운송 컨테이너**와 똑같은 아이디어로 해결합니다.

과거에는 배로 물건을 보낼 때, 상자, 통, 자루 등 제각각 다른 형태로 실어서 공간도 낭비되고 파손도 잦았습니다.
하지만 규격화된 '컨테이너'가 등장하면서 모든 것이 바뀌었습니다.

어떤 물건이든 이 표준 컨테이너에만 담으면, 전 세계 어떤 항구의 어떤 크레인, 트럭, 기차로도 쉽고 안전하게 운송할 수 있게 되었습니다.

Docker가 바로 **소프트웨어 세계의 '컨테이너'** 입니다.

애플리케이션을 실행하는 데 필요한 모든 것, 즉 **1️⃣ 코드, 2️⃣ 실행 환경(런타임) 3️⃣ 관련 라이브러리, 4️⃣ 설정 파일** 등을 하나로 싹 묶어서 'Docker 이미지'라는 표준화된 상자에 담습니다.

그리고 이 '이미지'를 실행한 것이 바로 **'Docker 컨테이너'** 입니다.

---

## 📦 Docker의 핵심 목적 5가지
1. **환경의 일관성 (Consistency):**
· 개발자의 노트북, 테스트 서버, 운영 서버 등 어디서든 동일한 환경을 보장합니다. "내 컴퓨터에서는 되는데... " 문제를 근본적으로 해결합니다.

2. **이식성 (Portability):**
· 한 번 Docker 이미지로 만들어두면, Docker가 설치된 어떤 컴퓨터나 클라우드 서버에서도 수정 없이 바로 실행할 수 있습니다. "한 번 빌드해서, 어디서든 실행한다(Build once, run anywhere)."

3. **가볍고 빠른 실행 (Lightweight & Fast):**
· 기존의 가상머신(VM)처럼 운영체제(OS)를 통째로 설치하는 방식이 아닙니다. 호스트 컴퓨터의 OS는 공유하면서, 애플리케이션과 그에 필요한 라이브러리만 격리하여 실행하기 때문에 훨씬 가볍고, 빠르며, 적은 자원을 소모합니다.

4. **격리 (Isolation):**
· 각 컨테이너는 서로 완전히 격리된 공간에서 실행됩니다. 따라서 하나의 컨테이너에 문제가 생겨도 다른 컨테이너에 영향을 주지 않으며, 컨테이너끼리 라이브러리 버전이 충돌할 걱정도 없습니다.

5. **손쉬운 확장 (Scalability):**
· 사용자가 몰려서 더 많은 서버 용량이 필요할 때, 똑같은 컨테이너를 몇 개 더 실행하기만 하면 됩니다. 이는 현대적인 마이크로서비스 아키텍처(MSA)를 구현하는 핵심 기술이 됩니다.

---

## 💡 Summary.
- **Docker의 주요 목적은 애플리케이션을 그 실행 환경과 함께 '포장'하여, 개발부터 배포, 운영에 이르는 전 과정을 더 빠르고, 안정적이며, 효율적으로 만드는 것입니다.**
