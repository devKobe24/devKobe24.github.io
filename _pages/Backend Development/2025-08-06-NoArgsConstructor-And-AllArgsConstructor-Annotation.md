---
title: "📚[Backend Development] `@NoArgsConstructor`와 `@AllArgsConstructor` 어노테이션"
tags:
    - Backend Ddevelopment
    - Annotation
    - Lombok
date: "2025-08-06"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 📚[Backend Development] `@NoArgsConstructor`와 `@AllArgsConstructor` 어노테이션

# 📦 `@NoArgsConstructor` 어노테이션.

`@NoArgsConstructor`는 **파라미터가 없는 기본 생성자(no-arg constructor)를 자동으로 만들어주는** Lombok 어노테이션입니다.

## ✅ 1. `@NoArgsConstructor` 어노테이션은 무엇인가요?

`@NoArgsConstructor`는 Lombok 라이브러리의 어노테이션 중 하나로, `public MyClass() {}`와 같이 아무런 인자도 받지 않는 생성자 코드를 컴파일 시점에 자동으로 생성해줍니다.

## ✅ 2. `@NoArgsConstructor` 어노테이션은 언제 사용하나요?

**JPA Entity 클래스**를 만들 때 거의 항상 사용됩니다.
또한, JSON 데이터를 객체로 변환하는 라이브러리(e.g, Jackson)를 사용할 때도 필요합니다.

## ✅ 3. `@NoArgsConstructor` 어노테이션은 어디서 사용하나요?

**클래스(Class) 레벨**에서 선언하여 사용합니다.

## ✅ 4. `@NoArgsConstructor` 어노테이션은 어떻게 사용하나요?

클래스 위에 어노테이션을 붙여주기만 하면 됩니다.
JPA Entity에서는 `protected` 접근 제어자를 사용하는 것이 좋은 패턴입니다.

```java
import lombok.AccessLevel;
import lombok.NoArgsConstructor;

@Entiry
@NoArgsConstructor(access = AccessLevel.PROTECTED) // protected 기본 생성자를 자동 생성
public class Product {
    
    @Id
    private Long id;
    private String name;
    
    /*
     // 아래 생성자가 컴파일 시점에 자동으로 생성됩니다.
     protected Product() {
     }
     */
}
```

## ✅ 5. `@NoArgsConstructor` 어노테이션은 왜 사용하나요?

`@NoArgsConstructor`를 사용하는 주된 이유는 **프레임워크의 요구사항을 만족**시키고 **객체 생성의 안정성**을 높이기 위함입니다.

- **프레임워크 호환성 :** JPA와 같은 프레임워크는 내부적으로 객체를 생성하기 위해 기본 생성자를 필요로 합니다. `@NoArgsConstructor`는 이 요구사항을 충족시켜줍니다.
- **안전성 :** `@NoArgsConstructor(access = AccessLevel.PROTECTED)`로 설정하면, 개발자가 `new Product()` 처럼 실수로 불완전한 객체를 생성하는 것을 막을 수 있습니다. 객체 생성은 `@Builder`나 정적 팩토리 메서드 등을 사용하도록 강제하여 코드의 안정성을 높입니다.
- **코드 간결성 :** 개발자가 직접 생성자 코드를 작성할 핑요가 없어 코드가 깔끔해집니다.

# 📦 `@AllArgsConstructor` 어노테이션.

`@AllArgsConstructor` 어노테이션는 **클래스의 모든 필드를 인자로 받는 생성자를 자동으로 만들어주는** Lombok 어노테이션입니다.

## ✅ 1. `@AllArgsConstructor` 어노테이션은 무엇인가요?

`@AllArgsConstructor`는 Lombok 라이브러리의 어노테이션 중 하나로, 클래스에 선언된 모든 필드를 파라미터로 순서대로 받는 생성자 코드를 컴파일 시점에 자동으로 생성해줍니다.

## ✅ 2. `@AllArgsConstructor` 어노테이션은 언제 사용하나요?

- 클래스의 모든 필드를 초기화해야 하는 객체를 만들 때.
- 다른 어노테이션, 특히 `@Builder`와 함께 사용하여 객체 생성을 더 편리하게 만들고 싶을 때.
- 의존성 주입(Dependency Injection) 테스트 등에서 모든 필드를 외부에서 주입받아야 할 때.

## ✅ 3. `@AllArgsConstructor` 어노테이션은 어디서 사용하나요?

**클래스(Class) 레벨**에 선언하여 사용합니다.

## ✅ 4. `@AllArgsConstructor` 어노테이션은 어떻게 사용하나요?

클래스 위에 어노테이션을 붙여주기만 하면 됩니다.

```java
import lombok.AllArgsConstructor;

@AllArgsConstructor // 이 어노테이션이 아래 생성자 코드를 자동으로 만듭니다.
public class Product {
    
    private Long id;
    private String name;
    
    /*
     // 아래 생성자가 컴파일 시점에 자동으로 생성됩니다.
     public Product(Long id, String name) {
         this.id = id;
         this.name = name;
     }
     */
}
```

## ✅ 5. `@AllArgsConstructor` 어노테이션은 왜 사용하나요?

`@AllArgsConstructor`를 사용하는 주된 이유는 **코드의 간결성**과 **편의성** 때문입니다.

- **보일러플레이트 코드 제거 :** 클래스에 필드가 추가되거나 순서가 변경될 때마다 생성자 코드를 직접 수정해야 하는 번거로움을 없애줍니다.
- **`@Builder`와의 시너지 :** `@Builder` 어노테이션은 객체를 생성할 때 모든 필드를 받는 생성자를 필요로 합니다. 이 때 `@AllArgsConstructor`를 함께 사용하면 개발자가 직접 생성자를 작성할 필요 없이 빑더 패턴을 쉽게 구현할 수 있습니다.
