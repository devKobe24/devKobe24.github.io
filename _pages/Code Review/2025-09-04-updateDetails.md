---
title: 💻[Code Review] `Product` 클래스의 `updateDetails` 대한 코드 리뷰.
tags:
    - Code review
    - OOP
    - SOLID
date: "2025-09-04"
thumbnail: "/assets/img/thumbnail/codereview.jpg"
---

## 🏛️ 아키텍처 및 설계 (Architecture & Design)

* `Product` 클래스의 `updateDetails` 메서드는 **Rich Domain Model(풍부한 도메인 모델)** 패턴을 완벽하게 구현한 사례입니다.
    * 이는 **도메인 주도 설계(Domain-Driven Design, DDD)** 의 핵심 원칙 중 하나로, 비즈니스 로직을 도메인 객체 내부에 캡슐화하여 다음과 같은 이점을 제공합니다:
        * **Domain :** 비즈니스 규칙과 데이터 변경 로직을 자체적으로 관리
        * **Service :** 비즈니스 프로세스 조정(Orchestration)에만 집중
        * **캡슐화 :** 객체의 상태 변경을 제어된 방식으로만 허용
        * **응집도 향상 :** 관련된 데이터와 로직이 한 곳에 모임

이러한 설계는 **SOLID 원칙** 중 **단일 책임 원칙(SRP)** 과 **캡슐화(Encapsulation)** 를 동시에 만족하는 이상적인 구조입니다.

---

## ✅ 클래스별 상세 리뷰 (Detailed Class Review)

```java
package com.kobe.productmanagement.domain;

import com.kobe.productmanagement.common.BaseTimeEntity;
import com.kobe.productmanagement.common.Category;
import jakarta.persistence.*;
import lombok.AccessLevel;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;

import java.util.ArrayList;
import java.util.List;

@Entity
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Product extends BaseTimeEntity {

	@Id
	@Column(length = 26) // ULID는 26자로 고정되므로 길이를 지정해주는 것이 좋습니다.
	private String productId;

	@Column(nullable = false)
	private String productName;

	@Column(nullable = false)
	private Integer productRegularPrice;

	@Column(nullable = false)
	private Integer stockQuantity;

	@Enumerated(EnumType.STRING)
	@Column(nullable = false)
	private Category category;

	@Column(nullable = false)
	private Integer productCostPrice;

	@Column(nullable = false)
	private String productSupplier;

	@Column(nullable = false, unique = true)
	private String barcodeNumber;

	// 양방향 관계를 위한 코드
	@OneToMany(mappedBy = "product", cascade = CascadeType.ALL, orphanRemoval = true, fetch = FetchType.LAZY)
	private List<Stock> stocks = new ArrayList<>();

	@Builder
	public Product(String productId,
	               String productName,
	               Integer productRegularPrice,
	               Integer stockQuantity,
	               Category category,
	               Integer productCostPrice,
	               String productSupplier,
	               String barcodeNumber
	) {
		this.productId = productId;
		this.productName = productName;
		this.productRegularPrice = productRegularPrice;
		this.stockQuantity = stockQuantity;
		this.category = category;
		this.productCostPrice = productCostPrice;
		this.productSupplier = productSupplier;
		this.barcodeNumber = barcodeNumber;
	}

	public void increaseStockQuantity(int quantity) {
		this.stockQuantity += quantity;
	}

	public void decreaseStockQuantity(int quantity) {
		int restStock = this.stockQuantity - quantity;
		if (restStock < 0) {
			throw new IllegalArgumentException("재고는 0개 미만이 될 수 없습니다. 현재 재고 :" + this.stockQuantity);
		}
		this.stockQuantity = restStock;
	}

	// OOP[캡슐화/Encapsulation] + SOLID[SRP/단일 책임 원칙]
	public void updateDetails(String productName,
	                         Integer productRegularPrice,
	                         Integer stockQuantity,
	                         Category category,
	                         Integer productCostPrice,
	                         String productSupplier,
	                         String barcodeNumber
	) {
		if (productName != null) {
			this.productName = productName;
		}
		if (productRegularPrice != null) {
			this.productRegularPrice = productRegularPrice;
		}
		if (stockQuantity != null) {
			this.stockQuantity = stockQuantity;
		}
		if (category != null) {
			this.category = category;
		}
		if (productCostPrice != null) {
			this.productCostPrice = productCostPrice;
		}
		if (productSupplier != null) {
			this.productSupplier = productSupplier;
		}
		if (barcodeNumber != null) {
			this.barcodeNumber = barcodeNumber;
		}
	}
}
```

### 1. `Product.java`(Domain Entity) - ⭐️ 핵심 분석

#### **🔒 캡슐화(Encapsulation) 완벽 구현**
* **데이터 보호**: 모든 필드가 `private`으로 선언되어 외부에서 직접 접근 불가
* **제어된 접근**: `updateDetails` 메서드를 통해서만 상태 변경 가능
* **자체 검증 로직**: `null` 체크를 통한 선택적 업데이트 구현
* **비즈니스 규칙 내재화**: 상품 정보 변경에 대한 모든 규칙이 도메인 객체 내부에 위치

#### **🎯 단일 책임 원칙(SRP) 준수**
* **Product의 책임**: 자신의 상태와 관련된 비즈니스 규칙 관리
* **Service Layer와의 역할 분리**: 
  - Service는 프로세스 조정만 담당
  - Product는 상태 변경 로직만 담당
* **응집도 향상**: 관련 데이터와 로직이 한 곳에 집중

#### **🛡️ 방어적 프로그래밍(Defensive Programming)**
```java
if (productName != null) {
    this.productName = productName;
}
```
* **Null Safety**: 각 파라미터에 대한 null 체크로 NPE(Null Pointer Excepiton) 방지
* **부분 업데이트 지원**: null이 아닌 값만 선택적으로 업데이트
* **안전한 상태 변경**: 무효한 데이터로 인한 객체 상태 오염 방지

#### **🏗️ 추가 비즈니스 메서드들**
```java
public void decreaseStockQuantity(int quantity) {
    int restStock = this.stockQuantity - quantity;
    if (restStock < 0) {
        throw new IllegalArgumentException("재고는 0개 미만이 될 수 없습니다. 현재 재고 :" + this.stockQuantity);
    }
    this.stockQuantity = restStock;
}
```
* **비즈니스 규칙 강제**: 재고 감소 시 음수 방지 로직
* **명확한 예외 메시지**: 비즈니스 규칙 위반 시 구체적인 오류 정보 제공
* **도메인 지식 표현**: 실제 비즈니스 요구사항을 코드로 명시적 표현

---

## 🏆 총평

### **✅ 훌륭한 점들**

1. **Rich Domain Model의 완벽한 구현**
   - 단순한 Data Container가 아닌 비즈니스 로직을 포함한 지능적 객체
   - Anemic Domain Model 안티패턴을 완벽히 회피

2. **SOLID 원칙의 실천적 적용**
   - SRP: 각 레이어와 객체의 책임이 명확히 분리
   - OCP: 새로운 업데이트 규칙 추가 시 기존 코드 수정 없이 확장 가능

3. **유지보수성 극대화**
   - 상품 정보 변경 정책이 바뀌어도 Product 클래스만 수정하면 됨
   - Service Layer의 코드는 전혀 건드릴 필요 없음

4. **안정성과 견고성**
   - Null Safety를 통한 방어적 프로그래밍
   - 비즈니스 규칙 위반 시 명확한 예외 처리

### **🎯 앞으로의 발전 방향**

이러한 설계 패턴을 지속적으로 적용하면서 다음과 같은 고급 패턴들도 고려해볼 수 있습니다:

- **Domain Event**: 상품 정보 변경 시 이벤트 발행으로 다른 바운디드 컨텍스트와의 연동
- **Value Object**: 가격, 재고 수량 등을 Value Object로 분리하여 더욱 견고한 도메인 모델 구축
- **Specification Pattern**: 복잡한 비즈니스 규칙 검증을 위한 명세 패턴 도입

현재의 `updateDetails` 메서드는 **객체지향 설계의 모범 사례**이며, 이런 방식을 지속해서 적용한다면 **확장 가능하고 유지보수하기 쉬운 엔터프라이즈급 애플리케이션**을 구축할 수 있을 것입니다.
