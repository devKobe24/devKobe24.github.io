---
title: "🔍[Troubleshooting] 의존성 역전 원칙(DIP) - 인터페이스 VS 구현체"
tags:
    - Troubleshooting
    - Backend Development
    - Spring Boot
    - OOP
    - DIP
date: "2025-08-28"
thumbnail: "/assets/img/thumbnail/troubleshooting.jpg"
---

# 🤔 Troubleshooting 의존성 역전 원칙(DIP)

## 📌 핵심 질문: 인터페이스 vs 구현체, 무엇을 주입받아야 할까?

### 🙋‍♂️ 자주 하는 착각.

```java
@RestController
@RequiredArgsConstructor
public class ProductAdminController {
    // 🤔 이렇게 하는 게 맞는거 아닌가?
    // ❌ 아닙니다!!
    private final ProductServiceImpl productServiceImpl;
    
    // ✅ 실제로는 이렇게 해야 합니다 :)
    private final ProductService productService;
}
```

**"ProductServiceImpl이 실제 구현체인데, 이걸 직접 주입받는 게 더 명확하지 않나요?"**
* 이는 매우 자연스러운 의문이지만, 객체지향 설계의 핵심을 놓친 접근법입니다. 😓

---

## 📌 핵심 포인트: 항상 인터페이스에 의존하라 !

### 코드 구조 예시.

```java
// 📋 인터페이스: 계약서 역할
public interface ProductService {
    ProductResponse createProduct(ProductRequest request);
    ProductResponse getProduct(String productId);
    List<ProductResponse> getProducts();
    // ... 기타 메서드들
}
```

```java
// 🛠️ 구현체: 실제 비즈니스 로직
@Service
@RequiredArgsConstructor
@Transactional
public class ProductServiceImpl implements ProductService {
    
    private final ProductRepository productRepository;
    private final StockRepository stockRepository;
    
    @Override
    public ProductResponse createProduct(ProductRequest request) {
        // 실제 구현 로직
        String newProductId = UlidCreator.getUlid().toString();
        Product newProduct = Product.builder()
            .productId(newProductId)
            .productName(request.getProductName())
            // ... 기타 필드들
            .build();
        
        Product savedProduct = productRepository.save(newProduct);
        return ProductResponse.from(savedProduct);
    }
}
```

```java
// 🎮 컨트롤러: 인터페이스에만 의존
@RestController
@RequiredArgsConstructor
@RequestMapping("/api/admin/products")
public class ProductAdminController {
    // ✅ 인터페이스에 의존 - 이것이 정답!
    private final ProductService productService;
    
    @PostMapping
    public ResponseEntity<ApiResponse<ProductResponse>> createProduct(
        @RequestBody ProductRequest request
    ) {
        ProductResponse response = productService.createProduct(request);
        return ResponseEntity.created(/* URI */).body(/* response */);
    }
}
```

---

## 🔌 USB 포트 비유로 이해하기

### 현실 세계의 예시

| 컴포넌트 | 역할 | 비유 |
|--------|----|-----|
|**ProductAdminController**|사용하는 측|💻 **컴퓨터 본체**|
|**ProductService (인터페이스)**|규격/계약|🔌 **USB 포트**|
|**ProductServiceImpl**|실제 구현|⌨️ **USB 키보드**|

### 🤔 왜 이 비유가 중요할까?
* 컴퓨터는 "USB 포트 규격"만 알면 됨
* 어떤 브랜드의 키보드인지는 관심 없음
* 나중에 마우스, 웹캠으로 바꿔도 컴퓨터는 그대로 동작
* **유연성과 확장성의 핵심**

---

## 💡 인터페이스 의존의 3가지 핵심 이점

### 1. 🚀 확장성 (OCP: 개방-폐쇄 원칙)

### 시나리오: VIP 고객 전용 상품 로직 추가

```java
@Service
@Primary // 이 구현체를 우선 사용
public class VipProductServiceImpl implements ProductService {
    
    private final ProductServiceImpl basicService;
    private final VipPolicyService vipPolicyService;
    
    @Override
    public ProductResponse createProduct(ProductRequest request) {
        // VIP 전용 로직 추가
        if (isVipProduct(request)) {
            return vipPolicyService.createVipProduct(request);
        }
        return basicService.createProduct(request);
    }
}
```

**🎉 결과**: 컨트롤러 코드는 **단 한줄도 변경 없이** 새로운 기능이 추가됨!

### 2. 📎 결합도 감소 (Decoupling)

```java
// ❌ 강한 결함 (나쁜 예)
public class ProductController {
    private final ProductServiceImpl impl; // 구체적인 구현에 의존
    
    // impl의 내부가 바뀌면 여기도 영향을 받을 수 있음
}

// ✅ 느슨한 결합 (좋은 예)
public class ProductController {
    private final ProductService service; // 추상화에 의존
    
    // 구현제가 어떻게 바뀌든 영향받지 않음
}
```

### 3. 🧪 테스트 용이성

```java
// 테스트용 가짜 구현체
@TestConfiguration
public class TestConfig {
    
    @Bean
    @Primary
    public ProductService mockProductService() {
        return new MockProductService(); // 빠르고 가벼운 테스트용 구현
    }
}
```

---

## ⚙️ Spring의 마법: DI와 IoC

### Spring이 인터페이스에서 구현체를 찾는 방법

```java
// 1️⃣ Spring이 시작되면...
@Service // 🏷️ "나는 Bean이야!"
public class ProductServiceImpl implements ProductService {
    // 🍃 Spring: "아하, ProductService의 구현체구나!""
}

// 2️⃣ 컨트롤러가 생성될 때...
@RequiredArgsConstructor // 🏗️ 생성자 자동 생성
public class ProductController {
    private final ProductService service;
    // 🍃 Spring: "ProductService가 필요하네? ProductServiceImpl을 넣어줄게!"
}
```

### DI 컨테이너의 동작 과정

1. **🔍 Bean 스캔**: `@Service`, `@Component` 등이 붙은 클래스들을 찾아 등록
2. **🗺️ 의존관계 맵핑**: 인터페이스와 구현체의 관계를 파악
3. **🛠️ 자동 주입**: 필요한 곳에 적절한 구현체를 자동으로 주입

---

## 📌 핵심 원칙 정리

### DIP (의존성 역전 원칙)의 정의

> **고수준 모듈은 저수준 모듈에 의존하면 안 된다.**
> **둘 다 추상화에 의존해야 한다.**

### 실제 적용

* **고수준 모듈**: `ProductAdminController` (비즈니스 로직을 사용하는 측)
* **저수준 모듈**: `ProductServiceImpl` (구체적인 구현)
* **추상화**: `ProductService` (인터페이스)

### 🎁 성공하는 개발자의 마인드셋

1. **당장의 편리함보다 미래의 확장성을 생각하라**
2. **구체적인 것보다 추상적인 것에 의존하라**
3. **변경에 강한 시스템을 설계하라**

---

## 🚨 자주 하는 실수들

### ❌ 잘못된 접근

```java
// 구현체에 직접 의존
private final ProductServiceImpl productServiceImpl;

// 인터페이스 없이 바로 구현
@Service
public class ProductService { // 인터페이스가 아닌 클래스
    //...
}
```

### ✅ 올바른 접근

```java
// 항상 인터페이스에 의존
private final ProductService productService;

// 인터페이스와 구현체를 분리
public interface ProductService { } // 계약
@Service
public class ProductServiceImpl implements ProductService { } // 구현
```

---

## 🎓 마무리

**의존성 역적 원칙(DIP)은 단순한 기술적 선택이 아닙니다.**

이는 변화하는 요구사항에 유연하게 대응할 수 있는 시스템을 만드는 **개발자의 기본 소양**입니다.

지금 당장은 구현체가 하나뿐이더라도, 미래의 확장성을 위해 항상 인터페이스에 의존하는 습관을 기르세요.

---
