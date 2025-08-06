---
title: "📚[Backend Development] Domain Layer, JPA Entity 생성시 반드시 지켜야 할 중요 원칙들"
tags:
    - Backend Ddevelopment
    - Layer
    - Architecture
    - Domain Layer
    - JPA
    - Spring
    - Framework
date: "2025-08-06"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 📚[Backend Development] Domain Layer, JPA Entity 생성시 반드시 지켜야 할 중요 원칙들
Java Spring 프레임워크로 프로젝트를 진행할 때 **Domain Layer, 특히 JPA Entity를 생성할 때 반드시 지켜야 할 중요한 원칙**들이 있습니다.

이 원칙들은 코드의 안정성, 유지보수성, 그리고 예상치 못한 버그를 방지하기 위해 꼭 필요합니다.

## ✅ 1. 기본 생성자(No-Arg Constructor)를 반드시 제공하세요.

JPA는 데이터베이스에서 조회한 데이터로 객체를 생성할 때, 먼저 빈 객체를 만든 후 각 필드에 값을 채워 넣습니다.
**이때 빈 객체를 만들기 위해 파라미터가 없는 기본 생성자가 반드시 필요합니다.**

- **방법 :** Lombok의 `@NoArgsConstructor(access = AccessLevel.PROTECTED)`를 사용하는 것이 가장 좋습니다.
- **이유 :** `protexted`로 접근을 제한하면, 개발자가 비즈니스 로직에서 `new Product()` 처럼 불완전한 객체를 실수로 생성하는 것을 막아 안정성을 높일 수 있습니다.

## ✅ 2. Setter 사용을 지양하고, 불변성(Immutability)을 추구하세요.

Entity 객체에 무분별한 `setter` 메서드를 열어두면, 애플리케이션의 여러 곳에서 객체의 상태가 의도치 않게 변경될 수 있어 데이터의 일관성을 해치고 버그를 유발하기 쉽습니다.

- **방법 :**
    1. 객체 생성은 `@Builder`를 통해 명확하게 합니다.
    2. `setter`를 만드는 대신, 상태를 변경해야 할 때는 그 **의도가 명확히 드러나는 비즈니스 메서드**를 만드세요. (예: `product.changePrice(newPrice`)

## ✅ 3. 모든 필드를 포함하는 `equals()`와 `hashCode()`를 피하세요.

JPA Entity에 일반적인 Lombok의 `@EqualsAndHashCode`를 사용하면, 연관관계 필드로 인해 예기치 않은 문제가 발생할 수 있습니다.
두 엔티티가 서로를 참조하는 경우 무한 루프에 빠질 수 있습니다.

- **방법 :** 객체의 고유성을 보장하는 **PK(`@Id`) 필드**만을 비교하도록 구현하는 것이 가장 안전합니다.
- **Lombok 사용 시 :** `@EqualsAndHashCode(of = "productId")` 와 같이 `of` 속성을 사용하여 PK 필드만 명시적으로 지정해 주세요.

## ✅ 4. `toString()` 사용에 주의하세요.

`@ToString` 어노테이션을 무심코 사용하면, 연관된 모든 엔티티를 조회하려는 쿼리가 발생하여 성능 저하를 일으키거나, 양방향 연관관계에서 무한 루프와 `StackOverflowError`를 유발할 수 있습니다.

- **방법 :** 연관관계를 맺고 있는 필드는 `@ToString.Exclude`를 사용하여 `toString()` 결과에서 제외하는 것이 안전합니다.

## ✅ 5. Entity를 API 요청(Request)/응답(Response)에 직접 사용하지 마세요.

Entity는 데이터베이스 테이블과 직접 연결된, 시스템의 가장 핵심적인 데이터 모델입니다.
이를 외부에 그대로 노출하면 보안에 취약하고, 내부 로직의 변경이 API 명세에 직접적인 영향을 주게 되어 유연성이 떨어집니다.

- **방법 :** 반드시 **DTO(Data Transfer Object)** 를 사용하여, API의 요청과 응답 데이터를 Entity와 분리하세요. 이는 지금까지 우리가 API 명세서를 설계하며 지켜온 가장 중요한 원칙입니다.
