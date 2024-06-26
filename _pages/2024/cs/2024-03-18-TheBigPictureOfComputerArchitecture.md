---
title: "💾 [CS] 컴퓨터 구조의 큰 그림"
tags:
    - CS
date: "2024-03-18"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 컴퓨터 구조의 큰 그림

우리가 알아야 할 컴퓨터 구조 지식은 크게 두 가지 입니다.
1. 컴퓨터가 이해하는 정보
2. 컴퓨터의 네 가지 핵심 부품

## 컴퓨터가 이해하는 정보

1. 데이터
    * 컴퓨터가 이해하는 숫자, 문자, 이미지, 동영상과 같은 정적인 정보

2. 명령어
    * 컴퓨터를 실직적으로 작동 시키는 중요한 정보
    * 데이터 없이는 아무것도 할 수 없는 정보 덩어리
    * **"데이터를 움직이고 컴퓨터를 작동 시키는 장보"**

**"즉, 명령어는 컴퓨터를 작동시키는 정보이고, 데이터는 명령어를 위해 존재하는 일종의 재료입니다."**
* 컴퓨터 프로그램은 '명령어들의 모음'으로 정의되기도 합니다.
    * 그래서 명령어는 컴퓨터 구조를 학습하는 데 있어 데이터보다 더 중요한 개념.

## 컴퓨터의 4가지 핵심 부품.

1. 중앙처리장치(Central Programming Unit, CPU)
    * 컴퓨터의 두뇌
    * 메모리에 저장된 명령어를 읽어 들이고, 읽어 들인 명령어를 해석하고, 실행하는 부품입니다.
    * CPU 내부 구성 요소 중 가장 중요한 세 가지는 산술논리연산장치(ALU: Arithmetic Logic Unit), 레지스터(register), 제어장치(CU: Control Unit) 입니다.
        * ALU: 계산기, 계산만을 위해 존재하는 부품, 컴퓨터 내부에서 수행되는 대부분의 계산은 ALU가 도맡아 수행
        * 레지스터: CPU 내부의 작은 임시 저장 장치, 프로그램을 실행하는 데 필요한 값들을 임시로 저장, CPU 안에는 여러 개의 레지스터가 존재하고 각기 다른 이름과 역할을 가짐
        * 제어장치: 제어 신호(Control Signal)라는 전기 신호를 내보내고 명령어를 해석하는 장치.
            * 제어 신호란 컴퓨터 부품들을 관리하고 작동시키기 위한 일종의 전기 신호
                * CPU가 메모리에 저장된 값을 읽고 싶을 땐 메모리를 향해 "메모리 읽기"라는 제어 신호를 보낸다.
                * CPU가 메모리에 어떤 값을 저장하고 싶을 땐 메모리를 향해 "메모리 쓰기"라는 제어 신호를 보낸다.
2. 주기억장치(Main memory, 메모리)
    * 현재 실행되는 프로그램의 명령어와 데이터를 저장하는 부품.
    * 즉, 프로그램이 실행되려면 반드시 메모리에 저장되어 있어야 합니다.
    * 메모리에 저장된 값의 위치는 주소로 알 수 있습니다.
3. 보조기억장치(secondary storage)
    * 메모리보다 크기가 크고 전원이 꺼져도 저장된 내용을 잃지 않는 메모리를 보조할 저장 장치
    * 보조기억장치는 '보관할' 프로그램을 저장한다고 생각해도 좋다.
4. 입출력장치(input/output(I/O) device)
    * 마이크, 스피커, 프린터, 마우스, 키보드처럼 컴퓨터 외부에 연결되어 컴퓨터 내부와 정보를 교환하는 장치를 의미.
    * '컴퓨터 주변에 붙어 있는 장치'라는 의미에서 "주변장치(peripheral device)"라 통칭하기도 함.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%8F%E1%85%A5%E1%86%B7%E1%84%91%E1%85%B2%E1%84%90%E1%85%A5%E1%84%8B%E1%85%B4%E1%84%92%E1%85%A2%E1%86%A8%E1%84%89%E1%85%B5%E1%86%B7%E1%84%87%E1%85%AE%E1%84%91%E1%85%AE%E1%86%B7.png?raw=true">

**"주소"**
* 컴퓨터가 빠르게 작동하기 위해서는 메모리 속 명령어와 데이터가 정돈된 위치에 저장되어 있어야 합니다.
    * 그래서 메모리에는 저장된 값에 빠르게 효율적으로 접근하기 위해 주소(address)라는 개념이 사용됩니다.
    * 주소로 메모리 내 원하는 위치에 접근할 수 있습니다.

**메인보드와 시스템 버스**
1. 메인보드
    * 마더보드(mother board)라고도 부름
    * 메인보드에는 앞에서 소개한 부품을 비롯한 여러 컴퓨터 부품을 부착할 수 있는 슬록과 연결 단자가 있습니다.
    * 메인 보드에 연력된 부품들은 서로 정보를 주고 받을수 있습니다. 이는 메인보드 내부에 "버스(bus)"라는 통로가 있기 때문입니다.
2. 시스템 버스(system bus)
    * 여러 버스 가운데 컴퓨터의 네 가지 핵심 부품을 연결하는 가장 중요한 버스입니다.
    * 주소 버스, 데이터 버스, 제어 버스로 구성되어 있습니다.
        * 주소 버스(address bus): 주소를 주고받는 통로
        * 데이터 버스(data bus): 명령어롸 데이터를 주고 받는 통로
        * 제어 버스(control bus): 제어 신호를 주고 받는 통로

## 키워드로 정리하는 핵심 포인트
* 컴퓨터가 이해하는 정보에는 **"데이터"** 와 **"명령어"** 가 있습니다.
* **"메모리"** 는 현재 실행되는 프로그램의 명령어와 데이터를 저장하는 부품입니다.
* **"CPU"** 는 메모리에 저장된 명령어를 읽어 들이고, 해석하고, 실행하는 부품입니다.
* **"보조기억장치"** 는 전원이 꺼져도 보관할 프로그램을 저장하는 부품입니다.
* **"입출력장치"** 는 컴퓨터 외부에 연결되어 컴퓨터 내부와 정보를 교환할 수 있는 부품입니다.
* **"시스템 버스"** 는 컴퓨터의 네 가지 핵심 부품들이 서로 정보를 주고받는 통로입니다.

## Q1. "메모리 주소가 무엇이며, iOS 시스템 내에서 어떤 역할을 수행한다고 생각하나요?"

메모리 주소는 컴퓨터 메모리 내에서 데이터나 명령어의 위치를 식별하는 데 사용되는 고유한 식별자입니다. 각 바이트 또는 워드에는 메모리 내의 위치를 나타내는 고유한 주소가 있으며, 이를 통해 CPU와 다른 시스템 구성 요소가 필요한 데이터를 정확히 찾아 읽고 쓸 수 있습니다.

iOS 시스템 내에서 메모리 주소의 역할은 특히 중요합니다. iOS는 메모리 관리에 자동 참조 카운팅(ARC)를 사용하여 객체의 생명 주기를 관리합니다. ARC는 객체에 대한 참조가 더 이상 필요하지 않게 되면 자동으로 메모리를 해제합니다. 이 과정에서 메모리 주소를 사용하여 각 객체의 위치를 파악하고 관리합니다. 따라서, 개발자로서 메모리 주소의 이해는 메모리 누수를 방지하고 앱의 성능을 최적화하는 데 필수적입니다.

또한, 메모리 주소를 이해하는 것은 포인터를 사용한 프로그래밍, 메모리 접근 최적화, 그리고 다양한 메모리 관리 기법을 적용하는 데 중요합니다. 예를 들어, 효율적인 데이터 구조 설계, 대규모 데이터 처리, 멀티스레딩 환경에서의 데이터 공유와 동기화 문제 해결 등은 메모리 주소와 밀접한 관련이 있습니다.

iOS 시스템 내에서 메모리 주소의 관리와 최적화는 앱의 반응 속도, 안정성, 그리고 사용자 경험에 직접적인 영향을 미치기 때문에, 이를 정확히 이해하고 효과적으로 활용하는 능력은 iOS 개발자에게 매우 중요한 자질입니다.

## Q2. "메모리 주소가 무엇이며, Java 시스템 내에서 어떤 역할을 수행한다고 생각하나요?"

"메모리 주소는 컴퓨터 메모리 내의 특정 위치를 식별하는 데 사용되는 고유한 식별자입니다. 이 주소를 통해, 컴퓨터 시스템은 메모리 내에서 데이터나 명령어를 정확히 찾아내어 읽고 쓸 수 있습니다. 간단히 말해, 메모리 주소는 컴퓨터 메모리 내의 '우편 주소'와 유사한 역할을 수행합니다.

Java 시스템 내에서, 메모리 주소의 역할은 Java 가상 머신(JVM)에 의해 추상화되어 다루어집니다. Java 개발자들은 직접적으로 메모리 주소를 다루지 않으며, 대신 Java가 제공하는 추상화된 메모리 모델을 사용하여 프로그래밍합니다. Java에서는 객체와 배열 등이 힙 메모리에 할당되며, 개발자는 이러한 객체에 대한 참조를 통해 메모리를 접근하게 됩니다. 여기서 '참조'는 실제 메모리 주소를 직접적으로 나타내지는 않지만, 특정 객체를 가리키는 역할을 합니다.

JVM은 가비지 컬렉션(Garbage Collection)을 통해 메모리 관리를 자동화합니다. 가비지 컬렉터는 더 이상 사용되지 않는 객체를 자동으로 검출하고, 그 메모리를 회수하여 재사용 가능하게 만듭니다. 이 과정에서 JVM은 내부적으로 메모리 주소를 관리하여, 효율적인 메모리 할당과 해제를 수행합니다.

따라서, Java 시스템 내에서 메모리 주소는 주로 메모리 할당, 객체 참조, 그리고 가비지 컬렉션과 같은 메모리 관리 작업에 중요한 역할을 수행합니다. Java 개발자로서 우리의 역할은 주로 안전하고 효율적인 코드 작성에 초점을 맞추며, JVM이 메모리 관리의 세부 사항을 추상화하고 처리하도록 합니다. 이렇게 함으로써, 개발자는 메모리 관리의 복잡성으로부터 벗어나 비즈니스 로직 구현에 더 집중할 수 있습니다."
