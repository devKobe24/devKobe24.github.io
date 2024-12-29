---
title: "💾 [CS] 1Day 1CS - 컴퓨터 구조의 큰 그림"
tags:
    - CS
date: "2024-12-19"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 컴퓨터 구조의 큰 그림.
<img src = "https://github.com/devKobe24/images2/blob/main/CS_IMG/Computer_Architecture_Map.png?raw=true">

## 1️⃣ 컴퓨터가 이해하는 정보.
- 프로그램을 개발하기 위해서는 프로그래밍 언어로 소스 코드를 작성해야 합니다.
    - **컴퓨터는 Java, C++, Python과 같은 프로그래밍 언어를 직접 이해하지 못합니다.**

> 여기서 잠깐 ☝️
> "소스 코드(Source Code)란?" 🤔
> 
> 소프트웨어 프로그램의 동작과 기능을 정의하기 위해 프로그래밍 언어로 작성된 텍스트를 말합니다.
> 즉, 프로그래머가 작성하는 코드로, 컴퓨터가 실행할 수 있는 형태로 변환되기 전의 원본 코드입니다.

- 컴퓨터가 이해할 수 있는 정보
    - **데이터와 명령어**
        - **데이터** 
            - 숫자, 문자, 이미지, 동영상과 같은 **정적인 정보**를 의미합니다.
            - 컴퓨터와 주고받는 정보나 컴퓨터에 저장된 정보 자체를 데이터라고 통칭하기도 합니다.
            - 데이터는 있는 그대로의 정보를 말합니다.
            - 명령어에 **종속적인 정보** 
            - 명령어의 **명령의 대상** 
            - **명령어의 재료**
        - **명령어**
            - 데이터를 활용하는 정보.
- 소스 코드는 내부적으로 컴퓨터가 이해할 수 있는 데이터와 명령어의 형태로 변환된 뒤에 실행됩니다.
- 명령어는 **수행할 동작**과 **수행할 대상**으로 이루어져 있습니다.
    - 명령어
        - 수행할 동작
        - 수행할 대상
- 소스 코드 예시:
```
더하라. 1과 2를
출력하라. "hello world"를
USB 메모리에 저장하라. cat.jpg를
```
- **데이터 :** 1, 2라는 숫자, "hello, world"라는 문자열, 'cat.jpg'라는 파일명.
- **명령어 :** "더하라", "출력하라", "USB 메모리에 저장하라"
- 컴퓨터는 기본적으로 0과 1만 이해할 수 있습니다.
    - 데이터와 명령어 또한 0과 1로 이루어져 있습니다.
        - 즉, 컴퓨터는 0과 1만으로 다양한 숫자(정수, 실수)와 문자 데이터를 표현하며, 이 데이터를 활용해 명령어를 실행합니다.
- 명령어를 실행하는 주체 : CPU
    - **CPU**
        - 컴퓨터의 핵심 부품 중 하나.
        - 명령어를 이해하고 실행하는 주체.
            - **CPU의 종류에 따라 실행 가능한 세부적인 명령어의 종류와 처리의 양상이 달라질 수 있음을 의미.**
                - 큰 틀에서 보면 CPU의 종류와 무관하게 공통적으로 활용되는 명령어는 어느 정도 정해져 있음.
- **명령어 사이클**
    - CPU가 명령어를 처리하는 순서.

<img src = "https://github.com/devKobe24/images2/blob/main/CS_IMG/information_that_computers_understand.png?raw=true">

## 2️⃣ 컴퓨터의 핵심 부품.
- 컴퓨터를 작동시키는 컴퓨터의 핵심 부품.
    - CPU(중앙처리장치), 메모리(주기억장치), 캐시 메모리, 보조기억장치, 입출력장치

<img src = "https://github.com/devKobe24/images2/blob/main/CS_IMG/key_components_of_a_computer.png?raw=true">

### 1️⃣ CPU(Central Processing Unit)
- 컴퓨터가 이해하는 정보에는 크게 데이터와 명령어가 있다고 했습니다.
    - 이러한 정보를 **읽어 들이고, 해석하고, 실행하는 부품**이 바로 **CPU(Central Processing Unit)입니다.**
- CPU의 내부
    - **산술논리연산장치(ALU, Arithmetic and Logic Unit)**
        - 사칙 연산, 논리 연산과 같은 **연산을 수행할 회로로 구성되어 있는 일종의 계산기.**
        - CPU가 처리할 명령어를 실질적으로 연산하는 요소.
    - **제어장치(CU, Control Unit)**
        - 명령어를 해석해 제어 신호라는 전기 신호를 내보내는 장치.
            - **제어 신호(control signal)란 부품을 작동시키기 위한 신호를 말합니다.**
                - CPU가 메모리를 향해 제어 신호를 보내면 메모리를 작동시킬 수 있고, 입출력장치를 향해 제어 신호를 보내면 입출력장치를 작동시킬 수 있습니다.
    - **레지스터(register)**
        - CPU 내부의 작은 임시 저장장치.
        - 데이터와 명령어를 처리하는 과정의 중간값을 저장합니다.
        - CPU 내에는 여러 개의 레지스터가 존재하며, **각기 다른 이름과 역할**을 가지고 있습니다.
- 이 중 가장 중요한 구성 요소는 **레지스터**
    - CPU가 처리하는 명령어는 반드시 레지스터에 저장되기 때문에 레지스터 값만 잘 관찰해도 프로그램이 어떻게 실행되는지 가장 낮은 단계에서 파악할 수 있습니다.

### 2️⃣ 메모리와 캐시 메모리.
- **메인 메모리(main memory)** 역할을 하는 하드웨어는 RAM과 ROM이 있습니다.
    - 일반적으로 '(메인) 메모리'라는 용어는 **RAM**을 지칭합니다.
- **메모리**
    - CPU가 읽어 들이고, 해석하고, 실행하는 모든 정보는 어딘가에 저장되어 있어야 하며, 이 정보를 저장하는 장치.
        - 즉, 메모리는 **현재 실행 중인 프로그램을 구성하는 데이터와 명령어를 저장하는 부품**
            - 여기서 중요한 것은 **'실행 중인'** 프로그램을 저장한다는 것.
- 프로그램이 실행되려면 그 프로그램을 이루는 데이터와 명령어가 메모리에 저장되어 있어야 합니다.
- 메모리(RAM)와 관련해 기억해야 할 중요한 배경 지식
    - 1. **주소(Address)**
    - 2. **휘발성(volatile)**
- 1. **주소(Address)**
    - CPU가 메모리에 접근할 때 컴퓨터가 빠르게 작동하기 위해 메모리속 데이터와 명령어가 정돈되어 있어야 합니다.
        - 그래서 사용되는 개념이 **주소(Address)** 입니다.
    - CPU가 원하는 정보로 접근하기 위해서는 주소가 필요합니다.
- 아래 그림은 1번지와 2번지에는 명령어, 3번지와 4번지에는 데이터가 저장되어 있고, 5번지와 6번지에는 아무것도 저장되어 있지 않은 상태의 메모리를 표현한 그림입니다.

<img src = "https://github.com/devKobe24/images2/blob/main/CS_IMG/Memory_status_photo.png?raw=true">

- 2. **휘발성(volatile)**
    - 전원이 공급되지 않을 때 저장하고 있는 정보가 지워지는 특성을 의미합니다.
    - 메모리(RAM)는 휘발성 저장장치로, 메모리에 저장된 정보는 컴퓨터의 전원이 꺼지면 모두 삭제됩니다.
        - 이는 메모리를 이해하기 위한 중요한 특성이므로 기억.
- 메모리를 학습할 때 반드시 함께 알아 두어야 할 저장장치 : **캐시 메모리(Cache Memory)**
    - **캐시 메모리(Cache Memory)**
        - CPU와 메모리 사이에는 반드시 하나 이상의 캐시 메모리가 있습니다.
        - CPU가 조금이라도 더 빨리 메모리에 저장된 값에 접근하기 위해 사용하는 저장장치입니다.
            - 빠른 메모리 접근을 보조하는 저장장치인 셈입니다.
        - 캐시 메모리는 CPU 안에 위치하기도 하고, CPU 밖에 위치하기도 하며, 여려 종류가 있습니다.

### 3️⃣ 보조기억장치.
- **보조기억장치(secondary storage)**
    - 컴퓨터의 전원이 꺼지면 저장된 정보를 모두 잃습니다. 이를 보조하기 위해 사용하는 장치입니다.
        - 즉, 전원이 꺼져도 저장된 정보가 사라지지 않는 **비휘발성(non-volatile)** 저장장치입니다.
            - CD-ROM, DVD, 하드 디스크 드라이브, 플래시 메모리(SSD, USB 메모리), 플로피 디스크와 같은 저장장치가 보조 기억장치의 일종
- 보조기억 장치는 **보관할 프로그램을 저장합니다.**
    - 메모리가 **현재 실행 중인 프로그램을 저장하는 것과 대비됩니다.**
- 유의점 : CPU가 보조기억장치에 저장된 프로그램을 곧장 가져와 실행할 수 없다는 점
    - 어떠한 프로그램을 실행시 보조기억장치에서 보관하고 있던 프로그램을 메모리로 복사해야 합니다.

### 4️⃣ 입출력장치.
- **입출력장치(Input/Output device)**
    - 컴퓨터 외부에 연결되어 컴퓨터 내부와 정보를 교환하는 장치를 말합니다.
        - 입력장치 : 컴퓨터에 어떠한 입력을 할 때 사용하는 장치
            - 예 : 마우스, 키보드, 마이크
        - 출력장치 : 컴퓨터의 정보를 받기 위해 사용하는 장치
            - 예 : 스피커, 모니터, 프린터
- 참고: 보조기억장치와 입출력장치는 완전히 배타적인 개념이 아닙니다.
    - 보조기억장치는 결국 메모리를 보조하는 임무를 수행하는 특별한 입출력장치로 볼 수 있습니다.
- **주변장치(peripheral device) :** 보조기억장치가 컴퓨터 내부와 정보를 주고받는 방식이 입출력장치와 크게 다르지 않기 때문에, 보조기억장치와 입출력장치를 주변장치라고 통칭하기도 합니다.

### 5️⃣ 메인 보드와 버스
- **메인 보드(main board) 혹은 마더 보드(mother board)**
    - 앞에서 살펴본 컴퓨터의 핵심 부품들은 공중에 떠 있지 않습니다.
        - 이 부품들을 고정하고 연결하는 기판에 연결되어 있습니다.
            - 이 기판을 메인보드 혹은 마더보드라고 부릅니다.
                - 메인 보드에는 컴퓨터의 핵심 부품을 비롯한 여러 부품들을 연결할 수 있는 슬롯과 연결 단자들이 있습니다.
- **버스(Bus)**
    - 메인 보드에 연결된 부품들은 각자의 역할을 적절히 수행하기 위해 서로 정보를 주고받습니다.
        - 이때 각 컴퓨터 부품들이 정보를 주고받는 통로
    - 버스의 종류는 다양하지만, 앞서 설명한 핵심 부품들을 연결하는 **시스템 버스(System Bus)가 가장 중요하다고 할 수 있습니다**
<img src = "https://github.com/devKobe24/images2/blob/main/CS_IMG/main_board.png?raw=true">

## 3️⃣ 컴퓨터 구조 지도 그리기

<img src = "https://github.com/devKobe24/images2/blob/main/CS_IMG/computer_architecture_map_2.png?raw=true">
- 한눈에 정리된 위의 컴퓨터 구조 지도를 보면서 미리 학습의 흐름을 정리해봅시다.
