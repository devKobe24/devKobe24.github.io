---
title: "📚[Backend Development] Request/Response DTO의 상세 구조."
tags:
    - Backend Ddevelopment
    - API
    - Layer
    - Architecture
    - DTO
date: "2025-08-08"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 📚[Backend Development] Request/Response DTO의 상세 구조.

DTO의 상세 구조는 **'어떤 역할을 하느냐'** 에 따라 달라집니다.
Request DTO는 **데이터를 받아 검증하는 것**에, Response DTO는 **데이터를 보기 좋게 가공하여 보여주는 것**에 초점을 맞춥니다.

## 📦 Request DTO의 상세 구조.

Request DTO는 클라이언트로부터 들어오는 데이터를 **안전하게 받아내기 위한 구조**를 가집니다.

- **필드 (Fields)**
    - API 요청 본문(Request Body)의 JSON `key`와 일치하는 멤버 변수들로 구성됩니다.
- **검증 어노테이션 (Validation Annotations)**
    - `@NotBlank`, `@NotNull`, `@Size`, `@Pattern` 등 `jakarta.validation` 어노테이션을 사용하여, 비즈니스 로직에 도달하기 전에 데이터가 유효한지 검사합니다.
        - 이것이 Request DTO의 가장 중요한 특징입니다.
- **기본 생성자와 Getter**
    - JSON 데이터를 객체로 변환(Deserialization)하기 위해 Lombok의 `@NoArgsConstructor`와 `@Getter`를 주로 사용합니다.

**코드 예시 :** `ProductCreateRequestDto.java`

```java
@Getter
@NoArgsConstructor // JSON 변환을 위한 기본 생성자
public class ProductCreateRequestDto {
    
    @NotBlank(message = "상품명은 필수입니다.")
    @Size(max = 100, message = "상품명은 100자를 넘을 수 없습니다.")
    private String productName;
    
    @NotNull(message = "판매가는 필수입니다.")
    @Positive(message = "판매가는 0보다 커야 합니다.")
    private BigDecimal productSaleCost;
    
    // ... 요청에 필요한 다른 필드와 검증 규칙들 ...
}
```

## 📦 Response DTO의 상세 구조.

Response DTO는 서버의 처리 결과를 클라이언트에게 **명확하고 사용하기 편한 형태로 보여주기 위한 구조**를 가집니다.

- **필드 (Fields)**
    - `Entity`의 데이터를 그대로 보여주기도 하고, 여러 `Entity`의 정보를 조합하거나 `Service`에서 계산된 값을 포함하기도 합니다.
    - 예: `margin`, `totalStockCount`
- **불변성 (Immutability)**
    - 응답으로 나가는 데이터가 중간에 변경되지 않도록 `final` 필드와 생성자(`@Builder` 활용)를 사용하여 불변 객체로 만드는 것이 좋은 패턴입니다. `setter`는 사용하지 않습니다.
- **정적 팩토리 메서드 (Static Factory Method)**
    - `Entity` 객체를 DTO 객체로 변환하는 로직을 DTO 내부에 정적 메서드(e.g, `from(Product product)`)로 만들어두면, `Service` 계층의 코드를 더 깔끔하게 유지할 수 있습니다.

**코드 예시: `ProductResponseDto.java`**

```java
@Getter
public class ProductResponseDto {
    
    private final Long productId;
    private final String productName;
    private final BigDecimal fianlSalePrice; // 계산된 최동 판매가
    private final int totalStockCount; // 계산된 총재고
        
    // DTO는 불변성을 위해 Builder를 통한 생성자만 허용
    @Builder
    
    private ProductResponseDto(Long productId, String productName, BigDecimal finalSalePrice, int totalStockCount) {
        this.productId = productId;
        this.productName = productName;
        this.finalSalePrice = finalSalePrice;
        this.totalStockCount = totalStockCount;
    }
    
    // Entity를 DTO로 변환하는 정적 팩토리 메서드
    public static ProductResponseDto from(Product product, int totalStockCount) {
        return ProductResponseDto.builder()
            .productId(product.getProductId())
            .productName(product.getProductName())
            .finalSalePrice(calculateFinalPrice(product.getProductSaleCost(), product.getProductDiscountRate()))
            .totalStockCount(totalStockCount)
            .build();
    }
    
    private static BigDecimal calculateFinalPrice(BigDecimal saleCost, Integer discountRate) {
        // ... 가격 계산 로직 ...
    }
}
```
