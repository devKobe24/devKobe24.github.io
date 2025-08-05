---
title: "📚[Backend Development] Java Spring 프레임워크 프로젝트 구현 시 계층 구현 순서와 이유."
tags:
    - Backend Ddevelopment
    - Component
    - Layer
    - Architecture
date: "2025-08-05"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 📚[Backend Development] Java Spring 프레임워크 프로젝트 구현 시 계층 구현 순서와 이유.

각 계층의 역할과 의존성을 고려했을 때, 프로젝트를 구현하는 데 권장되는 안정적인 순서가 있습니다.

## ✅ 1. Java Spring 프레임워크 프로젝트를 구현 시 어떤 계층 순서로 구현해야 하는가?

가장 안정적이고 추천되는 순서는 **'안에서 밖으로(Inside-Out)' 구현하는 방식**입니다.
즉, 데이터의 가장 핵심적인 부분부터 만들어서 바깥으로 확장해 나가는 순서입니다.

**✌️ 추천 순서: Domain Layer(도메인 계층) ➞ Data Access Layer(데이터 접근 계층) ➞ Business Layer(비즈니스 계층) ➞ Presentation Layer(표현 계층)**

1. **Domain Layer(도메인 계층)**
    - `Product`, `Stock`등 `@Entity` 클래스와 ERD 설계를 먼저 완성합니다. 이것이 모든 데이터의 뼈대가 됩니다.
2. **Data Access Layer(데이터 접근 계층)**
    - `ProductRepository` 등 `@Repository` 인터페이스를 만들어, 데이터베이스에 실제로 데이터를 CRUD하는 방법을 정의합니다.
3. **Business Layer(비즈니스 계층)**
    - `ProductService` 등 `@Service` 클래스를 만들어, `Repository`를 활용한 비즈니스 로직, 트랜잭션 처리, 계산 로직 등을 구현합니다.
4. **Presentation Layer(표현 계층)**
    - `@RestController`를 만들어, 외부의 요청을 받고 `Service`를 호출하여 그 결과를 반환하는 API 엔드포인트를 완성합니다.

## ✅ 2. 왜 '안에서 밖으로(Inside-Out)' 방식으로 계층을 구현해야 할까?

이 순서는 **'건물을 짓는 순서'** 에 비유할 수 있습니다.
외벽과 인테리어부터 할 수 없듯이, 소프트웨어도 뼈대와 기반부터 쌓아 올리는 것이 가장 안정적입니다.

- **1단계: 설계도와 뼈대(`Domain Layer` + `Data Access Layer`)**
    - 집을 짓기 전 설계도(ERD)를 그리고, 땅을 파고 철골(Entity, Repository)을 세우는 것과 같습니다. 이 기반이 튼튼해야만 그 위에 무엇이든 안전하게 올릴 수 있습니다.
- **2단계: 내부 설비(`Business Layer`)**
    - 뼈대가 완성된 후, 전기/배수/가스 설비(비즈니스 로직, 트랜잭션)를 설치합니다. 이 설비들은 뼈대 구조에 맞춰서 만들어집니다.
- **3단계: 외벽과 인테리어(`Presentation Layer`)**
    - 모든 내부 구조가 완성된 후, 사람들이 보고 사용할 수 있도록 외벽을 꾸미고 문과 창문(API 엔드포인트)을 답니다.

이 순서를 따랐을 때의 기술적인 장점은 다음과 같습니다.

- **의존성 순방향 개발**
    - Spring의 계층은 `Presentation ➞ Business ➞ Data Access` 순서로 의존합니다.
    - 의존되는 대상(안쪽 계층)을 먼저 만들어야, 이를 사용하는 바깥 계층을 안정적으로 구현할 수 있습니다.
- **탄탄한 기반 위에서의 개발**
    - 핵심 데이터 구조와 로직이 이미 완성되고 테스트된 상태에서 UI/API를 개발하므로, 나중에 구조를 뒤엎는 큰 변경이 발생할 확률이 줄어듭니다.
- **계층별 단위 테스트 용이**
    - 안쪽 계층부터 만들면, 각 계층이 완성될 때마다 독립적으로 단위 테스트를 수행하기 매우 편리하여 코드의 안정성을 높일 수 있습니다.
