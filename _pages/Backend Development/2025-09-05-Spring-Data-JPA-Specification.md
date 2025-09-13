---
title: "📚[Backend Development] 🏗️ Spring Data JPA Specification"
tags:
    - Backend Ddevelopment
    - Spring
    - JPA
    - Specification
    - Query
    - OOP
date: "2025-09-05"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 🏗️ Spring Data JPA Specification

Spring Data JPA Specification은 **동적 쿼리를 매우 우아하고 객체지향적으로 처리할 수 있는 강력한 도구**입니다.

---

## 🤔 Spring Data JPA Specification이란?

**JPA Specification(사양)** 이란, **여러 검색 조건을 '객체'처럼 조립하여 동적 쿼리를 생성하게 해주는 프로그래밍 인터페이스(API)** 입니다.

### ✨ 핵심 특징
- 복잡한 `if-else`문이나 문자열 쿼리 조합 없음
- **타입에 안전한(Type-safe) 코드**로 검색 조건 처리
- 필요한 검색 조건들을 **마치 레고 블록처럼 붙였다 뗐다** 가능
- 내부적으로는 JPA의 **Criteria API**를 사용하여 동작

---

## 🍕 직관적 비유: 맞춤형 피자 주문

Specification은 마치 피자 가게에서 토핑을 자유롭게 추가하고 빼는 것과 같습니다.

### ❌ 전통적인 방식 (Repository 메서드 폭발)
```java
// 모든 경우의 수를 메서드로 만들어야 함
List<Order> findByMemberName(String memberName);
List<Order> findByShippingStatus(ShippingStatus status);
List<Order> findByMemberNameAndShippingStatus(String memberName, ShippingStatus status);
List<Order> findByMemberNameAndShippingStatusAndCreatedAtBetween(...);
// 🔴 조합이 늘어날수록 메서드가 기하급수적으로 증가!
```

> **문제점**: "페퍼로니는 빼고 올리브와 버섯을 추가해주세요" 같은 복잡한 요청은 메뉴에 없으면 처리하기 힘듭니다.

### ✅ Specification 방식 (동적 조합)
```java
// 검색 조건들을 레고 블록처럼 자유롭게 조합
Specification<Order> spec = Specification
    .where(OrderSpecification.withMemberName(memberName))      // 🧩 토핑 1
    .and(OrderSpecification.withShippingStatus(status))        // 🧩 토핑 2  
    .and(OrderSpecification.betweenDates(startDate, endDate)); // 🧩 토핑 3

List<Order> orders = orderRepository.findAll(spec); // 🍕 맞춤형 피자 완성!
```

> **장점**: 손님(서비스 계층)이 토핑(Specification 객체)들을 고르면, 주방장(JPA)이 즉석에서 맞춤형 피자(동적 쿼리)를 만들어 줍니다.

---

## 🚀 베스트 프랙티스: 실무 예제 코드

가장 효과적인 Specification 사용법은 **"재사용 가능한 Specification들을 별도의 클래스로 분리하여 관리하는 것"** 입니다.

### 📁 **Step 1: Specification 클래스 생성 (Best Practice)**

검색 조건들을 정의하는 `OrderSpecification` 클래스를 새로 만듭니다.

#### `repository/specification/OrderSpecification.java` (신규 파일)

```java
package com.kobe.productmanagement.repository.specification;

import com.kobe.productmanagement.common.ShippingStatus;
import com.kobe.productmanagement.domain.Member;
import com.kobe.productmanagement.domain.Order;
import jakarta.persistence.criteria.Join;
import org.springframework.data.jpa.domain.Specification;
import org.springframework.util.StringUtils;

import java.time.LocalDate;
import java.time.LocalTime;

public class OrderSpecification {

    // 🔍 회원 이름으로 검색하는 Specification
    public static Specification<Order> withMemberName(String memberName) {
        return (root, query, criteriaBuilder) -> {
            // 🟢 null/빈 문자열 체크로 안전성 확보
            if (!StringUtils.hasText(memberName)) {
                return null; // 조건이 없으면 무시됨
            }
            Join<Order, Member> memberJoin = root.join("member");
            return criteriaBuilder.like(memberJoin.get("memberName"), "%" + memberName + "%");
        };
    }

    // 📦 배송 상태로 검색하는 Specification  
    public static Specification<Order> withShippingStatus(ShippingStatus status) {
        return (root, query, criteriaBuilder) -> {
            if (status == null) {
                return null;
            }
            return criteriaBuilder.equal(root.get("shippingStatus"), status);
        };
    }

    // 📅 주문 날짜 범위로 검색하는 Specification
    public static Specification<Order> betweenDates(LocalDate startDate, LocalDate endDate) {
        return (root, query, criteriaBuilder) -> {
            if (startDate == null || endDate == null) {
                return null;
            }
            return criteriaBuilder.between(
                root.get("createdAt"), 
                startDate.atStartOfDay(), 
                endDate.atTime(LocalTime.MAX)
            );
        };
    }
}
```

### 🔧 **Step 2: Service에서 Specification 조합하여 사용**

이제 `OrderServiceImpl`에서는 새로 만든 `OrderSpecification`의 메서드들을 조합하여 훨씬 더 깔끔하게 쿼리를 실행할 수 있습니다.

#### `service/OrderServiceImpl.java` (리팩토링)

```java
package com.kobe.productmanagement.service;

import com.kobe.productmanagement.domain.Order;
import com.kobe.productmanagement.dto.request.OrderSearchRequest;
import com.kobe.productmanagement.dto.request.OrderStatusUpdateRequest;
import com.kobe.productmanagement.dto.response.OrderResponse;
import com.kobe.productmanagement.repository.OrderRepository;
import com.kobe.productmanagement.repository.specification.OrderSpecification; // 🟢 신규 import
import lombok.RequiredArgsConstructor;
import org.springframework.data.jpa.domain.Specification;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;
import java.util.stream.Collectors;

@Service
@RequiredArgsConstructor
@Transactional
public class OrderServiceImpl implements OrderService {

    private final OrderRepository orderRepository;

    @Override
    @Transactional(readOnly = true)
    public List<OrderResponse> getAllOrders(OrderSearchRequest searchRequest) {
        // 🧩 Specification들을 레고 블록처럼 체인 형태로 조합
        Specification<Order> spec = Specification
            .where(OrderSpecification.withMemberName(searchRequest.getMemberName()))
            .and(OrderSpecification.withShippingStatus(searchRequest.getShippingStatus()))
            .and(OrderSpecification.betweenDates(searchRequest.getStartDate(), searchRequest.getEndDate()));

        return orderRepository.findAll(spec).stream()
                .map(OrderResponse::from)
                .collect(Collectors.toList());
    }

    @Override
    public OrderResponse updateOrderState(String orderId, OrderStatusUpdateRequest request) {
        Order order = orderRepository.findById(orderId)
                .orElseThrow(() -> new IllegalArgumentException("해당 주문을 찾을 수 없습니다 ID: " + orderId));

        order.updateDetails(request.getStatus());
        return OrderResponse.from(order);
    }

    // 🗑️ 기존의 복잡한 private searchOrders 메서드는 이제 삭제!
}
```

---

## 🎯 이 방식의 핵심 장점

### 1. 🔄 **재사용성 (Reusability)**
```java
// 다른 서비스에서도 동일한 Specification 재사용 가능
public class AdminService {
    public List<Order> getVipOrders() {
        return orderRepository.findAll(
            OrderSpecification.withShippingStatus(ShippingStatus.DELIVERED)
                .and(OrderSpecification.withMemberName("VIP"))
        );
    }
}
```

### 2. 📖 **가독성 (Readability)**
```java
// ❌ 기존: 복잡한 쿼리 생성 로직이 서비스에 섞임
if (memberName != null) {
    // 복잡한 Criteria API 코드...
}
if (status != null) {
    // 또 다른 복잡한 코드...
}

// ✅ 개선: 선언적이고 명확한 의도 표현
Specification<Order> spec = Specification
    .where(OrderSpecification.withMemberName(memberName))
    .and(OrderSpecification.withShippingStatus(status));
```

### 3. 🧩 **조합의 유연성 (Flexibility)**
```java
// 🎪 복잡한 조건도 자유자재로 조합 가능
Specification<Order> complexSpec = Specification
    .where(OrderSpecification.withMemberName("김철수"))
    .and(OrderSpecification.withShippingStatus(ShippingStatus.SHIPPED))
    .or(OrderSpecification.betweenDates(startDate, endDate))
    .and(Specification.not(OrderSpecification.withShippingStatus(ShippingStatus.CANCELLED)));
```

---

## 📊 전통적 방식 vs Specification 비교

| 구분 | 전통적 방식 | Specification 방식 |
|------|------------|-------------------|
| **메서드 수** | ❌ 조합별로 메서드 폭발 | ✅ 재사용 가능한 조각들 |
| **유지보수** | ❌ 조건 추가 시 메서드 증가 | ✅ Specification만 추가 |
| **가독성** | ❌ 복잡한 쿼리 생성 로직 | ✅ 선언적이고 명확한 의도 |
| **타입 안정성** | ❌ 문자열 기반 쿼리 위험 | ✅ 컴파일 타임 체크 |
| **테스트** | ❌ 각 메서드별 개별 테스트 | ✅ Specification 단위 테스트 |

---

## 💡 실무 팁

### 🎨 **네이밍 컨벤션**
```java
// ✅ 좋은 네이밍: 의도가 명확함
OrderSpecification.withMemberName()
OrderSpecification.betweenDates()
OrderSpecification.hasShippingStatus()

// ❌ 나쁜 네이밍: 의도가 불분명
OrderSpecification.memberName()
OrderSpecification.dates()
OrderSpecification.status()
```

### 🔍 **null 체크는 필수**
```java
// 🟢 항상 null 체크를 통해 안전성 확보
public static Specification<Order> withMemberName(String memberName) {
    return (root, query, criteriaBuilder) -> {
        if (!StringUtils.hasText(memberName)) {
            return null; // null 반환 시 해당 조건은 무시됨
        }
        // 실제 조건 로직...
    };
}
```

### 🎪 **복잡한 조건 조합 예시**
```java
// 실무에서 자주 사용되는 복잡한 검색 조건
Specification<Order> spec = Specification
    .where(OrderSpecification.withMemberName(searchRequest.getMemberName()))
    .and(OrderSpecification.withShippingStatus(searchRequest.getStatus()))
    .and(OrderSpecification.betweenDates(searchRequest.getStartDate(), searchRequest.getEndDate()))
    .and(OrderSpecification.withMinAmount(searchRequest.getMinAmount()))
    .or(OrderSpecification.isVipOrder());
```

---

## 🏆 결론

Spring Data JPA Specification은 **복잡한 동적 쿼리를 우아하고 객체지향적으로 처리할 수 있는 최고의 솔루션** 중 하나입니다.

### 🎯 핵심 가치
- **🧩 레고 블록식 조합**: 필요한 검색 조건들을 자유자재로 조립
- **🛡️ 타입 안정성**: 컴파일 타임에 오류 발견 가능  
- **📖 선언적 코드**: 비즈니스 의도가 명확하게 드러나는 코드
- **🔄 높은 재사용성**: 한 번 만든 Specification을 여러 곳에서 활용

복잡한 검색 기능이 필요한 실무 프로젝트에서는 **반드시 고려해볼 만한 강력한 도구**입니다!
