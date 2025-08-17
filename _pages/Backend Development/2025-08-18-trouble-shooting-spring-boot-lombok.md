---
title: "📚[Backend Development] 🚨 Spring Boot + Lombok 트러블슈팅 가이드"
tags:
    - Backend Development
    - Lombok
    - Spring Boot
    - Trouble Shooting
date: "2025-08-18"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 🚨 Spring Boot + Lombok 트러블슈팅 가이드

Spring Boot와 Lombok을 사용하는 과정에서 직접 맞닥뜨린 에러를 해결하는 과정을 기록해 보았습니다.

---

## 🔍 문제 1: Lombok Getter 메서드를 찾을 수 없음

### 📋 에러 상황
ProductManagementApplication 실행 시 다음과 같은 컴파일 에러가 발생했습니다.

```bash
/Users/kobe/Desktop/ProductManagement/src/main/java/com/kobe/productmanagement/dto/response/StockResponse.java:35: 
error: cannot find symbol

stock.getStockId(),
     ^
  symbol:   method getStockId()
  location: variable stock of type Stock
```

### 🎯 원인 분석
**`cannot find symbol` 에러는 Java 컴파일러가 코드에서 참조하는 메서드나 변수를 찾지 못할 때 발생합니다.**

1. **컴파일 시점 문제**: `Stock` 클래스에서 `getStockId()` 메서드를 찾을 수 없음
2. **Lombok 동작 실패**: `@Getter` 어노테이션이 제대로 처리되지 않아 getter 메서드가 생성되지 않음
3. **IDE vs 실제 빌드**: IDE에서는 정상 작동하지만 실제 빌드 시 실패

### 🔧 해결 방법

#### 1단계: Stock 엔티티 클래스 확인

```java
@Entity
@Table(name = "stocks")
@Getter  // ⭐ 이 어노테이션이 있는지 반드시 확인!
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Stock extends BaseTimeEntity {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "stock_id")
    private Long stockId;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "product_id", nullable = false)
    private Product product;
    
    @Column(name = "barcode_number", unique = true)
    private String barcodeNumber;
    
    // Lombok이 자동으로 생성할 메서드들:
    // - getStockId()
    // - getProduct()
    // - getBarcodeNumber()
}
```

#### 2단계: 클린 빌드 실행

```bash
# Gradle 사용자
./gradlew clean build

# Maven 사용자
./mvnw clean install
```

### ✅ 확인 포인트
- [ ] `@Getter` 어노테이션이 클래스에 추가되어 있는가?
- [ ] `import lombok.Getter;` import 구문이 있는가?
- [ ] 클린 빌드를 실행했는가?

---

## 🔍 문제 2: 대량의 Lombok 메서드 누락 에러

### 📋 에러 상황
`./gradlew clean build` 실행 시 25개의 컴파일 에러가 한번에 발생했습니다.

```bash
> Task :compileJava FAILED

error: cannot find symbol
  symbol:   method getStockId()
  location: variable stock of type Stock

error: cannot find symbol
  symbol:   method getProduct()
  location: variable stock of type Stock

error: cannot find symbol
  symbol:   method builder()
  location: class Product

error: cannot find symbol
  symbol:   method getName()
  location: variable request of type ProductCreateRequest

25 errors
```

### 🎯 원인 분석
**IDE에서는 Lombok이 정상 작동하지만, Gradle 빌드 시에는 Lombok 어노테이션 프로세서가 동작하지 않고 있습니다.**

### 💡 핵심 개념 이해
- **IDE의 Lombok 플러그인**: 개발 중 코드 하이라이팅과 자동완성을 위함
- **빌드 도구의 어노테이션 프로세서**: 실제 .class 파일 생성 시 메서드를 만드는 역할

### 🔧 해결 방법

#### build.gradle 설정 수정

```gradle
dependencies {
    // 기존 의존성들
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-validation'
    
    // ⭐ Lombok 설정 추가 (가장 중요!)
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    
    // 테스트용 어노테이션 프로세서도 추가
    testCompileOnly 'org.projectlombok:lombok'
    testAnnotationProcessor 'org.projectlombok:lombok'
    
    // 데이터베이스 드라이버
    runtimeOnly 'com.mysql:mysql-connector-j'
    testImplementation 'com.h2database:h2'  // 테스트용 H2 DB
    
    // 테스트 의존성
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

#### 실무 코드 예시

설정 후 다음과 같은 코드들이 정상 작동합니다:

```java
// ✅ ProductService.java
@Service
@RequiredArgsConstructor
public class ProductService {
    
    private final ProductRepository productRepository;
    
    public Long createProduct(ProductCreateRequest request) {
        Product newProduct = Product.builder()  // @Builder 어노테이션으로 생성
                .name(request.getName())         // @Getter로 생성된 메서드
                .price(request.getPrice())
                .stockQuantity(request.getStockQuantity())
                .category(request.getCategory())
                .costPrice(request.getCostPrice())
                .productSupplier(request.getProductSupplier())
                .barcodeNumber(request.getBarcodeNumber())
                .build();
        
        Product savedProduct = productRepository.save(newProduct);
        return savedProduct.getProductId();  // @Getter로 생성된 메서드
    }
}
```

```java
// ✅ StockResponse.java
public record StockResponse(
    Long stockId,
    String productName,
    BigDecimal costPrice,
    BigDecimal sellingPrice,
    Integer stockQuantity,
    String category,
    LocalDateTime createdAt,
    LocalDateTime updatedAt,
    String productSupplier
) {
    public static StockResponse from(Stock stock) {
        return new StockResponse(
            stock.getStockId(),              // ✅ 정상 작동
            stock.getProduct().getName(),    // ✅ 정상 작동
            stock.getProduct().getCostPrice(),
            stock.getProduct().getCostPrice(),
            stock.getProduct().getStockQuantity(),
            stock.getProduct().getCategory(),
            stock.getCreatedAt(),            // BaseTimeEntity에서 상속
            stock.getUpdatedAt(),
            stock.getProduct().getProductSupplier()
        );
    }
}
```

### 📚 어노테이션 프로세서 설정 설명

| 설정 | 역할 |
|------|------|
| `compileOnly` | 컴파일 시점에만 필요하고, 런타임에는 불필요한 의존성 |
| `annotationProcessor` | **핵심!** 컴파일 시 어노테이션을 실제 코드로 변환하는 프로세서 |
| `testCompileOnly` | 테스트 코드 컴파일 시에만 필요한 의존성 |
| `testAnnotationProcessor` | 테스트 코드에서도 Lombok 어노테이션 처리 |

---

## 🔍 문제 3: 테스트 실행 시 데이터베이스 연결 실패

### 📋 에러 상황
빌드는 성공했지만 테스트 실행 시 다음 에러가 발생했습니다.

```bash
> Task :test FAILED

ProductManagementApplicationTests > contextLoads() FAILED
    java.lang.IllegalStateException at DefaultCacheAwareContextLoaderDelegate.java:180
        Caused by: org.springframework.beans.factory.BeanCreationException
            Caused by: org.hibernate.service.spi.ServiceException
                Caused by: org.hibernate.HibernateException at DialectFactoryImpl.java:191

FAILURE: Build failed with an exception.
```

### 🎯 원인 분석

**Hibernate가 데이터베이스 방언(Dialect)을 결정하지 못해 발생하는 문제입니다.**

1. **테스트 환경의 드라이버 부족**: `runtimeOnly`로 선언된 MySQL 드라이버는 테스트에서 사용할 수 없음
2. **설정 파일 중복**: 테스트도 메인 `application.properties`를 사용해 MySQL 연결 시도

### 🔧 해결 방법

#### 1단계: 테스트용 설정 파일 생성

**📁 src/test/resources/application.yml** 파일을 새로 생성합니다.

```yaml
# ===============================
# 🧪 TEST DATABASE (H2 In-Memory)
# ===============================
# Spring Boot가 H2 데이터베이스를 자동으로 설정하도록 합니다.

spring:
  # JPA/Hibernate 설정
  jpa:
    hibernate:
      ddl-auto: create-drop
    show-sql: true
    properties:
      hibernate:
        format_sql: true
        
  # H2 Console 활성화 (디버깅용)
  h2:
    console:
      enabled: true
      path: /h2-console

# 로깅 설정
logging:
  level:
    org.springframework.web: DEBUG
    com.kobe.productmanagement: DEBUG
```

#### 2단계: 실제 테스트 코드 예시

```java
@SpringBootTest
@Transactional
class ProductServiceTest {
    
    @Autowired
    private ProductService productService;
    
    @Autowired
    private ProductRepository productRepository;
    
    @Test
    @DisplayName("상품 생성 테스트 - H2 데이터베이스 사용")
    void createProduct_Success() {
        // given
        ProductCreateRequest request = ProductCreateRequest.builder()
            .name("맥북 프로 16인치")
            .price(new BigDecimal("2490000"))
            .stockQuantity(10)
            .category("전자제품")
            .costPrice(new BigDecimal("2000000"))
            .productSupplier("Apple Korea")
            .barcodeNumber("8801234567890")
            .build();
        
        // when
        Long productId = productService.createProduct(request);
        
        // then
        assertThat(productId).isNotNull();
        
        Optional<Product> savedProduct = productRepository.findById(productId);
        assertThat(savedProduct).isPresent();
        assertThat(savedProduct.get().getName()).isEqualTo("맥북 프로 16인치");
    }
    
    @Test
    @DisplayName("재고 조회 테스트 - 연관관계 포함")
    void findStockWithProduct_Success() {
        // given - 테스트 데이터 준비
        Product product = Product.builder()
            .name("아이폰 15 Pro")
            .price(new BigDecimal("1350000"))
            .stockQuantity(5)
            .category("스마트폰")
            .costPrice(new BigDecimal("1100000"))
            .productSupplier("Apple Korea")
            .barcodeNumber("8801234567891")
            .build();
        
        Product savedProduct = productRepository.save(product);
        
        Stock stock = Stock.builder()
            .product(savedProduct)
            .barcodeNumber("STOCK_8801234567891")
            .build();
        
        // when & then - H2 데이터베이스에서 정상 작동
        assertThat(stock.getProduct().getName()).isEqualTo("아이폰 15 Pro");
    }
}
```

### 🏗️ 환경별 설정 구조

```
src/
├── main/
│   └── resources/
│       └── application.yml          # 🚀 운영/개발용 (MySQL)
└── test/
    └── resources/
        └── application.yml          # 🧪 테스트용 (H2)
```

#### 운영 환경 설정 (main/resources)
```yaml
# MySQL 데이터베이스 연결
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/product_management
    username: root
    password: password
  jpa:
    hibernate:
      ddl-auto: validate
```

#### 테스트 환경 설정 (test/resources)
```yaml
# H2 인메모리 데이터베이스 (자동 설정)
spring:
  jpa:
    hibernate:
      ddl-auto: create-drop
  h2:
    console:
      enabled: true
```

---

## 📊 문제 해결 체크리스트

### ✅ Lombok 설정 확인
- [ ] `@Getter`, `@Builder` 등 어노테이션이 클래스에 있는가?
- [ ] `build.gradle`에 `annotationProcessor 'org.projectlombok:lombok'` 추가됨?
- [ ] IDE에 Lombok 플러그인이 설치되어 있는가?

### ✅ 데이터베이스 설정 확인
- [ ] 테스트용 `application.yml` 파일이 별도로 있는가?
- [ ] `testImplementation 'com.h2database:h2'` 의존성이 추가됨?
- [ ] 운영 환경과 테스트 환경의 데이터베이스가 분리되어 있는가?

### ✅ 빌드 및 테스트
- [ ] `./gradlew clean build` 명령어가 성공하는가?
- [ ] 모든 테스트가 통과하는가?
- [ ] IDE에서 개별 테스트 실행이 가능한가?

---

## 🎉 마무리

이제 Spring Boot + Lombok 프로젝트에서 발생하는 주요 문제들을 해결할 수 있습니다! 

### 🚀 다음 단계 권장사항

1. **CI/CD 파이프라인 구축**: GitHub Actions나 Jenkins를 활용한 자동 빌드
2. **테스트 커버리지 측정**: JaCoCo를 활용한 코드 커버리지 확인
3. **프로파일별 설정 분리**: `application-dev.yml`, `application-prod.yml` 등

### 📞 추가 도움이 필요하다면?
- Spring Boot 공식 문서: https://spring.io/projects/spring-boot
- Lombok 공식 문서: https://projectlombok.org/
