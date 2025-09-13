---
title: "📚[Backend Development] 🏗️ JpaSpecificationExecutor"
tags:
    - Backend Ddevelopment
    - Spring
    - Dynamic
    - Query
    - JpaSpecificationExecutor
    
date: "2025-09-06"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 🎯 JpaSpecificationExecutor

`JpaSpecificationExecutor`는 **동적 쿼리를 다루는 데 있어 매우 중요한 개념**입니다.

---

## 🤔 JpaSpecificationExecutor를 추가로 상속받는 의미는?

`OrderRepository`가 `JpaSpecificationExecutor`를 추가로 상속받는 것은, 기본 리포지토리(`JpaRepository`)에 **"동적 쿼리 조립 능력"이라는 강력한 추가 기능을 장착**하는 것을 의미합니다.

### 🏪 식당 비유로 이해하기

#### 🍽️ **JpaRepository = 기본 메뉴판**
```java
public interface OrderRepository extends JpaRepository<Order, String> {
    // 🍕 정해진 메뉴들
    List<Order> findAll();           // "모든 주문 조회"
    Optional<Order> findById(String id);  // "ID로 주문 찾기"
    Order save(Order order);         // "주문 저장"
    void deleteById(String id);      // "주문 삭제"
}
```

> **특징**: `findById`, `findAll`, `save` 등 **정해진 규칙의 단순한 쿼리**만 실행 가능

#### 🎨 **JpaSpecificationExecutor = 주방장 특선 주문**
```java
public interface OrderRepository extends JpaRepository<Order, String>, 
                                       JpaSpecificationExecutor<Order> {
    // ✨ 이제 추가로 사용 가능한 "맞춤형 주문" 기능들
    List<Order> findAll(Specification<Order> spec);
    Page<Order> findAll(Specification<Order> spec, Pageable pageable);
    Optional<Order> findOne(Specification<Order> spec);
    long count(Specification<Order> spec);
}
```

> **특징**: **정해지지 않은 여러 조건들을 조합하여 맞춤형 쿼리** 실행 가능
> - 예: "회원 이름이 '홍'으로 시작하고, 주문 상태가 '배송중'이며, 지난주에 주문한 내역"

### 🔄 변화 요약

| 구분 | JpaRepository만 상속 | + JpaSpecificationExecutor 상속 |
|------|---------------------|------------------------------|
| **쿼리 타입** | ❌ 정적 쿼리만 가능 | ✅ 동적 쿼리 조립 가능 |
| **조건 조합** | ❌ 미리 정의된 메서드만 | ✅ 런타임에 자유로운 조합 |
| **유연성** | ❌ 제한적 | ✅ 무한한 확장성 |

---

## 🛠️ 상속으로 인한 구체적 변경사항

가장 큰 변경점은 `OrderRepository`를 사용하는 서비스 계층에서 **새로운 메서드들을 사용할 수 있게 된다**는 것입니다.

### 📋 추가되는 핵심 메서드들

`JpaSpecificationExecutor` 인터페이스가 `OrderRepository`에 추가해주는 대표적인 메서드들:

```java
// 🔍 조건에 맞는 모든 결과 조회
List<Order> findAll(Specification<Order> spec);

// 📄 조건에 맞는 결과를 페이징하여 조회  
Page<Order> findAll(Specification<Order> spec, Pageable pageable);

// 🎯 조건에 맞는 첫 번째 결과 조회
Optional<Order> findOne(Specification<Order> spec);

// 📊 조건에 맞는 결과 개수 조회
long count(Specification<Order> spec);

// ✅ 조건에 맞는 결과가 존재하는지 확인
boolean exists(Specification<Order> spec);
```

### 🎪 **핵심 특징**: `Specification<Order>` 파라미터

모든 메서드가 **`Specification<Order>` 타입의 객체를 파라미터로 받는다**는 점이 가장 중요합니다.

```java
// ❌ 이전: 정해진 메서드만 호출 가능
List<Order> orders = orderRepository.findAll();

// ✅ 이후: 동적 조건을 담은 Specification으로 자유로운 쿼리 실행
Specification<Order> spec = OrderSpecification.withMemberName("홍길동")
    .and(OrderSpecification.withShippingStatus(SHIPPED));
List<Order> orders = orderRepository.findAll(spec);
```

---

## 🚀 베스트 프랙티스: 실무 예제 코드

**핵심 원칙**: "재사용 가능한 검색 조건들을 별도의 Specification 클래스로 분리하여 관리하기"

이렇게 하면 서비스 로직은 검색 조건이 어떻게 만들어지는지에 대한 복잡한 내용과 분리되어 훨씬 깔끔해집니다.

### 🔧 **Step 1: Specification 클래스 생성 (Best Practice)**

검색 조건들을 정의하는 `OrderSpecification` 클래스를 새로 만듭니다. 각 조건은 재사용 가능하도록 `static` 메서드로 정의합니다.

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
                return null; // 조건 값이 없으면 무시
            }
            Join<Order, Member> memberJoin = root.join("member");
            return criteriaBuilder.like(memberJoin.get("memberName"), "%" + memberName + "%");
        };
    }

    // 📦 배송 상태로 검색하는 Specification
    public static Specification<Order> withShippingStatus(ShippingStatus status) {
        return (root, query, criteriaBuilder) -> {
            if (status == null) return null;
            return criteriaBuilder.equal(root.get("shippingStatus"), status);
        };
    }

    // 📅 주문 날짜 범위로 검색하는 Specification
    public static Specification<Order> betweenDates(LocalDate startDate, LocalDate endDate) {
        return (root, query, criteriaBuilder) -> {
            if (startDate == null || endDate == null) return null;
            return criteriaBuilder.between(
                root.get("createdAt"), 
                startDate.atStartOfDay(), 
                endDate.atTime(LocalTime.MAX)
            );
        };
    }

    // 💰 최소 주문 금액으로 검색하는 Specification
    public static Specification<Order> withMinAmount(Integer minAmount) {
        return (root, query, criteriaBuilder) -> {
            if (minAmount == null) return null;
            return criteriaBuilder.greaterThanOrEqualTo(root.get("totalAmount"), minAmount);
        };
    }

    // ⭐ VIP 회원 주문 검색하는 Specification  
    public static Specification<Order> isVipOrder() {
        return (root, query, criteriaBuilder) -> {
            Join<Order, Member> memberJoin = root.join("member");
            return criteriaBuilder.equal(memberJoin.get("memberType"), "VIP");
        };
    }
}
```

### 🎨 **Step 2: Service에서 Specification 조합하여 사용**

이제 `OrderServiceImpl`에서는 `private` 메서드로 검색 로직을 직접 구현하는 대신, 새로 만든 `OrderSpecification`의 메서드들을 **레고 블록처럼 조합**하여 사용합니다.

#### `service/OrderServiceImpl.java` (리팩토링 후)

```java
package com.kobe.productmanagement.service;

import com.kobe.productmanagement.domain.Order;
import com.kobe.productmanagement.dto.request.OrderSearchRequest;
import com.kobe.productmanagement.dto.request.OrderStatusUpdateRequest;
import com.kobe.productmanagement.dto.response.OrderResponse;
import com.kobe.productmanagement.repository.OrderRepository;
import com.kobe.productmanagement.repository.specification.OrderSpecification; // 🟢 신규 import
import lombok.RequiredArgsConstructor;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
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
            .and(OrderSpecification.betweenDates(searchRequest.getStartDate(), searchRequest.getEndDate()))
            .and(OrderSpecification.withMinAmount(searchRequest.getMinAmount()));

        // ✨ JpaSpecificationExecutor가 제공하는 findAll(spec) 메서드 사용
        return orderRepository.findAll(spec).stream()
                .map(OrderResponse::from)
                .collect(Collectors.toList());
    }

    // 🆕 페이징 처리가 포함된 검색 메서드 추가
    @Override
    @Transactional(readOnly = true)
    public Page<OrderResponse> getOrdersWithPaging(OrderSearchRequest searchRequest, Pageable pageable) {
        Specification<Order> spec = Specification
            .where(OrderSpecification.withMemberName(searchRequest.getMemberName()))
            .and(OrderSpecification.withShippingStatus(searchRequest.getShippingStatus()))
            .and(OrderSpecification.betweenDates(searchRequest.getStartDate(), searchRequest.getEndDate()));

        // ✨ 페이징까지 지원하는 findAll(spec, pageable) 메서드 사용
        return orderRepository.findAll(spec, pageable)
                .map(OrderResponse::from);
    }

    // 🆕 복잡한 조건의 검색 예시
    @Override
    @Transactional(readOnly = true) 
    public List<OrderResponse> getVipOrders(String memberName) {
        Specification<Order> spec = Specification
            .where(OrderSpecification.withMemberName(memberName))
            .and(OrderSpecification.isVipOrder())
            .or(OrderSpecification.withMinAmount(100000)); // 10만원 이상 주문

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

## 📊 리팩토링 전후 비교

### ❌ **리팩토링 전: 복잡한 서비스 로직**

```java
@Service
public class OrderServiceImpl {
    
    // 🔴 서비스에 복잡한 쿼리 생성 로직이 섞여 있음
    private List<Order> searchOrders(OrderSearchRequest request) {
        CriteriaBuilder cb = entityManager.getCriteriaBuilder();
        CriteriaQuery<Order> query = cb.createQuery(Order.class);
        Root<Order> root = query.from(Order.class);
        
        List<Predicate> predicates = new ArrayList<>();
        
        if (StringUtils.hasText(request.getMemberName())) {
            Join<Order, Member> memberJoin = root.join("member");
            predicates.add(cb.like(memberJoin.get("memberName"), "%" + request.getMemberName() + "%"));
        }
        
        if (request.getShippingStatus() != null) {
            predicates.add(cb.equal(root.get("shippingStatus"), request.getShippingStatus()));
        }
        
        // ... 더 복잡한 로직들
        
        query.where(predicates.toArray(new Predicate[0]));
        return entityManager.createQuery(query).getResultList();
    }
}
```

### ✅ **리팩토링 후: 깔끔한 선언적 코드**

```java
@Service  
public class OrderServiceImpl {
    
    // 🟢 선언적이고 명확한 의도 표현
    public List<OrderResponse> getAllOrders(OrderSearchRequest searchRequest) {
        Specification<Order> spec = Specification
            .where(OrderSpecification.withMemberName(searchRequest.getMemberName()))
            .and(OrderSpecification.withShippingStatus(searchRequest.getShippingStatus()))
            .and(OrderSpecification.betweenDates(searchRequest.getStartDate(), searchRequest.getEndDate()));

        return orderRepository.findAll(spec).stream()
                .map(OrderResponse::from)
                .collect(Collectors.toList());
    }
}
```

---

## 💡 실무 활용 팁

### 🎯 **복잡한 조건 조합 예시**

```java
// 🎪 실무에서 자주 사용되는 복잡한 검색 조건
public List<OrderResponse> getComplexOrderSearch(OrderSearchRequest request) {
    Specification<Order> spec = Specification
        .where(OrderSpecification.withMemberName(request.getMemberName()))
        .and(OrderSpecification.withShippingStatus(request.getStatus()))
        .and(OrderSpecification.betweenDates(request.getStartDate(), request.getEndDate()))
        .and(OrderSpecification.withMinAmount(request.getMinAmount()))
        .or(OrderSpecification.isVipOrder())
        .and(Specification.not(OrderSpecification.withShippingStatus(ShippingStatus.CANCELLED)));
        
    return orderRepository.findAll(spec).stream()
            .map(OrderResponse::from)
            .collect(Collectors.toList());
}
```

### 🔧 **성능 최적화 팁**

```java
// 📈 카운트 쿼리 최적화
public long getOrderCount(OrderSearchRequest request) {
    Specification<Order> spec = Specification
        .where(OrderSpecification.withMemberName(request.getMemberName()))
        .and(OrderSpecification.withShippingStatus(request.getStatus()));
        
    return orderRepository.count(spec); // ✨ count 전용 메서드 활용
}

// 📄 페이징 성능 최적화
public Page<OrderResponse> getOrdersWithOptimizedPaging(OrderSearchRequest request, Pageable pageable) {
    Specification<Order> spec = Specification
        .where(OrderSpecification.withMemberName(request.getMemberName()));
        
    return orderRepository.findAll(spec, pageable) // ✨ 페이징 최적화된 쿼리
            .map(OrderResponse::from);
}
```

### 🧪 **단위 테스트 작성**

```java
@Test
public void testOrderSpecification() {
    // 🧩 각 Specification을 독립적으로 테스트 가능
    Specification<Order> spec = OrderSpecification.withMemberName("홍길동");
    List<Order> orders = orderRepository.findAll(spec);
    
    assertThat(orders).allMatch(order -> 
        order.getMember().getMemberName().contains("홍길동"));
}
```

---

## 🏆 결론

`JpaSpecificationExecutor`는 **JpaRepository의 기본 기능에 강력한 동적 쿼리 조립 능력을 추가해주는 핵심 인터페이스**입니다.

### 🎯 핵심 가치

- **🧩 레고 블록식 조합**: 검색 조건들을 자유자재로 조립
- **📖 선언적 코드**: 비즈니스 의도가 명확하게 드러나는 코드  
- **🔄 높은 재사용성**: 한 번 만든 Specification을 여러 곳에서 활용
- **🛡️ 관심사 분리**: 복잡한 쿼리 생성 로직을 서비스에서 분리
- **📄 페이징 지원**: 대용량 데이터 처리를 위한 효율적인 페이징

### 🚀 적용 효과

이처럼 Specification을 별도 클래스로 분리하면, **서비스 계층은 "어떤 조건으로 검색하는지"만 선언적으로 명시**하면 되므로 코드가 훨씬 더 깔끔해지고, 각 Specification은 다른 곳에서도 재사용할 수 있게 되어 **유지보수성이 크게 향상**됩니다!

복잡한 검색 기능이 필요한 실무 프로젝트에서는 **반드시 고려해볼 만한 강력한 도구**입니다.
