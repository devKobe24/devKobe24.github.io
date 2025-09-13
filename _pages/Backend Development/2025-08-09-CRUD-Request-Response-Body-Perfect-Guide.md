---
title: "📚[Backend Development] CRUD 각 기능별 Request/Response Body 설계 완벽 가이드"
tags:
    - Backend Ddevelopment
    - HTTP
    - RESTful API
    - API
    - Architecture
    - CRUD
date: "2025-08-09"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 🎯 CRUD API Request/Response Body 설계 완벽 가이드

> **🤔 왜 이 가이드를 만들었나?**
> 
> Java Spring 백엔드 프로젝트에서 API 명세서를 작성하는 과정에서, CRUD의 Request/Response Body를 어떻게 설계해야 하는지 명확한 원칙과 패턴을 이해하지 못해 정리한 실전 가이드입니다.

## 🎯 학습 목표

**RESTful API 설계의 핵심인 CRUD 각 기능별 Request/Response Body 작성 원칙과 패턴을 완벽히 마스터하자!**

---

## 📋 목차

1. [RESTful API Body 설계의 핵심 원칙](#-restful-api-body-설계의-핵심-원칙)
2. [CRUD별 Request/Response Body 패턴](#-crud별-requestresponse-body-패턴)
3. [Spring Boot 실전 적용 예시](#-spring-boot-실전-적용-예시)
4. [API 명세서 작성 체크리스트](#-api-명세서-작성-체크리스트)

---

## 🎨 RESTful API Body 설계의 핵심 원칙

### 💡 대원칙: **"최소한의 정보로 명확하게 소통한다"**

RESTful API의 Body 설계는 각 HTTP Method의 본래 목적에 맞춰 필요한 정보만 주고받는 것이 핵심입니다.

### 📊 HTTP Method별 Body 사용 원칙

| HTTP Method | Request Body | Response Body | 핵심 원칙 |
|-------------|-------------|---------------|-----------|
| **POST** 📝 | ✅ 필수 | ✅ 필수 | 생성에 필요한 모든 정보 → 완전한 생성 결과 |
| **GET** 🔍 | ❌ 사용 안함 | ✅ 필수 | URL 파라미터로 조건 → 조회 결과 반환 |
| **PATCH/PUT** ✏️ | ✅ 필수 | ✅ 필수 | 변경할 정보만 → 완전한 수정 결과 |
| **DELETE** 🗑️ | ❌ 사용 안함 | ❌ 비워둠 | URL로 식별 → 204 상태 코드로 성공 표시 |

---

## 🔄 CRUD별 Request/Response Body 패턴

### 📝 CREATE (POST) - 새로운 리소스 생성

#### 🎯 설계 원칙
- **Request**: 서버가 자동 생성하는 값(ID, 생성일시)을 제외한 **모든 필수 정보**
- **Response**: 서버에서 생성된 값을 포함한 **완전한 리소스 상태**

#### 💻 예시: 상품 등록 API
```http
POST /api/products
```

**📤 Request Body**
```json
{
    "productName": "신선한 목장 우유 1L",
    "productSaleCost": 2500,
    "initialStockCount": 50,
    "expirationDate": "2025-12-31"
}
```

**📥 Response Body (201 Created)**
```json
{
    "productId": "PROD-001",
    "productName": "신선한 목장 우유 1L",
    "productSaleCost": 2500,
    "totalStockCount": 50,
    "expirationDate": "2025-12-31",
    "createdAt": "2025-08-09T10:30:00",
    "updatedAt": "2025-08-09T10:30:00",
    "status": "ACTIVE"
}
```

---

### 🔍 READ (GET) - 리소스 조회

#### 🎯 설계 원칙
- **Request**: Body 사용하지 않음, 모든 조건은 URL 파라미터로
- **Response**: 단일 객체 `{}` 또는 배열 `[]` 형태로 반환

#### 💻 예시: 상품 조회 API

**단일 상품 조회**
```http
GET /api/products/PROD-001
```

**📥 Response Body (200 OK)**
```json
{
    "productId": "PROD-001",
    "productName": "신선한 목장 우유 1L",
    "productSaleCost": 2500,
    "totalStockCount": 50,
    "status": "ACTIVE"
}
```

**상품 목록 조회**
```http
GET /api/products?name=우유&status=ACTIVE&page=0&size=10
```

**📥 Response Body (200 OK)**
```json
{
    "content": [
        {
            "productId": "PROD-001",
            "productName": "신선한 목장 우유 1L",
            "productSaleCost": 2500,
            "status": "ACTIVE"
        },
        {
            "productId": "PROD-002", 
            "productName": "저지 우유 500ml",
            "productSaleCost": 1800,
            "status": "ACTIVE"
        }
    ],
    "totalElements": 2,
    "totalPages": 1,
    "currentPage": 0
}
```

---

### ✏️ UPDATE (PATCH/PUT) - 리소스 수정

#### 🎯 설계 원칙
- **PATCH**: 변경할 필드만 전송 (부분 업데이트)
- **PUT**: 리소스 전체 교체
- **Response**: 수정 완료된 **전체 리소스 상태**

#### 💻 예시: 상품 수정 API
```http
PATCH /api/products/PROD-001
```

**📤 Request Body**
```json
{
    "productSaleCost": 2800,
    "totalStockCount": 30
}
```

**📥 Response Body (200 OK)**
```json
{
    "productId": "PROD-001",
    "productName": "신선한 목장 우유 1L",
    "productSaleCost": 2800,
    "totalStockCount": 30,
    "expirationDate": "2025-12-31",
    "updatedAt": "2025-08-09T15:45:00",
    "status": "ACTIVE"
}
```

---

### 🗑️ DELETE - 리소스 삭제

#### 🎯 설계 원칙
- **Request**: Body 사용하지 않음
- **Response**: 빈 Body + 204 No Content 상태 코드

#### 💻 예시: 상품 삭제 API
```http
DELETE /api/products/PROD-001
```

**📥 Response (204 No Content)**
```
(비어있음)
```

---

## 🚀 Spring Boot 실전 적용 예시

### 📁 프로젝트 구조에서 DTO 활용

```java
// Request DTO
@Data
@NoArgsConstructor
public class ProductCreateRequest {
    @NotBlank(message = "상품명은 필수입니다")
    private String productName;
    
    @Min(value = 0, message = "가격은 0 이상이어야 합니다")
    private Integer productSaleCost;
    
    @Min(value = 0, message = "재고는 0 이상이어야 합니다") 
    private Integer initialStockCount;
    
    @Future(message = "유통기한은 미래 날짜여야 합니다")
    private LocalDate expirationDate;
}

// Response DTO
@Data
@Builder
public class ProductResponse {
    private String productId;
    private String productName;
    private Integer productSaleCost;
    private Integer totalStockCount;
    private LocalDate expirationDate;
    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;
    private ProductStatus status;
}
```

### 🎯 Controller에서의 활용

```java
@RestController
@RequestMapping("/api/products")
public class ProductController {
    
    @PostMapping
    public ResponseEntity<ProductResponse> createProduct(
            @Valid @RequestBody ProductCreateRequest request) {
        
        ProductResponse response = productService.createProduct(request);
        return ResponseEntity.status(HttpStatus.CREATED).body(response);
    }
    
    @GetMapping("/{productId}")
    public ResponseEntity<ProductResponse> getProduct(
            @PathVariable String productId) {
        
        ProductResponse response = productService.getProduct(productId);
        return ResponseEntity.ok(response);
    }
    
    @PatchMapping("/{productId}")
    public ResponseEntity<ProductResponse> updateProduct(
            @PathVariable String productId,
            @RequestBody ProductUpdateRequest request) {
        
        ProductResponse response = productService.updateProduct(productId, request);
        return ResponseEntity.ok(response);
    }
    
    @DeleteMapping("/{productId}")
    public ResponseEntity<Void> deleteProduct(@PathVariable String productId) {
        productService.deleteProduct(productId);
        return ResponseEntity.noContent().build();
    }
}
```

---

## ✅ API 명세서 작성 체크리스트

### 📝 Request Body 체크포인트

- [ ] **POST**: 자동 생성 값(ID, 생성일시) 제외한 모든 필수 정보 포함
- [ ] **GET/DELETE**: Body 사용하지 않음
- [ ] **PATCH**: 변경할 필드만 포함
- [ ] **PUT**: 전체 리소스 정보 포함
- [ ] **Validation**: `@Valid`, `@NotNull` 등 검증 어노테이션 적용

### 📤 Response Body 체크포인트

- [ ] **POST/PATCH/PUT**: 완전한 리소스 상태 반환
- [ ] **GET**: 단일 객체 `{}` 또는 목록 배열 `[]` 반환
- [ ] **DELETE**: 빈 Body + 204 상태 코드
- [ ] **페이징**: `content`, `totalElements`, `totalPages` 등 메타데이터 포함
- [ ] **에러 응답**: 일관된 에러 응답 형식 적용

### 🎯 공통 설계 원칙

- [ ] **일관성**: 프로젝트 전체에서 동일한 네이밍 컨벤션 사용
- [ ] **보안**: 민감한 정보(패스워드, 토큰 등) 응답에서 제외
- [ ] **성능**: 불필요한 필드 제외, 필요시 별도 API 제공
- [ ] **문서화**: Swagger/OpenAPI 등을 활용한 명세서 자동화

---

## 💡 핵심 정리

> 🎯 **RESTful API Body 설계의 핵심**
> 
> 1. **각 HTTP Method의 목적에 맞는 Body 설계**
> 2. **Request는 최소한, Response는 완전하게**
> 3. **일관된 패턴으로 예측 가능한 API**
> 4. **Spring Boot의 DTO/Entity 분리 원칙 준수**

이제 Java Spring 백엔드 프로젝트에서 자신 있게 API 명세서를 작성할 수 있을 거예요! 🚀

---

*이 가이드가 도움이 되었다면, 다음에는 에러 처리와 상태 코드 활용법도 정리해보겠습니다! 👋*
