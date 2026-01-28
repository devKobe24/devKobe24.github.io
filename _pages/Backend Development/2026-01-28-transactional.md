---
title: "📚[Backend Development] 🚀 Transactional"
tags:
  - Backend Development
  - Server
  - Java
  - Lombok

date: "2026-01-28"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 🔄 Spring @Transactional 완벽 가이드

> DB 작업의 안전망! 데이터 정합성을 보장하는 트랜잭션의 모든 것

---

## 📋 목차

1. [@Transactional이 뭔가요?](#1-transactional이-뭔가요)
2. [트랜잭션의 핵심 개념](#2-트랜잭션의-핵심-개념)
3. [언제 사용해야 할까?](#3-언제-사용해야-할까)
4. [절대 사용하면 안 되는 경우](#4-절대-사용하면-안-되는-경우)
5. [핵심 옵션 완벽 정리](#5-핵심-옵션-완벽-정리)
6. [실무 패턴과 Best Practices](#6-실무-패턴과-best-practices)
7. [흔한 실수와 해결책](#7-흔한-실수와-해결책)
8. [핵심 요약](#8-핵심-요약)

---

## 1️⃣ @Transactional이 뭔가요?

**@Transactional**은 메서드(또는 클래스)를 하나의 트랜잭션 단위로 묶어주는 Spring의 핵심 애노테이션입니다.

### 💡 한 문장 정의

> "이 메서드 안의 DB 작업들은 **전부 성공**하거나, 하나라도 실패하면 **전부 취소(롤백)**해라"

### 🎬 영화관 예매로 이해하기

```
영화 예매 프로세스:
1. 좌석 선택 ✅
2. 결제 진행 ✅
3. 포인트 차감 ❌ (실패!)

결과:
→ 좌석 선택 취소 (롤백)
→ 결제 취소 (롤백)
→ 모든 것이 처음 상태로!
```

만약 트랜잭션이 없다면?
- 좌석은 예약됨
- 결제는 완료됨
- 하지만 포인트 차감 실패
- **결과: 데이터 불일치! 💥**

---

## 2️⃣ 트랜잭션의 핵심 개념

### 🔹 트랜잭션(Transaction)이란?

**DB에서 말하는 트랜잭션은 논리적 작업의 묶음입니다.**

#### 실제 예시: 온라인 쇼핑몰 주문

```java
@Transactional
public void placeOrder(OrderRequest request) {
    // 1. 주문 저장
    Order order = orderRepository.save(new Order(request));
    
    // 2. 재고 차감
    stockRepository.decreaseStock(request.getProductId(), request.getQuantity());
    
    // 3. 결제 내역 저장
    paymentRepository.save(new Payment(order));
    
    // 4. 포인트 적립
    pointRepository.addPoint(request.getUserId(), order.getPoint());
}
```

#### 트랜잭션이 보장하는 것

```
✅ 모든 작업 성공 → COMMIT (영구 반영)
❌ 하나라도 실패 → ROLLBACK (모두 취소)
```

---

### 🎯 ACID 속성

트랜잭션의 4가지 핵심 속성

| 속성 | 영문 | 의미 | 예시 |
|:---:|:---:|---|---|
| **원자성** | Atomicity | 전부 또는 전무 | 송금: 출금과 입금 둘 다 성공 or 둘 다 실패 |
| **일관성** | Consistency | 규칙 위반 불가 | 잔액은 음수가 될 수 없음 |
| **격리성** | Isolation | 동시 실행 시 영향 없음 | A의 송금이 B의 조회에 영향 주지 않음 |
| **지속성** | Durability | 영구 보존 | COMMIT 후 시스템 장애 발생해도 데이터 유지 |

### 📊 트랜잭션 생명주기

```
[시작]
   ↓
@Transactional 메서드 호출
   ↓
트랜잭션 시작 (BEGIN)
   ↓
비즈니스 로직 실행
   ↓
   ├─→ 성공 → COMMIT → [DB 반영]
   └─→ 실패 → ROLLBACK → [원상 복구]
```

---

## 3️⃣ 언제 사용해야 할까?

### ✅ Case 1: 여러 DB 작업이 "한 세트"일 때 (가장 중요!)

#### 예시 1: 계좌 이체

```java
@Service
@RequiredArgsConstructor
public class TransferService {
    
    private final AccountRepository accountRepository;
    
    @Transactional
    public void transfer(Long fromId, Long toId, BigDecimal amount) {
        // 1. 출금 계좌에서 차감
        Account from = accountRepository.findById(fromId).orElseThrow();
        from.withdraw(amount);  // UPDATE
        
        // 2. 입금 계좌에 추가
        Account to = accountRepository.findById(toId).orElseThrow();
        to.deposit(amount);     // UPDATE
        
        // 3. 거래 내역 저장
        transactionRepository.save(new Transaction(from, to, amount));  // INSERT
    }
}
```

**트랜잭션 없이 실행하면?**

| 상황 | 결과 | 문제점 |
|---|---|---|
| 출금 성공, 입금 실패 | 돈이 사라짐 💸 | 치명적! |
| 출금 성공, 내역 저장 실패 | 추적 불가 📉 | 감사 실패 |
| 입금 성공, 출금 실패 | 돈이 생김 💰 | 무결성 위반 |

**트랜잭션으로 보호하면?**
- 하나라도 실패 → 모두 롤백
- 데이터 정합성 보장! ✅

---

#### 예시 2: 주문 처리

```java
@Service
@RequiredArgsConstructor
public class OrderService {
    
    @Transactional
    public OrderResponse createOrder(OrderRequest request) {
        // 1. 주문 생성
        Order order = Order.create(request);
        orderRepository.save(order);
        
        // 2. 재고 차감
        for (OrderItem item : request.getItems()) {
            Product product = productRepository.findById(item.getProductId())
                .orElseThrow(() -> new ProductNotFoundException());
            
            if (product.getStock() < item.getQuantity()) {
                throw new OutOfStockException();  // 여기서 예외 발생 시
            }
            
            product.decreaseStock(item.getQuantity());
        }
        
        // 3. 결제 정보 저장
        Payment payment = Payment.create(order, request.getPaymentMethod());
        paymentRepository.save(payment);
        
        // 4. 사용자 포인트 차감
        User user = userRepository.findById(request.getUserId()).orElseThrow();
        user.usePoint(request.getUsedPoint());
        
        return OrderResponse.from(order);
    }
}
```

**보장되는 것:**
- 재고 부족 시 → 주문, 결제, 포인트 차감 모두 취소
- 결제 실패 시 → 주문, 재고 차감 모두 취소
- 데이터 일관성 유지!

---

### ✅ Case 2: 데이터 변경 작업 (CUD)

| 작업 | SQL | 트랜잭션 필요 | 이유 |
|:---:|:---:|:---:|---|
| **조회** | SELECT | ❌ | 데이터 변경 없음 (단, Lock 필요시 ✅) |
| **생성** | INSERT | ✅ | 롤백 보장 필요 |
| **수정** | UPDATE | ✅ | 원자성 보장 필요 |
| **삭제** | DELETE | ✅ | 복구 가능성 필요 |

#### 예시: 사용자 정보 수정

```java
@Service
public class UserService {
    
    // ❌ 트랜잭션 없는 경우
    public void updateUserInfo(Long userId, UserUpdateRequest request) {
        User user = userRepository.findById(userId).orElseThrow();
        user.updateEmail(request.getEmail());        // DB 반영 안 됨!
        user.updatePhoneNumber(request.getPhone());  // DB 반영 안 됨!
    }
    
    // ✅ 트랜잭션 있는 경우
    @Transactional
    public void updateUserInfo(Long userId, UserUpdateRequest request) {
        User user = userRepository.findById(userId).orElseThrow();
        user.updateEmail(request.getEmail());        // 변경 감지
        user.updatePhoneNumber(request.getPhone());  // 변경 감지
        // 메서드 종료 시 자동 UPDATE!
    }
}
```

---

### ✅ Case 3: JPA 변경 감지(Dirty Checking) 사용 시

#### 🎯 변경 감지의 마법

```java
@Service
@RequiredArgsConstructor
public class MemberService {
    
    private final MemberRepository memberRepository;
    
    @Transactional  // 이게 없으면 변경 감지 작동 안 함!
    public void updateMemberName(Long id, String newName) {
        // 1. 조회
        Member member = memberRepository.findById(id).orElseThrow();
        
        // 2. 변경 (save() 호출 안 함!)
        member.changeName(newName);
        
        // 3. 메서드 종료
        // → JPA가 자동으로 UPDATE 쿼리 실행!
    }
}
```

#### 🔄 동작 과정

```
1. @Transactional 시작
2. Member 조회 (영속성 컨텍스트에 저장)
3. changeName() 호출 (객체 상태만 변경)
4. 트랜잭션 커밋 직전
   → JPA가 스냅샷 비교
   → 변경 감지
   → UPDATE 쿼리 자동 생성
   → DB에 반영
5. 트랜잭션 종료 (COMMIT)
```

> 💡 **핵심**: `@Transactional` 없으면 변경 사항이 DB에 반영되지 않습니다!

---

### ✅ Case 4: 예외 발생 시 자동 롤백

#### 기본 롤백 규칙

| 예외 타입 | 롤백 여부 | 예시 |
|:---:|:---:|---|
| **RuntimeException** | ✅ 자동 롤백 | `IllegalArgumentException`, `NullPointerException` |
| **Error** | ✅ 자동 롤백 | `OutOfMemoryError` |
| **Checked Exception** | ❌ 롤백 안 됨 | `IOException`, `SQLException` |

#### 예시: 자동 롤백

```java
@Service
public class OrderService {
    
    @Transactional
    public void processOrder(Long orderId) {
        // 1. 주문 조회
        Order order = orderRepository.findById(orderId).orElseThrow();
        
        // 2. 재고 확인 및 차감
        Stock stock = stockRepository.findByProductId(order.getProductId());
        if (stock.getQuantity() < order.getQuantity()) {
            throw new OutOfStockException("재고 부족!");  // RuntimeException
            // → 자동 롤백!
        }
        stock.decrease(order.getQuantity());
        
        // 3. 결제 처리
        payment.process(order);
        
        // 4. 주문 상태 변경
        order.complete();
    }
}
```

**실행 흐름:**
```
주문 조회 ✅
재고 확인 ✅
재고 부족 발견 💥
OutOfStockException 발생
→ 자동 ROLLBACK
→ 모든 변경 사항 취소!
```

---

## 4️⃣ 절대 사용하면 안 되는 경우

### ❌ Case 1: Controller 계층에 남발

#### 잘못된 예시

```java
@RestController
@RequestMapping("/api/orders")
@Transactional  // ❌ Controller에 트랜잭션!
public class OrderController {
    
    private final OrderService orderService;
    
    @PostMapping
    public ResponseEntity<OrderResponse> createOrder(@RequestBody OrderRequest request) {
        OrderResponse response = orderService.createOrder(request);
        return ResponseEntity.ok(response);
    }
}
```

#### 문제점

| 문제 | 설명 |
|---|---|
| **책임 위반** | Controller는 요청/응답 변환만 담당해야 함 |
| **트랜잭션 범위 과다** | HTTP 통신 전체가 트랜잭션에 포함됨 |
| **성능 저하** | 불필요하게 DB 커넥션 점유 시간 증가 |
| **테스트 어려움** | 트랜잭션 경계가 모호해짐 |

#### 올바른 구조

```java
// ✅ Controller: 트랜잭션 없음
@RestController
@RequestMapping("/api/orders")
@RequiredArgsConstructor
public class OrderController {
    
    private final OrderService orderService;
    
    @PostMapping
    public ResponseEntity<OrderResponse> createOrder(@RequestBody OrderRequest request) {
        // 요청/응답 변환만 담당
        OrderResponse response = orderService.createOrder(request);
        return ResponseEntity.ok(response);
    }
}

// ✅ Service: 트랜잭션 처리
@Service
@RequiredArgsConstructor
public class OrderService {
    
    @Transactional
    public OrderResponse createOrder(OrderRequest request) {
        // 비즈니스 로직 + DB 작업
        // ...
    }
}
```

### 🏗️ 권장 계층 구조

```
[Client]
    ↓
[Controller] ← 트랜잭션 ❌
    ↓
[Service] ← @Transactional ✅
    ↓
[Repository] ← 트랜잭션 전파
    ↓
[Database]
```

---

### ❌ Case 2: 단순 조회 메서드

#### 잘못된 예시

```java
@Service
public class MemberService {
    
    // ❌ 단순 조회에 기본 트랜잭션
    @Transactional
    public List<Member> findAllMembers() {
        return memberRepository.findAll();
    }
    
    // ❌ 단순 조회에 기본 트랜잭션
    @Transactional
    public Member findMember(Long id) {
        return memberRepository.findById(id).orElseThrow();
    }
}
```

#### 문제점

- **불필요한 쓰기 Lock**: 데이터 변경이 없는데 Lock 획득
- **성능 저하**: 트랜잭션 관리 오버헤드
- **동시성 감소**: 다른 트랜잭션 대기 발생 가능

#### 올바른 사용

```java
@Service
public class MemberService {
    
    // ✅ 조회 전용 트랜잭션
    @Transactional(readOnly = true)
    public List<Member> findAllMembers() {
        return memberRepository.findAll();
    }
    
    // ✅ 조회 전용 트랜잭션
    @Transactional(readOnly = true)
    public Member findMember(Long id) {
        return memberRepository.findById(id).orElseThrow();
    }
    
    // ✅ 변경 작업은 기본 트랜잭션
    @Transactional
    public void updateMember(Long id, MemberUpdateRequest request) {
        Member member = memberRepository.findById(id).orElseThrow();
        member.update(request);
    }
}
```

### 📊 readOnly 효과

| 항목 | readOnly = false | readOnly = true |
|:---:|:---:|:---:|
| **Flush 모드** | AUTO | MANUAL |
| **스냅샷 저장** | ✅ | ❌ |
| **변경 감지** | ✅ | ❌ |
| **성능** | 보통 | 향상 ⚡ |
| **용도** | CUD 작업 | 조회 작업 |

---

### ❌ Case 3: Private 메서드

#### 작동하지 않는 예시

```java
@Service
public class MemberService {
    
    public void publicMethod() {
        // private 메서드 호출
        privateTransactionalMethod();  // 트랜잭션 적용 안 됨! ❌
    }
    
    @Transactional  // 동작 안 함!
    private void privateTransactionalMethod() {
        // DB 작업...
    }
}
```

#### 이유

Spring AOP는 **프록시 기반**으로 동작합니다.

```
실제 객체: MemberService
    ↓
프록시 객체: MemberService$$Proxy
    ↓ (public 메서드만 가로챔)
@Transactional 적용
```

**Private 메서드는 프록시가 가로챌 수 없음!**

#### 해결 방법

```java
@Service
public class MemberService {
    
    // ✅ public으로 변경
    @Transactional
    public void transactionalMethod() {
        // DB 작업...
    }
}
```

---

### ❌ Case 4: 같은 클래스 내부 호출

#### 작동하지 않는 예시

```java
@Service
public class MemberService {
    
    public void outerMethod() {
        // 같은 클래스의 @Transactional 메서드 호출
        innerTransactionalMethod();  // 트랜잭션 적용 안 됨! ❌
    }
    
    @Transactional
    public void innerTransactionalMethod() {
        // DB 작업...
    }
}
```

#### 이유

내부 호출은 프록시를 거치지 않고 직접 호출됩니다.

```
[Client] → [Proxy] → outerMethod()
                         ↓ (직접 호출, 프록시 우회!)
                    innerTransactionalMethod()
```

#### 해결 방법 1: 메서드 분리

```java
// ✅ 별도 Service로 분리
@Service
@RequiredArgsConstructor
public class MemberService {
    
    private final MemberTransactionService transactionService;
    
    public void outerMethod() {
        transactionService.innerTransactionalMethod();  // 프록시 통과!
    }
}

@Service
public class MemberTransactionService {
    
    @Transactional
    public void innerTransactionalMethod() {
        // DB 작업...
    }
}
```

#### 해결 방법 2: Self-Injection

```java
@Service
@RequiredArgsConstructor
public class MemberService {
    
    private final MemberService self;  // Self-Injection
    
    public void outerMethod() {
        self.innerTransactionalMethod();  // 프록시 통과!
    }
    
    @Transactional
    public void innerTransactionalMethod() {
        // DB 작업...
    }
}
```

---

## 5️⃣ 핵심 옵션 완벽 정리

### 🎯 주요 옵션 한눈에 보기

```java
@Transactional(
    readOnly = true,                          // 조회 최적화
    timeout = 30,                             // 타임아웃 (초)
    rollbackFor = Exception.class,            // 롤백 대상 예외
    noRollbackFor = BusinessException.class,  // 롤백 제외 예외
    propagation = Propagation.REQUIRED,       // 전파 레벨
    isolation = Isolation.DEFAULT             // 격리 수준
)
```

---

### 1️⃣ readOnly (읽기 전용)

#### 설정

```java
@Transactional(readOnly = true)
public Member findMember(Long id) {
    return memberRepository.findById(id).orElseThrow();
}
```

#### 효과

| 항목 | 효과 |
|---|---|
| **Flush 모드** | MANUAL로 변경 (자동 Flush 안 함) |
| **스냅샷 비교** | 생략 (메모리 절약) |
| **변경 감지** | 비활성화 |
| **DB 힌트** | SELECT ... FOR SHARE (읽기 Lock) |
| **성능** | 10-20% 향상 |

#### 실무 사용

```java
@Service
@Transactional(readOnly = true)  // 클래스 레벨: 기본값
public class MemberService {
    
    // 조회 메서드들은 readOnly 상속
    public Member findMember(Long id) { ... }
    public List<Member> findAll() { ... }
    
    // 변경 메서드만 오버라이드
    @Transactional  // readOnly = false
    public void updateMember(Long id, MemberUpdateRequest request) { ... }
}
```

---

### 2️⃣ rollbackFor / noRollbackFor (롤백 규칙)

#### 기본 동작

```java
@Transactional
public void process() {
    // RuntimeException → 롤백 ✅
    // Checked Exception → 롤백 ❌
}
```

#### Checked Exception도 롤백

```java
@Transactional(rollbackFor = Exception.class)
public void processWithFile() throws IOException {
    // ...
    throw new IOException("파일 오류");  // 롤백됨!
}
```

#### 특정 예외는 롤백 제외

```java
@Transactional(noRollbackFor = BusinessException.class)
public void processOrder() {
    // ...
    throw new BusinessException("비즈니스 예외");  // 롤백 안 함!
}
```

#### 실무 패턴

```java
@Transactional(
    rollbackFor = Exception.class,          // 모든 예외 롤백
    noRollbackFor = AlreadyProcessedException.class  // 이미 처리된 경우 제외
)
public void processPayment(PaymentRequest request) {
    // ...
}
```

---

### 3️⃣ propagation (전파 레벨)

트랜잭션 메서드가 또 다른 트랜잭션 메서드를 호출할 때의 동작 방식

#### 주요 전파 레벨

| 전파 레벨 | 설명 | 사용 케이스 |
|:---:|---|---|
| **REQUIRED** (기본) | 기존 트랜잭션 사용, 없으면 새로 생성 | 일반적인 경우 |
| **REQUIRES_NEW** | 항상 새 트랜잭션 생성 | 독립적인 작업 |
| **SUPPORTS** | 트랜잭션 있으면 참여, 없어도 OK | 조회 작업 |
| **MANDATORY** | 트랜잭션 필수 (없으면 예외) | 엄격한 검증 |
| **NOT_SUPPORTED** | 트랜잭션 없이 실행 | 외부 시스템 호출 |
| **NEVER** | 트랜잭션 있으면 예외 | 트랜잭션 금지 |
| **NESTED** | 중첩 트랜잭션 생성 | 부분 롤백 |

#### REQUIRED (기본값)

```java
@Transactional
public void outerMethod() {
    // 트랜잭션 A 시작
    innerMethod();  // 트랜잭션 A 사용
    // 트랜잭션 A 종료
}

@Transactional(propagation = Propagation.REQUIRED)
public void innerMethod() {
    // 기존 트랜잭션 A 참여
}
```

#### REQUIRES_NEW (새 트랜잭션)

```java
@Transactional
public void outerMethod() {
    // 트랜잭션 A 시작
    
    try {
        innerMethod();  // 트랜잭션 B 생성 (독립)
    } catch (Exception e) {
        // 트랜잭션 B 롤백
        // 트랜잭션 A는 영향 없음!
    }
    
    // 트랜잭션 A 커밋
}

@Transactional(propagation = Propagation.REQUIRES_NEW)
public void innerMethod() {
    // 새로운 트랜잭션 B 시작
    // 트랜잭션 A와 독립적
}
```

#### 실무 예시: 로그 저장

```java
@Service
@RequiredArgsConstructor
public class OrderService {
    
    private final OrderRepository orderRepository;
    private final AuditLogService auditLogService;
    
    @Transactional
    public void processOrder(OrderRequest request) {
        // 주문 처리
        Order order = orderRepository.save(new Order(request));
        
        try {
            // 감사 로그 저장 (별도 트랜잭션)
            auditLogService.saveLog(order);  // 주문 실패해도 로그는 저장!
        } catch (Exception e) {
            // 로그 저장 실패해도 주문은 성공
            log.error("로그 저장 실패", e);
        }
    }
}

@Service
public class AuditLogService {
    
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void saveLog(Order order) {
        // 독립적인 트랜잭션으로 로그 저장
        auditLogRepository.save(new AuditLog(order));
    }
}
```

---

### 4️⃣ isolation (격리 수준)

동시에 실행되는 여러 트랜잭션 간의 격리 정도

#### 격리 수준 비교

| 격리 수준 | Dirty Read | Non-Repeatable Read | Phantom Read | 성능 |
|:---:|:---:|:---:|:---:|:---:|
| **READ_UNCOMMITTED** | ⚠️ 발생 | ⚠️ 발생 | ⚠️ 발생 | 최고 ⚡⚡⚡ |
| **READ_COMMITTED** | ✅ 방지 | ⚠️ 발생 | ⚠️ 발생 | 높음 ⚡⚡ |
| **REPEATABLE_READ** | ✅ 방지 | ✅ 방지 | ⚠️ 발생 | 보통 ⚡ |
| **SERIALIZABLE** | ✅ 방지 | ✅ 방지 | ✅ 방지 | 낮음 🐢 |

#### 문제 유형 설명

**Dirty Read (더티 리드)**
```
T1: 데이터 수정 (커밋 전)
T2: 수정된 데이터 읽음
T1: 롤백
T2: 잘못된 데이터 사용! ❌
```

**Non-Repeatable Read (반복 불가능 읽기)**
```
T1: 데이터 읽음 (값: 100)
T2: 데이터 수정 및 커밋 (값: 200)
T1: 같은 데이터 다시 읽음 (값: 200)
T1: 값이 바뀜! ❌
```

**Phantom Read (팬텀 리드)**
```
T1: 조건 검색 (결과: 10건)
T2: 새 데이터 INSERT 및 커밋
T1: 같은 조건 검색 (결과: 11건)
T1: 결과 개수가 바뀜! ❌
```

#### 실무 사용

```java
@Service
public class StockService {
    
    // 재고 조회 및 차감 (동시성 제어 필요)
    @Transactional(isolation = Isolation.REPEATABLE_READ)
    public void decreaseStock(Long productId, int quantity) {
        Product product = productRepository.findById(productId).orElseThrow();
        
        if (product.getStock() < quantity) {
            throw new OutOfStockException();
        }
        
        product.decreaseStock(quantity);
    }
}
```

---

### 5️⃣ timeout (타임아웃)

#### 설정

```java
@Transactional(timeout = 30)  // 30초
public void longRunningTask() {
    // 30초 이상 걸리면 예외 발생
}
```

#### 실무 사용

```java
@Service
public class BatchService {
    
    @Transactional(timeout = 300)  // 5분
    public void processBatch() {
        // 대량 데이터 처리
        // 5분 초과 시 TransactionTimedOutException
    }
}
```

---

## 6️⃣ 실무 패턴과 Best Practices

### 🌟 패턴 1: 클래스 레벨 + 메서드 레벨 조합

```java
@Service
@Transactional(readOnly = true)  // 기본값: 조회 최적화
@RequiredArgsConstructor
public class MemberService {
    
    private final MemberRepository memberRepository;
    
    // readOnly 상속 (조회)
    public Member findMember(Long id) {
        return memberRepository.findById(id).orElseThrow();
    }
    
    // readOnly 상속 (조회)
    public List<Member> findAllMembers() {
        return memberRepository.findAll();
    }
    
    // 오버라이드: 변경 작업
    @Transactional
    public void createMember(MemberCreateRequest request) {
        Member member = Member.create(request);
        memberRepository.save(member);
    }
    
    // 오버라이드: 변경 작업
    @Transactional
    public void updateMember(Long id, MemberUpdateRequest request) {
        Member member = memberRepository.findById(id).orElseThrow();
        member.update(request);
    }
    
    // 오버라이드: 삭제 작업
    @Transactional
    public void deleteMember(Long id) {
        memberRepository.deleteById(id);
    }
}
```

**장점:**
- ✅ 중복 코드 감소
- ✅ 조회 메서드 성능 최적화
- ✅ 변경 메서드만 명시적 표시

---

### 🌟 패턴 2: 커스텀 애노테이션

#### 정의

```java
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Transactional(readOnly = true)
public @interface ReadOnlyTransaction {
}

@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Transactional(
    rollbackFor = Exception.class,
    timeout = 30
)
public @interface WriteTransaction {
}
```

#### 사용

```java
@Service
public class ProductService {
    
    @ReadOnlyTransaction
    public List<Product> findAllProducts() {
        return productRepository.findAll();
    }
    
    @WriteTransaction
    public void createProduct(ProductRequest request) {
        productRepository.save(new Product(request));
    }
}
```

**장점:**
- ✅ 의도 명확화
- ✅ 일관된 설정
- ✅ 유지보수 편의

---

### 🌟 패턴 3: 트랜잭션 템플릿

#### 프로그래매틱 트랜잭션

```java
@Service
@RequiredArgsConstructor
public class PaymentService {
    
    private final TransactionTemplate transactionTemplate;
    
    public PaymentResult processPayment(PaymentRequest request) {
        return transactionTemplate.execute(status -> {
            try {
                // 비즈니스 로직
                Payment payment = createPayment(request);
                
                if (payment.isFailed()) {
                    status.setRollbackOnly();  // 수동 롤백
                    return PaymentResult.failed();
                }
                
                return PaymentResult.success(payment);
                
            } catch (Exception e) {
                status.setRollbackOnly();
                throw e;
            }
        });
    }
}
```

**사용 케이스:**
- 조건부 롤백 필요 시
- 트랜잭션 범위를 코드로 제어해야 할 때

---

### 🌟 패턴 4: 이벤트 기반 트랜잭션

```java
@Service
@RequiredArgsConstructor
public class OrderService {
    
    private final ApplicationEventPublisher eventPublisher;
    
    @Transactional
    public void createOrder(OrderRequest request) {
        Order order = orderRepository.save(new Order(request));
        
        // 이벤트 발행 (트랜잭션 커밋 후 처리)
        eventPublisher.publishEvent(new OrderCreatedEvent(order));
    }
}

@Component
@RequiredArgsConstructor
public class OrderEventListener {
    
    private final EmailService emailService;
    
    @TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
    public void handleOrderCreated(OrderCreatedEvent event) {
        // 주문 완료 후에만 실행
        emailService.sendOrderConfirmation(event.getOrder());
    }
}
```

---

## 7️⃣ 흔한 실수와 해결책

### ⚠️ 실수 1: 트랜잭션 범위 내에서 외부 API 호출

#### 문제 코드

```java
@Transactional
public void processOrder(OrderRequest request) {
    Order order = orderRepository.save(new Order(request));
    
    // 외부 API 호출 (느림!)
    ExternalPaymentResponse response = paymentApiClient.charge(order);
    
    // DB 커넥션을 오래 점유!
    order.updatePaymentInfo(response);
}
```

**문제점:**
- DB 커넥션을 외부 API 응답까지 점유
- 타임아웃 위험
- 동시 처리량 감소

#### 해결책

```java
@Transactional
public void processOrder(OrderRequest request) {
    // 1. DB 작업만 먼저
    Order order = orderRepository.save(new Order(request));
}

// 2. 외부 API는 별도 메서드
public void chargePayment(Long orderId) {
    ExternalPaymentResponse response = paymentApiClient.charge(orderId);
    updatePaymentInfo(orderId, response);
}

@Transactional
public void updatePaymentInfo(Long orderId, ExternalPaymentResponse response) {
    Order order = orderRepository.findById(orderId).orElseThrow();
    order.updatePaymentInfo(response);
}
```

---

### ⚠️ 실수 2: 트랜잭션 내에서 예외를 잡아서 처리

#### 문제 코드

```java
@Transactional
public void processOrder(OrderRequest request) {
    try {
        orderRepository.save(new Order(request));
        
        // 예외 발생
        throw new RuntimeException("에러!");
        
    } catch (Exception e) {
        log.error("에러 발생", e);
        // 예외를 먹어버림 → 롤백 안 됨!
    }
}
```

**문제점:**
- 예외를 catch하면 Spring이 롤백을 인지 못함
- 데이터 정합성 깨짐

#### 해결책 1: 예외 다시 던지기

```java
@Transactional
public void processOrder(OrderRequest request) {
    try {
        orderRepository.save(new Order(request));
        throw new RuntimeException("에러!");
    } catch (Exception e) {
        log.error("에러 발생", e);
        throw e;  // 다시 던지기!
    }
}
```

#### 해결책 2: setRollbackOnly

```java
@Transactional
public void processOrder(OrderRequest request) {
    try {
        orderRepository.save(new Order(request));
        throw new RuntimeException("에러!");
    } catch (Exception e) {
        log.error("에러 발생", e);
        TransactionAspectSupport.currentTransactionStatus()
            .setRollbackOnly();  // 수동 롤백 마킹
    }
}
```

---

### ⚠️ 실수 3: 대용량 데이터 처리

#### 문제 코드

```java
@Transactional
public void processAllMembers() {
    List<Member> members = memberRepository.findAll();  // 100만 건!
    
    for (Member member : members) {
        member.process();
        // 메모리 부족! OutOfMemoryError
    }
}
```

#### 해결책: 페이징 또는 스트림

```java
@Transactional(readOnly = true)
public void processAllMembers() {
    int pageSize = 1000;
    int page = 0;
    
    while (true) {
        List<Member> members = memberRepository.findAll(
            PageRequest.of(page, pageSize)
        ).getContent();
        
        if (members.isEmpty()) break;
        
        // 별도 트랜잭션으로 처리
        processMembers(members);
        
        page++;
    }
}

@Transactional(propagation = Propagation.REQUIRES_NEW)
public void processMembers(List<Member> members) {
    for (Member member : members) {
        member.process();
    }
}
```

---

## 8️⃣ 핵심 요약

### 💡 한 문장 정리

> **@Transactional은 DB 작업을 하나의 안전한 묶음으로 처리하여 데이터 정합성을 보장하는 필수 애노테이션이다.**

---

### 📌 기억해야 할 핵심

| 항목 | 내용 |
|:---:|---|
| **사용 위치** | Service 계층 |
| **주요 용도** | 데이터 변경 작업 (CUD) |
| **롤백 규칙** | RuntimeException → 자동 롤백 |
| **조회 최적화** | `readOnly = true` 사용 |
| **JPA 필수** | 변경 감지를 위해 필수 |

---

### 🎓 실무 체크리스트

#### 기본 사용
- [ ] Service 계층에 `@Transactional` 적용
- [ ] Controller에는 사용하지 않음
- [ ] 조회 메서드는 `readOnly = true`
- [ ] 변경 메서드는 기본 설정

#### 고급 설정
- [ ] 클래스 레벨 + 메서드 레벨 조합 사용
- [ ] 적절한 전파 레벨 선택
- [ ] 격리 수준 이해 및 설정
- [ ] 타임아웃 설정 (장기 작업)

#### 주의사항
- [ ] Private 메서드에 사용하지 않음
- [ ] 같은 클래스 내부 호출 피하기
- [ ] 예외를 catch할 때 다시 던지기
- [ ] 외부 API 호출은 트랜잭션 밖에서
- [ ] 대용량 데이터는 페이징 처리

---

### 📊 상황별 사용 가이드

```
단순 조회 → @Transactional(readOnly = true)
데이터 변경 → @Transactional
예외 시 롤백 → @Transactional (기본)
체크 예외 롤백 → @Transactional(rollbackFor = Exception.class)
독립 트랜잭션 → @Transactional(propagation = REQUIRES_NEW)
동시성 제어 → @Transactional(isolation = REPEATABLE_READ)
```

---

## 🚀 다음 학습 주제

- JPA 영속성 컨텍스트와 1차 캐시
- 낙관적 락(Optimistic Lock) vs 비관적 락(Pessimistic Lock)
- 분산 트랜잭션과 2PC(Two-Phase Commit)
- Spring Batch와 트랜잭션 관리
- 이벤트 기반 아키텍처와 Saga 패턴

---

## 💬 FAQ

<details>
<summary><strong>Q1. @Transactional을 인터페이스에 붙여도 되나요?</strong></summary>

**가능하지만 권장하지 않습니다.**

```java
// ❌ 권장하지 않음
public interface MemberService {
    @Transactional
    void updateMember(Long id, String name);
}

// ✅ 권장
@Service
public class MemberServiceImpl implements MemberService {
    
    @Transactional
    @Override
    public void updateMember(Long id, String name) {
        // ...
    }
}
```

**이유:**
- JDK Dynamic Proxy 사용 시 작동하지 않을 수 있음
- CGLIB Proxy에서만 작동
- 명시성이 떨어짐

**결론:** 구현 클래스에 직접 붙이는 것이 안전합니다.
</details>

<details>
<summary><strong>Q2. @Transactional 메서드에서 또 다른 @Transactional 메서드를 호출하면?</strong></summary>

**기본적으로는 하나의 트랜잭션으로 묶입니다. (REQUIRED 전파)**

```java
@Transactional
public void outer() {
    inner();  // 같은 트랜잭션 사용
}

@Transactional
public void inner() {
    // outer와 같은 트랜잭션
}
```

**독립적인 트랜잭션이 필요하면:**

```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void inner() {
    // 새로운 트랜잭션 시작
}
```
</details>

<details>
<summary><strong>Q3. 트랜잭션 내에서 비동기 메서드를 호출하면?</strong></summary>

**비동기 메서드는 별도 스레드에서 실행되므로 트랜잭션이 전파되지 않습니다.**

```java
@Transactional
public void processOrder() {
    orderRepository.save(order);
    
    asyncService.sendEmail();  // 별도 스레드, 별도 트랜잭션
}

@Async
@Transactional
public void sendEmail() {
    // 완전히 독립적인 트랜잭션
}
```

**주의:** 비동기 메서드에서 트랜잭션이 필요하면 명시적으로 `@Transactional`을 붙여야 합니다.
</details>

<details>
<summary><strong>Q4. readOnly = true일 때 데이터를 수정하면?</strong></summary>

**JPA를 사용하는 경우:**

```java
@Transactional(readOnly = true)
public void updateMember(Long id, String name) {
    Member member = memberRepository.findById(id).orElseThrow();
    member.setName(name);  // 변경 감지 안 됨!
}
```

- 변경 감지(Dirty Checking)가 작동하지 않음
- DB에 UPDATE 쿼리가 전송되지 않음
- 데이터 변경 안 됨

**JDBC Template 사용하는 경우:**
- DB에 따라 다름
- MySQL: 일부 스토리지 엔진에서 예외 발생
- PostgreSQL: 예외 발생

**결론:** readOnly는 조회 전용으로만 사용하세요!
</details>

<details>
<summary><strong>Q5. 트랜잭션 시작 시점과 종료 시점은?</strong></summary>

**시작 시점:**
- `@Transactional` 메서드 진입 직후
- 실제 DB 작업 전

**종료 시점:**
- 메서드 정상 종료 시 → COMMIT
- 예외 발생 시 → ROLLBACK
- 메서드 return 직전

```java
@Transactional
public void example() {
    System.out.println("1. 트랜잭션 시작됨");
    
    repository.save(entity);
    System.out.println("2. 아직 COMMIT 안 됨");
    
    return;  // 3. 여기서 COMMIT 또는 ROLLBACK
}
System.out.println("4. 트랜잭션 종료됨");
```
</details>

---

## 📚 참고 자료

### 공식 문서
- [Spring Framework - Transaction Management](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction)
- [Spring Data JPA - Transactions](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#transactions)

### 추천 강의
- 인프런: "스프링 DB 1편 - 데이터 접근 핵심 원리" (김영한)
- 인프런: "스프링 DB 2편 - 데이터 접근 활용 기술" (김영한)

### 추천 도서
- "자바 ORM 표준 JPA 프로그래밍" - 김영한
- "스프링 5 레시피" - 마틴 데니엄, 다니엘 루비오, 조시 롱

---

**🎉 이제 @Transactional을 완벽하게 이해하고 사용할 수 있습니다!**

> 💡 **마지막 조언**: 트랜잭션은 데이터 정합성의 핵심입니다. 항상 신중하게 사용하고, 테스트를 통해 검증하세요!
