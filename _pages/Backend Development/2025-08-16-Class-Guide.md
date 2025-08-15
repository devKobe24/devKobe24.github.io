---
title: "📚[Backend Development] Java 백엔드 개발자를 위한 클래스 완전 정복!!"
tags:
    - Backend Development
    - Class
date: "2025-08-16"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 🏗️ Java 백엔드 개발자를 위한 클래스 완전 정복 !!

## 🤔 클래스란 무엇인가?

**클래스(Class)는 객체(Object)를 만들어내기 위한 '설계도' 또는 '틀'입니다.**

### 🥮 붕어빵으로 이해하는 클래스
- **붕어빵 틀** = 클래스 (설계도)
- **실제 붕어빵** = 객체 (인스턴스)
- **팥, 슈크림 등** = 필드 (속성)
- **굽기, 포장하기 등** = 메서드 (행위)

```java
// 🏪 Product 클래스 (상품 설계도)
public class Product {
    // 붕어빵의 '맛'처럼 각 상품이 가지는 고유한 속성들
    private String name;      // 상품명
    private int price;        // 가격  
    private int stockQuantity; // 재고수량
}

// 실제 상품 객체들 생성
Product apple = new Product();     // 🍎 사과 객체
Product banana = new Product();    // 🍌 바나나 객체
```

---

## 🧩 클래스의 구성요소

클래스는 크게 **두 가지 핵심 요소**로 이루어져 있습니다.

### 1. 📦 필드 (Fields) - 속성과 상태

**객체가 가질 데이터를 정의하는 부분입니다.**

```java
public class Product {
    // ✅ 필드들 - "이 객체는 어떤 정보를 가지고 있는가?"
    private String name;           // 상품명
    private int price;            // 가격
    private int stockQuantity;    // 재고수량
    private String category;      // 카테고리
    private boolean isActive;     // 판매중 여부
}
```

**특징:**
- 객체의 **상태(State)**를 나타냅니다
- 각 객체마다 **고유한 값**을 가집니다
- **데이터 타입**을 명시해야 합니다

### 2. ⚙️ 메서드 (Methods) - 행위와 기능

**객체가 수행할 수 있는 동작을 정의하는 부분입니다.**

```java
public class Product {
    private String name;
    private int price;
    private int stockQuantity;
    
    // ✅ 메서드들 - "이 객체는 무엇을 할 수 있는가?"
    
    // 재고 감소
    public void decreaseStock(int quantity) {
        if (quantity <= 0) {
            throw new IllegalArgumentException("수량은 양수여야 합니다");
        }
        if (this.stockQuantity < quantity) {
            throw new IllegalStateException("재고가 부족합니다");
        }
        this.stockQuantity -= quantity;
    }
    
    // 가격 변경
    public void changePrice(int newPrice) {
        if (newPrice <= 0) {
            throw new IllegalArgumentException("가격은 0보다 커야 합니다");
        }
        this.price = newPrice;
    }
    
    // 재고 확인
    public boolean hasStock() {
        return this.stockQuantity > 0;
    }
    
    // 상품 정보 조회
    public String getProductInfo() {
        return String.format("상품명: %s, 가격: %d원, 재고: %d개", 
                           name, price, stockQuantity);
    }
}
```

**특징:**
- 객체의 **행동(Behavior)**을 나타냅니다
- 필드 값을 이용해 **비즈니스 로직**을 수행합니다
- **매개변수**와 **반환값**을 가질 수 있습니다

---

## 🎯 언제 클래스를 사용할까?

### ✅ 적합한 상황

1. **동일한 구조의 객체가 여러 개 필요한 경우**
```java
// 🛒 쇼핑몰에서 수많은 상품들을 관리해야 할 때
Product laptop = new Product("노트북", 1500000, 10);
Product mouse = new Product("마우스", 25000, 50);
Product keyboard = new Product("키보드", 80000, 30);
```

2. **현실 세계의 개념을 코드로 표현해야 하는 경우**
```java
// 🏪 전자상거래 도메인 모델링
public class Customer { /* 고객 */ }
public class Order { /* 주문 */ }
public class Payment { /* 결제 */ }
public class Delivery { /* 배송 */ }
```

3. **관련된 데이터와 기능을 묶어서 관리해야 하는 경우**
```java
// 📊 계산기 기능을 하나의 클래스로 묶기
public class Calculator {
    private double result;  // 계산 결과 저장
    
    public void add(double value) { /* 더하기 */ }
    public void subtract(double value) { /* 빼기 */ }
    public double getResult() { /* 결과 조회 */ }
}
```

---

## 🚀 왜 클래스를 사용할까?

### 1. 🔄 코드의 재사용성 (Reusability)

```java
// ❌ 클래스 없이 개발하면...
String product1Name = "노트북";
int product1Price = 1500000;
int product1Stock = 10;

String product2Name = "마우스";
int product2Price = 25000;  
int product2Stock = 50;
// 매번 변수를 반복해서 선언해야 함 😵

// ✅ 클래스를 사용하면!
Product product1 = new Product("노트북", 1500000, 10);
Product product2 = new Product("마우스", 25000, 50);
// 깔끔하고 일관된 구조! 😊
```

### 2. 📂 체계적인 코드 관리 (Organization)

```java
// ✅ Product 관련 모든 것이 한 곳에!
public class Product {
    // 상품 데이터
    private String name;
    private int price;
    
    // 상품 기능
    public void validatePrice() { /* 가격 검증 */ }
    public void applyDiscount() { /* 할인 적용 */ }
    public void updateStock() { /* 재고 업데이트 */ }
}
```

### 3. 🌍 현실 세계 모델링 (Domain Modeling)

```java
// 🏦 은행 시스템 예시
public class Account {
    private String accountNumber;  // 계좌번호
    private long balance;         // 잔액
    
    public void deposit(long amount) {    // 입금
        this.balance += amount;
    }
    
    public void withdraw(long amount) {   // 출금
        if (balance >= amount) {
            this.balance -= amount;
        }
    }
}
```

### 4. 💊 데이터 보호 (캡슐화, Encapsulation)

```java
public class BankAccount {
    private long balance;  // ✅ private으로 직접 접근 차단
    
    // ✅ 안전한 방법으로만 잔액 변경 가능
    public void deposit(long amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("입금액은 양수여야 합니다");
        }
        this.balance += amount;
    }
    
    // ❌ 외부에서 balance에 직접 접근 불가
    // account.balance = -1000000;  // 컴파일 에러!
}
```

---

## 🛠️ 실전 클래스 작성 가이드

### 📋 클래스 설계 체크리스트

```java
// ✅ 좋은 클래스 예시
public class Product {
    // 1. 필드는 private으로 보호
    private ProductId id;
    private String name;
    private Money price;
    private Stock stock;
    
    // 2. 생성자로 필수 데이터 보장
    public Product(ProductId id, String name, Money price) {
        this.id = Objects.requireNonNull(id);
        this.name = validateName(name);
        this.price = Objects.requireNonNull(price);
        this.stock = Stock.zero();
    }
    
    // 3. 비즈니스 로직을 메서드로 표현
    public void changePrice(Money newPrice) {
        if (newPrice.isLessThanOrEqual(Money.zero())) {
            throw new IllegalArgumentException("상품 가격은 0보다 커야 합니다");
        }
        this.price = newPrice;
    }
    
    // 4. 의미있는 메서드명 사용
    public boolean isAvailable() {
        return stock.hasQuantity();
    }
    
    // 5. 필요한 경우에만 getter 제공
    public String getName() {
        return name;
    }
}
```

### ⚠️ 피해야 할 안티패턴

```java
// ❌ 나쁜 클래스 예시
public class Product {
    // 1. 모든 필드가 public (캡슐화 위반)
    public String name;
    public int price;
    public int stock;
    
    // 2. 의미없는 getter/setter만 존재 (빈약한 도메인 모델)
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getPrice() { return price; }
    public void setPrice(int price) { this.price = price; }
    
    // 3. 비즈니스 로직이 없음
    // 실제 상품의 행동이나 규칙이 표현되지 않음
}
```

---

## 🎯 백엔드 개발에서의 클래스 활용

### 🏪 전자상거래 도메인 예시

```java
// 📦 주문 애그리게이트
public class Order {
    private OrderId id;
    private CustomerId customerId;
    private List<OrderItem> items = new ArrayList<>();
    private OrderStatus status;
    private LocalDateTime orderedAt;
    
    public void addItem(Product product, int quantity) {
        validateCanAddItem();
        OrderItem item = new OrderItem(product, quantity);
        items.add(item);
    }
    
    public void confirm() {
        if (status != OrderStatus.PENDING) {
            throw new IllegalStateException("대기 중인 주문만 확정할 수 있습니다");
        }
        this.status = OrderStatus.CONFIRMED;
    }
}

// 💰 결제 서비스
@Service
public class PaymentService {
    
    public PaymentResult processPayment(Order order, PaymentMethod method) {
        validateOrder(order);
        
        Money totalAmount = order.calculateTotal();
        Payment payment = Payment.create(order.getId(), totalAmount, method);
        
        return paymentGateway.process(payment);
    }
}
```

---

## 📚 핵심 용어 정리

| 용어 | 영어 | 설명 | 예시 |
|------|------|------|------|
| 클래스 | Class | 객체를 만들기 위한 설계도 | `public class Product { }` |
| 객체 | Object | 클래스로부터 생성된 실체 | `Product apple = new Product();` |
| 인스턴스 | Instance | 메모리에 할당된 객체 | apple은 Product의 인스턴스 |
| 필드 | Field | 객체의 상태를 나타내는 변수 | `private String name;` |
| 메서드 | Method | 객체의 행동을 나타내는 함수 | `public void changePrice() { }` |
| 생성자 | Constructor | 객체를 초기화하는 특별한 메서드 | `public Product(String name) { }` |
| 캡슐화 | Encapsulation | 데이터와 메서드를 하나로 묶고 보호 | `private` 접근 제어자 사용 |

---

## 🎉 마무리

클래스는 Java 백엔드 개발의 **핵심 기초**입니다!

### 🚦 다음 단계 학습 로드맵
1. **상속 (Inheritance)** - 클래스 간의 관계 이해
2. **다형성 (Polymorphism)** - 같은 메서드, 다른 동작
3. **추상화 (Abstraction)** - 인터페이스와 추상 클래스
4. **컬렉션 (Collections)** - 객체들을 효율적으로 관리
5. **디자인 패턴** - 검증된 설계 해법들

### 💡 실습 추천
- 간단한 쇼핑몰 상품 관리 시스템 만들기
- 은행 계좌 클래스로 입출금 기능 구현하기
- 학생 성적 관리 시스템 설계해보기

**클래스를 정복하면 객체지향 프로그래밍의 문이 활짝 열립니다!** 🚀
