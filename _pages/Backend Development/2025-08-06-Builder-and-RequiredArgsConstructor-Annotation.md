---
title: "📚[Backend Development] `@Builder`와 `@RequiredArgsConstructor`"
tags:
    - Backend Ddevelopment
    - Annotation
    - Lombok
date: "2025-08-06"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 📚[Backend Development] `@Builder`와 `@RequiredArgsConstructor`

Lombok의 `@Builder`와 `@RequiredArgsContructor` 어노테이션에 대해 알아겠습니다.


# 📦 `@Builder` 어노테이션

## ✅ 1. `@Builder` 어노테이션은 언제 사용하나요?

`@Builder`는 **객체를 생성**할 때 사용하며, 특히 아래와 같은 상황에서 매우 유용합니다.
- 객체에 설정해야 할 **필드가 많을 때**
- 일부 필드는 **선택적으로 설정**하고 싶을 때
- 객체 생성 시 **코드의 가독성**을 높이고 싶을 때
- 생성된 객체의 **불변성(Immutability)** 을 보장하고 싶을 때

## ✅ 2. `@Builder` 어노테이션은 어디서 사용하나요?

`@Builder` 어노테이션은 주로 **클래스(Class)** 또는 **생성자(Constructor)** 위에 붙여서 사용합니다.

```java
// 1. 클래스에 적용하는 경우
@Builder
public class Product {
    private Long productId;
    private String productName;
    // ...
}

// 2. 생성자에 적용하는 경우 (특정 필드만 빌더에 포함하고 싶을 때)
public class Product {
    private Long productId;
    private String productName;
    
    @Builder
    public Produc(String productName) {
        this.productName = productName;
    }
}
```

## ✅ 3. `@Builder` 어노테이션은 어떻게 사용하나요?

`@Builder`를 클래스에 붙이면, Lombok이 컴파일 시점에 자동으로 빌더 코드를 생성해줍니다.
우리는 아래와 같이 **메서드 체이닝(Method Chaining)** 방식으로 직관적인 코드를 작성할 수 있습니다.

**Java 코드 예시**

```java
// 빌더를 사용하여 객체 생성
Product product = Product.builder()
    .productName("신선한 유기농 우유 1L")
    .productSaleCost(BigDecimal.valueOf(2500))
    .supplier("서울 우유")
    .builder(); // 마지막에 build()를 호출하여 객체 생성 완료
```

`.필드명(값)` 형태로 원하는 값만 설정하고, 순서에 상관없이 자유롭게 작성할 수 있습니다.

## ✅ 4. `@Builder` 어노테이션은 왜 사용하나요?

`@Builder`는 **객체 생성을 더 안전하고, 유연하며, 읽기 쉽게** 만들기 위해 사용합니다.
이는 '빌더 디자인 패턴(Builder Design Pattern)'을 자동으로 구현해주는 것입니다.

- **가독성 (Readability) :** `new Product(1L, "우유", ...)` 처럼 생성자를 사용하는 것보다, `builder().productName("우유").build()`처럼 어떤 필드에 어떤 값이 들어가는지 명확하게 알 수 있습니다.
- **유연성 (Flexibility) :** 생성자와 달리 필요한 필드만 선택적으로 설정할 수 있고, 순서에 구애받지 않습니다.
- **객체 일관성 / 불변성 (Consistency / Immutability) :** `bulid()` 메서드가 호출되기 전까지는 객체가 생성되지 않습니다. 따라서 여러 줄의 `setter`를 사용하는 방식과 달리, 객체가 불완전한 상태로 외부에 노출될 위험이 없습니다.

# 📦 `@RequiredArgsConstructor` 어노테이션

`@RequiredArgsConstructor`는 **필수 인자(final 필드)만을 받는 생성자를 자동으로 만들어주는** Lombok 어노테이션입니다.

## ✅ 1. `@RequiredArgsConstructor` 어노테이션은 무엇인가요?

`@RequiredArgsContructor`는 Lombok 라이브러리 어노테이션 중 하나로, 클래스 내에서 `final` 키워드가 붙어 있거나 `@NonNull` 어노테이션이 붙은 필드만을 인자로 받는 생성자를 컴파일 시점에 자동으로 생성해줍니다.

## ✅ 2. `@RequiredArgsConstructor` 어노테이션은 언제사용하나요?

주로 Spring 프레임워크에서 **생성자 기반의 의존성 주입(DI, Dependency Injection)** 을 구현할 때 사용됩니다.

## ✅ 3. `@RequiredArgsConstructor` 어노테이션은 어디서 사용하나요?

**클래스(Class) 레벨**에 선언하여 사용합니다.

## ✅ 4. `@RequiredArgsConstructor` 어노테이션은 어떻게 사용하나요?

아래와 같이 클래스 위에 어노테이션을 붙여주기만 하면 됩니다.

```java
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;

@Service
@RequiredArgsConstructor // 이 어노테이션이 final 필드를 위한 생성자를 자동으로 만듭니다.
public class ProductService {
    
    private final ProductRepository productRepository; // final로 선언된 필수 의존성
    private final StockRepository stockRepository; // final로 선언된 필수 의존성
    
    private String optionalField; // final이 아니므로 생성자에 포함되지 않음
    
    /*
     // 아래 생성자가 컴파일 시점에 자동으로 생성됩니다.
     public ProductService(ProductRepository productRepository, StockRepository stockRepository) {
         this.productRepository = productRepository;
         this.stockRepository = stockRepository;
     }
     */
}
```

## ✅ 5. 왜 사용하나요? (Why)

`@RequiredArgsConstructor`를 사용하는 이유는 **코드의 간결성**과 **안정성**을 높이기 위함입니다.

- **보일러플레이트 코드 제거 :** 의존성이 추가되거나 변경될 때마다 생성자 코드를 직접 수정할 필요 없이, `final` 필드를 선언하기만 하면 되므로 코드가 매우 깔끔해집니다.
- **안전한 객체 생성 :** `final` 필드는 반드시 생성 시점에 초기화되어야 하므로, 의존성이 누락되는 것을 컴파일 단계에서 방지할 수 있습니다.
- **불변성 확보 :** `final`로 선언된 의존성은 변경이 불가능하므로, 객체의 불변성을 보장하여 애플리케이션의 안정성을 높입니다.
