---
title: "📚[Backend Development] 🎯 SOLID 원칙 - 단일 책임 원칙(SRP) 트러블슈팅 가이드"
tags:
    - Backend Development
    - Spring Boot
    - Trouble Shooting
    - SRP
    - SOLID
date: "2025-08-23"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 🎯 SOLID 원칙 - 단일 책임 원칙(SRP) 트러블슈팅 가이드

Spring Boot 개발에서 자주 발생하는 SRP(Single Responsibility Principle) 위반 사례와 해결 방법을 정리했습니다.

---

## 🔍 문제 1: 하나의 서비스가 너무 많은 일을 담당

### 📋 에러 상황
하나의 서비스 클래스에서 여러 가지 책임을 동시에 처리하는 코드를 발견했습니다.

```java
@Service
public class BadOrderService {
    private final OrderRepository orderRepository;
    private final JavaMailSender mailSender;
    
    public Order createOrder(OrderRequest request) {
        // 주문 처리 + 이메일 발송 + 로깅 + 검증... 😵
        Order order = new Order(request.getProductId(), request.getQuantity());
        Order savedOrder = orderRepository.save(order);
        
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(request.getUserEmail());
        mailSender.send(message);
        
        return savedOrder;
    }
}
```

### 🎯 원인 분석
**하나의 클래스가 여러 책임을 가지면서 SRP(Single Responsibility Principle)를 위반했습니다.**

1. **변경의 이유가 여러 개**: 주문 로직 변경, 이메일 템플릿 변경 등
2. **높은 결합도**: 이메일 시스템 장애가 주문 시스템에 영향
3. **테스트 복잡성**: 여러 의존성을 모두 모킹해야 함

### 🔧 해결 방법

#### 1단계: 책임별로 클래스 분리

```java
// ✅ 주문 처리 책임만 담당
@Service
public class OrderService {
    private final OrderRepository orderRepository;
    private final NotificationService notificationService; // 🎯 알림은 다른 서비스에 위임
    
    public Order createOrder(OrderRequest request) {
        // 1️⃣ 주문 생성과 저장에만 집중
        Order order = new Order(request.getProductId(), request.getQuantity());
        Order savedOrder = orderRepository.save(order);
        
        // 2️⃣ 알림 발송은 전문 서비스에 위임
        notificationService.sendOrderCompletionNotification(savedOrder, request.getUserEmail());
        
        return savedOrder;
    }
}
```

```java
// ✅ 알림 발송 책임만 담당
@Service
public class NotificationService {
    private final JavaMailSender mailSender;
    
    public void sendOrderCompletionNotification(Order order, String userEmail) {
        // 🎯 알림 발송에만 집중
        try {
            SimpleMailMessage message = new SimpleMailMessage();
            message.setTo(userEmail);
            message.setSubject("주문이 완료되었습니다.");
            message.setText("주문 번호: " + order.getId());
            mailSender.send(message);
        } catch (MailException e) {
            // 🚨 실패 처리 로직
            System.err.println("이메일 발송 실패: " + e.getMessage());
        }
    }
}
```

#### 2단계: 각 클래스의 단일 책임 확인

**변경의 이유 분석**
| 클래스 | 변경 이유 | 책임 |
|--------|-----------|------|
| `OrderService` | 주문 정책 변경 | 주문 생성, 저장, 관리 |
| `NotificationService` | 알림 방식 변경 | 이메일, SMS 등 알림 발송 |

### 📚 SRP 핵심 개념

| 원칙 | 설명 | 혜택 |
|------|------|------|
| **Single Responsibility** | 하나의 클래스는 하나의 책임만 | 높은 응집도, 낮은 결합도 |
| **One Reason to Change** | 변경의 이유가 오직 하나 | 유지보수성 향상 |
| **Cohesion** | 관련 기능들의 응집 | 코드 가독성 증대 |

---

## 🔍 문제 2: 데이터와 비즈니스 로직의 혼재

### 📋 에러 상황
Entity나 DTO에 비즈니스 로직이 섞여있어 책임이 모호한 경우입니다.

```java
// 😵 Entity에 비즈니스 로직이 혼재
@Entity
public class BadUser {
    private String email;
    private String password;
    
    // ❌ 데이터 저장 + 비즈니스 로직이 함께
    public boolean validatePassword(String inputPassword) {
        return BCrypt.checkpw(inputPassword, this.password);
    }
    
    public void sendWelcomeEmail() {
        // ❌ Entity가 이메일 발송까지 담당
        JavaMailSender mailSender = ApplicationContextProvider.getBean(JavaMailSender.class);
        // ... 이메일 발송 로직
    }
}
```

### 🎯 원인 분석
**데이터 표현과 비즈니스 로직이 한 곳에 섞여있어 책임 분리가 안되었습니다.**

1. **Entity의 역할 오버로드**: 데이터 + 검증 + 외부 서비스 호출
2. **테스트 어려움**: Entity 테스트에 외부 의존성 필요
3. **재사용성 저하**: 다른 컨텍스트에서 로직 재사용 불가

### 🔧 해결 방법

#### 1단계: Entity는 데이터만 담당

```java
// ✅ 순수한 데이터 표현에만 집중
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String email;
    private String password;
    
    // 🎯 단순한 데이터 접근자만 제공
    public String getEmail() { return email; }
    public String getPassword() { return password; }
    
    // 생성자, equals, hashCode 등...
}
```

#### 2단계: 비즈니스 로직은 전용 서비스로 분리

```java
// ✅ 사용자 인증 책임만 담당
@Service
public class AuthenticationService {
    
    public boolean validatePassword(User user, String inputPassword) {
        // 🔐 패스워드 검증 로직에만 집중
        return BCrypt.checkpw(inputPassword, user.getPassword());
    }
    
    public boolean isValidEmail(String email) {
        // ✉️ 이메일 형식 검증
        return email.matches("^[A-Za-z0-9+_.-]+@(.+)$");
    }
}
```

```java
// ✅ 사용자 관련 알림 책임만 담당
@Service
public class UserNotificationService {
    private final JavaMailSender mailSender;
    
    public void sendWelcomeEmail(User user) {
        // 🎉 환영 이메일 발송에만 집중
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(user.getEmail());
        message.setSubject("회원가입을 환영합니다!");
        message.setText("안녕하세요, " + user.getEmail() + "님!");
        mailSender.send(message);
    }
}
```

### 🎨 계층별 책임 분리 패턴

```java
// 🏗️ 사용자 생성 전체 과정을 조율하는 서비스
@Service
@RequiredArgsConstructor
public class UserRegistrationService {
    
    private final UserRepository userRepository;
    private final AuthenticationService authenticationService;
    private final UserNotificationService notificationService;
    
    @Transactional
    public User registerUser(UserRegistrationRequest request) {
        // 1️⃣ 입력 검증은 AuthenticationService에 위임
        if (!authenticationService.isValidEmail(request.getEmail())) {
            throw new InvalidEmailException("유효하지 않은 이메일입니다.");
        }
        
        // 2️⃣ 사용자 생성 및 저장
        User user = new User(request.getEmail(), encryptPassword(request.getPassword()));
        User savedUser = userRepository.save(user);
        
        // 3️⃣ 환영 이메일 발송은 NotificationService에 위임
        notificationService.sendWelcomeEmail(savedUser);
        
        return savedUser;
    }
    
    private String encryptPassword(String rawPassword) {
        return BCrypt.hashpw(rawPassword, BCrypt.gensalt());
    }
}
```

---

## 🔍 문제 3: 컨트롤러에 비즈니스 로직 포함

### 📋 에러 상황
Controller에서 HTTP 요청 처리 외에 비즈니스 로직까지 담당하는 경우입니다.

```java
// 😵 Controller가 너무 많은 책임을 가짐
@RestController
@RequestMapping("/api/orders")
public class BadOrderController {
    
    private final OrderRepository orderRepository;
    private final JavaMailSender mailSender;
    
    @PostMapping
    public ResponseEntity<Order> createOrder(@RequestBody OrderRequest request) {
        // ❌ HTTP 처리 + 비즈니스 로직 + 외부 서비스 호출
        
        // 1. 입력 검증
        if (request.getQuantity() <= 0) {
            return ResponseEntity.badRequest().build();
        }
        
        // 2. 비즈니스 로직
        Order order = new Order(request.getProductId(), request.getQuantity());
        Order savedOrder = orderRepository.save(order);
        
        // 3. 이메일 발송
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(request.getUserEmail());
        mailSender.send(message);
        
        return ResponseEntity.ok(savedOrder);
    }
}
```

### 🎯 원인 분석
**Controller가 웹 계층 책임을 넘어서 비즈니스 로직까지 처리하고 있습니다.**

1. **계층간 책임 혼재**: 프레젠테이션 + 비즈니스 + 데이터 액세스
2. **테스트 복잡성**: 웹 테스트에 비즈니스 로직 검증까지 포함
3. **재사용 불가**: 웹이 아닌 다른 방식으로 호출 불가

### 🔧 해결 방법

#### Controller는 HTTP 처리에만 집중

```java
// ✅ HTTP 요청/응답 처리에만 집중
@RestController
@RequestMapping("/api/orders")
@RequiredArgsConstructor
public class OrderController {
    
    private final OrderService orderService; // 🎯 비즈니스 로직은 서비스에 위임
    
    @PostMapping
    public ResponseEntity<OrderResponse> createOrder(@RequestBody @Valid OrderRequest request) {
        // 1️⃣ HTTP 요청을 받아서 서비스에 전달
        try {
            Order order = orderService.createOrder(request);
            
            // 2️⃣ 비즈니스 결과를 HTTP 응답으로 변환
            OrderResponse response = OrderResponse.from(order);
            return ResponseEntity.ok(response);
            
        } catch (InvalidOrderException e) {
            // 3️⃣ 예외를 적절한 HTTP 상태로 변환
            return ResponseEntity.badRequest().build();
        }
    }
}
```

### 📊 계층별 책임 분리 가이드라인

| 계층 | 주요 책임 | 포함하면 안되는 것 |
|------|-----------|-------------------|
| **Controller** | HTTP 요청/응답 처리 | 비즈니스 로직, DB 접근 |
| **Service** | 비즈니스 로직 수행 | HTTP 관련 코드, SQL |
| **Repository** | 데이터 액세스 | 비즈니스 검증, 외부 API |
| **Entity** | 데이터 표현 | 외부 서비스 호출 |

---

## 📊 SRP 체크리스트

### ✅ 클래스 단일 책임 확인
- [ ] 이 클래스를 수정해야 할 이유가 2개 이상인가?
- [ ] 클래스명에 'And', 'Or', 'Manager' 등이 들어가는가?
- [ ] 하나의 메서드가 50줄을 넘어가는가?

### ✅ 의존성 체크
- [ ] 한 클래스가 5개 이상의 의존성을 가지는가?
- [ ] 서로 다른 계층의 책임이 한 곳에 있는가?
- [ ] 테스트 시 많은 Mock 객체가 필요한가?

### ✅ 응집도 확인
- [ ] 클래스 내 메서드들이 같은 데이터를 사용하는가?
- [ ] 메서드들이 하나의 목적으로 그룹화되는가?
- [ ] 클래스명만 보고 역할을 예측할 수 있는가?

---

## 🎯 실전 SRP 적용 전략

### 1단계: 기존 코드 분석

```java
// 🔍 SRP 위반 징후 찾기
@Service
public class UserService {
    
    // ⚠️ 너무 많은 의존성 = 책임 과다 의심
    private final UserRepository userRepository;
    private final JavaMailSender mailSender;
    private final PasswordEncoder passwordEncoder;
    private final FileStorageService fileService;
    private final PaymentService paymentService; // 🚨 결제? 사용자 서비스에서?
    
    // 🔍 메서드명으로 책임 분석
    public void createUser() { }          // ✅ 사용자 관리
    public void sendEmail() { }           // ⚠️ 알림 책임?
    public void processPayment() { }      // ⚠️ 결제 책임?
    public void generateReport() { }      // ⚠️ 보고서 생성 책임?
}
```

### 2단계: 책임별 서비스 분리

```java
// ✅ 사용자 관리 책임만 담당
@Service
public class UserService {
    private final UserRepository userRepository;
    private final PasswordEncoder passwordEncoder;
    
    public User createUser(CreateUserRequest request) {
        // 사용자 생성 로직만 집중
    }
    
    public User updateUser(Long userId, UpdateUserRequest request) {
        // 사용자 수정 로직만 집중
    }
}

// ✅ 사용자 알림 책임만 담당
@Service 
public class UserNotificationService {
    private final JavaMailSender mailSender;
    
    public void sendWelcomeEmail(User user) { }
    public void sendPasswordResetEmail(User user, String token) { }
}

// ✅ 사용자 결제 책임만 담당
@Service
public class UserPaymentService {
    private final PaymentService paymentService;
    
    public void processUserPayment(User user, PaymentRequest request) { }
}
```

### 3단계: 조합 서비스로 전체 플로우 관리

```java
// 🎭 여러 서비스를 조합하여 복잡한 비즈니스 플로우 처리
@Service
@RequiredArgsConstructor
public class UserRegistrationFacade {
    
    private final UserService userService;
    private final UserNotificationService notificationService;
    private final UserPaymentService paymentService;
    
    @Transactional
    public User registerUser(UserRegistrationRequest request) {
        // 1️⃣ 사용자 생성
        User user = userService.createUser(request);
        
        // 2️⃣ 환영 이메일 발송
        notificationService.sendWelcomeEmail(user);
        
        // 3️⃣ 초기 결제 처리 (프리미엄 회원인 경우)
        if (request.isPremium()) {
            paymentService.processUserPayment(user, request.getPaymentInfo());
        }
        
        return user;
    }
}
```

---

## 🎉 마무리

이제 SRP를 적용하여 유지보수하기 쉽고 테스트하기 용이한 코드를 작성할 수 있습니다!

### 🚀 다음 단계 권장사항

1. **개방-폐쇄 원칙(OCP)**: 확장에는 열려있고 수정에는 닫힌 구조 설계
2. **의존성 역전 원칙(DIP)**: 인터페이스 기반의 느슨한 결합 구조
3. **리팩토링 도구 활용**: IntelliJ IDEA의 Extract Service 기능 활용

### 📞 추가 학습 리소스
- Clean Code by Robert Martin: 클린 코드의 바이블
- Effective Java 3판: 자바 개발자 필수 서적
- Spring Boot Reference Documentation: 스프링 부트 공식 가이드

### 💡 핵심 기억할 점
**"하나의 클래스는 하나의 변경 이유만 가져야 한다"** - 이 원칙만 지켜도 80%의 코드 품질 문제가 해결됩니다!
