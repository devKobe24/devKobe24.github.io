---
title: "📚[Backend Development] 🚀 @EntityListeners"
tags:
  - Backend Development
  - Server
  - Java
  - Lombok

date: "2026-01-26"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 🎧 JPA @EntityListeners 완벽 가이드

> Entity 생명주기를 감시하는 똑똑한 리스너! 데이터 저장 도구를 도메인 이벤트 시스템으로 업그레이드하세요.

---

## 📋 목차
1. [@EntityListeners가 뭔가요?](#1-entitylisteners가-뭔가요)
2. [감지 가능한 7가지 생명주기 이벤트](#2-감지-가능한-7가지-생명주기-이벤트)
3. [언제 사용해야 할까?](#3-언제-사용해야-할까)
4. [Entity 내부 vs 외부 Listener](#4-entity-내부-vs-외부-listener)
5. [실무 표준 패턴](#5-실무-표준-패턴)
6. [반드시 알아야 할 주의사항](#6-반드시-알아야-할-주의사항)
7. [@EntityListeners vs Spring Event](#7-entitylisteners-vs-spring-event)
8. [핵심 요약](#8-핵심-요약)

---

## 1️⃣ @EntityListeners가 뭔가요?

**@EntityListeners**는 JPA Entity의 생명주기 이벤트를 가로채서 특정 로직을 자동으로 실행하게 해주는 강력한 어노테이션입니다.

### 🎯 한 문장 정의

> "이 Entity가 저장·수정·삭제될 때, 이 클래스의 메서드를 자동으로 호출해줘!"

### 💡 기본 형태

```java
@Entity
@EntityListeners(AuditingEntityListener.class)
public class Member {
    // 이제 이 Entity는 감시 대상! 👀
}
```

### 🔄 동작 원리

```
Entity 변경 → JPA가 감지 → Listener 호출 → 자동 실행
```

**예시**:
1. `member.save()` 호출
2. JPA: "잠깐! Member가 저장되려고 해!"
3. Listener: "저장 전 작업 실행!"
4. 실제 INSERT 쿼리 실행

---

## 2️⃣ 감지 가능한 7가지 생명주기 이벤트

JPA는 Entity의 전체 생명주기를 7개 시점으로 나눠서 감시합니다.

| 이벤트 | 어노테이션 | 실행 시점 | 주요 용도 |
|:---:|:---:|:---:|---|
| 💾 **저장 전** | `@PrePersist` | INSERT 전 | 생성일 설정, 초기값 세팅 |
| ✅ **저장 후** | `@PostPersist` | INSERT 후 | 로그 기록, 이벤트 발행 |
| ✏️ **수정 전** | `@PreUpdate` | UPDATE 전 | 수정일 갱신, 검증 |
| ✅ **수정 후** | `@PostUpdate` | UPDATE 후 | 변경 이력 저장 |
| 🗑️ **삭제 전** | `@PreRemove` | DELETE 전 | 연관 데이터 정리 |
| ✅ **삭제 후** | `@PostRemove` | DELETE 후 | 캐시 삭제, 로그 |
| 📖 **로딩 후** | `@PostLoad` | SELECT 후 | 데이터 후처리 |

### 📊 생명주기 타임라인

```
CREATE: @PrePersist → [INSERT] → @PostPersist
UPDATE: @PreUpdate → [UPDATE] → @PostUpdate  
DELETE: @PreRemove → [DELETE] → @PostRemove
READ:   [SELECT] → @PostLoad
```

---

## 3️⃣ 언제 사용해야 할까?

### ✅ Case 1: 생성일/수정일 자동 관리 ⭐ (가장 대표적!)

```java
@Entity
@EntityListeners(AuditingEntityListener.class)
public class Post {

    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdAt;

    @LastModifiedDate
    private LocalDateTime updatedAt;
    
    @CreatedBy
    @Column(updatable = false)
    private String createdBy;
    
    @LastModifiedBy
    private String lastModifiedBy;
}
```

**장점**:
- ✅ 자동으로 시간 기록
- ✅ 개발자가 신경 쓸 필요 없음
- ✅ 휴먼 에러 방지

> 💡 **필수 설정**: `@EnableJpaAuditing`을 설정 클래스에 추가해야 작동!

---

### ✅ Case 2: 공통 로직 분리

```java
public class ValidationListener {
    
    @PrePersist
    @PreUpdate
    public void validate(Object entity) {
        if (entity instanceof Validatable) {
            ((Validatable) entity).validate();
        }
    }
}
```

**적용 대상**:
- 📝 로그 기록
- 🔍 감사(Audit) 추적
- ✅ 상태 검증
- 📊 통계 수집

---

### ✅ Case 3: 데이터 보정/정규화

```java
public class DataNormalizationListener {
    
    @PrePersist
    @PreUpdate
    public void normalize(Object entity) {
        if (entity instanceof Member) {
            Member member = (Member) entity;
            // 이메일 소문자 변환
            member.normalizeEmail();
            // 전화번호 형식 통일
            member.normalizePhoneNumber();
        }
    }
}
```

**활용 예시**:
- 📧 이메일 소문자 변환
- 📱 전화번호 포맷팅
- ✂️ 문자열 trim
- 🔤 대소문자 정규화

---

### ✅ Case 4: Entity 책임 분리

#### ❌ Before: Entity에 모든 로직

```java
@Entity
public class Member {
    private LocalDateTime updatedAt;
    
    public void update(String name) {
        this.name = name;
        this.updatedAt = LocalDateTime.now();  // 😰 매번 직접 설정
    }
}
```

#### ✅ After: Listener로 분리

```java
@Entity
@EntityListeners(AuditingEntityListener.class)
public class Member {
    @LastModifiedDate
    private LocalDateTime updatedAt;  // 🎉 자동으로 설정됨!
    
    public void update(String name) {
        this.name = name;  // 비즈니스 로직에만 집중
    }
}
```

**이점**:
- 🎯 관심사 분리 (Separation of Concerns)
- 🧪 테스트 용이성 향상
- 📝 코드 가독성 개선

---

## 4️⃣ Entity 내부 vs 외부 Listener

### 🔹 방식 1: Entity 내부에 직접 작성

```java
@Entity
public class Member {
    
    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;

    @PrePersist
    public void onCreate() {
        this.createdAt = LocalDateTime.now();
        this.updatedAt = LocalDateTime.now();
    }
    
    @PreUpdate
    public void onUpdate() {
        this.updatedAt = LocalDateTime.now();
    }
}
```

| 장점 ✅ | 단점 ❌ |
|---|---|
| 간단하고 직관적 | Entity 책임 증가 |
| 별도 클래스 불필요 | 재사용 불가능 |
| 빠른 구현 | 테스트 복잡 |

---

### 🔹 방식 2: 외부 Listener 클래스 (✨ 권장)

```java
// Listener 클래스
public class AuditListener {
    
    @PrePersist
    public void prePersist(Object entity) {
        System.out.println("저장 직전: " + entity);
    }

    @PreUpdate
    public void preUpdate(Object entity) {
        System.out.println("수정 직전: " + entity);
    }
}

// Entity
@Entity
@EntityListeners(AuditListener.class)
public class Member {
    // 깔끔! 🎨
}
```

| 장점 ✅ | 단점 ❌ |
|---|---|
| 관심사 완벽 분리 | 클래스 추가 필요 |
| 여러 Entity에서 재사용 | 초기 설정 필요 |
| 테스트 독립적 | - |
| 유지보수 쉬움 | - |

### 🏆 선택 가이드

```
간단한 로직 (1-2줄) → Entity 내부 메서드
복잡한 로직 / 재사용 → 외부 Listener 클래스 ⭐
```

---

## 5️⃣ 실무 표준 패턴

### 🌟 BaseTimeEntity 패턴 (추상 클래스 활용)

```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseTimeEntity {

    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdAt;

    @LastModifiedDate
    private LocalDateTime updatedAt;
}
```

### 🎯 실제 Entity에서 상속

```java
@Entity
public class Post extends BaseTimeEntity {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String title;
    private String content;
    
    // createdAt, updatedAt은 자동 관리됨! 🎉
}

@Entity
public class Comment extends BaseTimeEntity {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String content;
    
    // createdAt, updatedAt은 자동 관리됨! 🎉
}
```

### ✨ 고급 패턴: 생성자/수정자 추적

```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseEntity {

    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdAt;

    @LastModifiedDate
    private LocalDateTime updatedAt;

    @CreatedBy
    @Column(updatable = false)
    private String createdBy;

    @LastModifiedBy
    private String lastModifiedBy;
}
```

### 🔧 AuditorAware 설정 (생성자/수정자 자동 주입)

```java
@Configuration
@EnableJpaAuditing
public class JpaAuditingConfig {

    @Bean
    public AuditorAware<String> auditorProvider() {
        return () -> {
            // SecurityContext에서 현재 사용자 가져오기
            Authentication authentication = 
                SecurityContextHolder.getContext().getAuthentication();
            
            if (authentication == null || !authentication.isAuthenticated()) {
                return Optional.of("system");
            }
            
            return Optional.of(authentication.getName());
        };
    }
}
```

---

## 6️⃣ 반드시 알아야 할 주의사항

### ⚠️ 주의 1: 비즈니스 로직 넣지 말 것! (매우 중요)

#### ❌ 잘못된 사용

```java
public class PaymentListener {
    
    @PostPersist
    public void processPayment(Order order) {
        // 결제 처리 ❌
        paymentService.charge(order);
        
        // 알림 발송 ❌
        notificationService.sendEmail(order);
        
        // 외부 API 호출 ❌
        externalApi.syncOrder(order);
    }
}
```

**문제점**:
- 🔥 트랜잭션 경계가 불명확
- 🔥 실패 시 롤백 처리 복잡
- 🔥 순서 예측 불가능

#### ✅ 올바른 사용

```java
public class AuditListener {
    
    @PrePersist
    public void setCreatedDate(Object entity) {
        // 단순한 값 설정만 ✅
        if (entity instanceof BaseTimeEntity) {
            ((BaseTimeEntity) entity).setCreatedAt(LocalDateTime.now());
        }
    }
}
```

---

### ⚠️ 주의 2: Repository/Service 주입 불가 (기본)

#### ❌ 작동하지 않음

```java
public class BadListener {
    
    @Autowired  // ❌ 주입 안 됨!
    private MemberRepository memberRepository;
    
    @PrePersist
    public void doSomething(Object entity) {
        memberRepository.save(...);  // NullPointerException!
    }
}
```

**이유**: Listener는 기본적으로 Spring Bean이 아님!

#### ✅ 해결 방법 1: Spring Bean으로 등록

```java
@Component
public class SpringBeanListener {
    
    @Autowired
    private MemberRepository memberRepository;
    
    @PrePersist
    public void doSomething(Object entity) {
        // 이제 작동함!
    }
}
```

#### ✅ 해결 방법 2: ApplicationContext 활용

```java
public class SmartListener {
    
    @PrePersist
    public void doSomething(Object entity) {
        ApplicationContext context = 
            BeanUtil.getApplicationContext();
        MemberRepository repo = 
            context.getBean(MemberRepository.class);
        // 사용
    }
}
```

#### 🎯 더 좋은 방법: Spring Event 사용

```java
// Listener는 단순하게
@PostPersist
public void onCreated(Member member) {
    ApplicationEventPublisher publisher = 
        BeanUtil.getBean(ApplicationEventPublisher.class);
    publisher.publishEvent(new MemberCreatedEvent(member));
}

// 별도 EventHandler에서 처리
@EventListener
public void handleMemberCreated(MemberCreatedEvent event) {
    // 여기서 Repository, Service 자유롭게 사용
    notificationService.sendWelcomeEmail(event.getMember());
}
```

---

### ⚠️ 주의 3: 예외 발생 시 전체 트랜잭션 실패

```java
@PrePersist
public void validate(Member member) {
    if (member.getAge() < 0) {
        throw new IllegalArgumentException("나이는 음수일 수 없습니다");
        // → 전체 INSERT 취소! 🚨
    }
}
```

**결과**:
- 예외 발생 → DB 저장 실패
- 트랜잭션 롤백
- 데이터 일관성 유지

> 💡 **Tip**: 검증은 가급적 Service 레이어에서!

---

### ⚠️ 주의 4: Cascade 연산과의 상호작용

```java
@Entity
@EntityListeners(LoggingListener.class)
public class Order {
    
    @OneToMany(cascade = CascadeType.ALL)
    private List<OrderItem> items;
}

@Entity
@EntityListeners(LoggingListener.class)
public class OrderItem {
}
```

**주의**: Order 저장 시 OrderItem의 Listener도 모두 실행됨!

---

## 7️⃣ @EntityListeners vs Spring Event

언제 무엇을 사용해야 할까요?

| 비교 항목 | @EntityListeners 🎧 | Spring Event 📢 |
|:---:|:---:|:---:|
| **호출 시점** | DB 트랜잭션 전/후 | 비즈니스 로직 흐름 |
| **주요 용도** | 감사, 데이터 보정 | 알림, 외부 연동 |
| **의존성 주입** | 복잡함 (기본 불가) | 자유로움 |
| **외부 시스템** | ❌ 비추천 | ✅ 추천 |
| **트랜잭션** | DB 트랜잭션 내부 | 별도 제어 가능 |
| **실행 순서** | JPA가 결정 | 명시적 제어 |
| **성능** | 빠름 | 상대적으로 느림 |

### 🎯 선택 가이드

```java
// EntityListeners 사용 ✅
- 생성일/수정일 자동 기록
- 데이터 정규화 (trim, 소문자 등)
- 간단한 감사 로그

// Spring Event 사용 ✅
- 이메일/푸시 알림 발송
- 외부 API 연동
- 복잡한 비즈니스 로직
- 여러 서비스 호출
```

### 💡 함께 사용하기

```java
// 1단계: EntityListener에서 이벤트 발행
@Component
public class MemberEventListener {
    
    @Autowired
    private ApplicationEventPublisher eventPublisher;
    
    @PostPersist
    public void onMemberCreated(Member member) {
        eventPublisher.publishEvent(new MemberCreatedEvent(member));
    }
}

// 2단계: EventHandler에서 비즈니스 로직 처리
@Component
public class MemberEventHandler {
    
    @Autowired
    private EmailService emailService;
    
    @EventListener
    @Async
    public void handleMemberCreated(MemberCreatedEvent event) {
        emailService.sendWelcomeEmail(event.getMember());
    }
}
```

---

## 8️⃣ 핵심 요약

### 💡 한 문장 정리

> **@EntityListeners**는 Entity 생명주기에 자동으로 반응하는 후크(Hook)다.

### 📌 기억 공식

```
✅ 생성일/수정일 자동 기록 = @EntityListeners + Auditing
✅ 데이터 보정/검증 = @EntityListeners
❌ 비즈니스 로직 = Spring Event 사용
❌ 외부 API 호출 = Spring Event 사용
```

### 🎓 실무 체크리스트

#### 기본 설정
- [ ] `@EnableJpaAuditing` 설정 완료
- [ ] `BaseTimeEntity` 추상 클래스 생성
- [ ] 모든 Entity가 `BaseTimeEntity` 상속

#### 고급 설정
- [ ] `AuditorAware` 구현 (생성자/수정자 추적)
- [ ] 커스텀 Listener 분리
- [ ] Spring Event와 역할 분담

#### 주의사항
- [ ] Listener에 비즈니스 로직 없음
- [ ] 외부 API 호출 없음
- [ ] DI가 필요하면 Spring Bean으로 등록
- [ ] 예외 처리 전략 수립

---

## 🚀 실전 예제: 완벽한 구성

### 1️⃣ 기본 Entity

```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseEntity {

    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdAt;

    @LastModifiedDate
    private LocalDateTime updatedAt;

    @CreatedBy
    @Column(updatable = false)
    private String createdBy;

    @LastModifiedBy
    private String lastModifiedBy;
}
```

### 2️⃣ 설정 클래스

```java
@Configuration
@EnableJpaAuditing
public class JpaConfig {

    @Bean
    public AuditorAware<String> auditorProvider() {
        return () -> Optional.ofNullable(
            SecurityContextHolder.getContext()
                .getAuthentication()
                .getName()
        );
    }
}
```

### 3️⃣ 실제 Entity

```java
@Entity
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Post extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String title;

    @Column(nullable = false, columnDefinition = "TEXT")
    private String content;

    @Enumerated(EnumType.STRING)
    private PostStatus status;

    public static Post create(String title, String content) {
        Post post = new Post();
        post.title = title;
        post.content = content;
        post.status = PostStatus.DRAFT;
        return post;
        // createdAt, updatedAt, createdBy, lastModifiedBy는 자동 설정! 🎉
    }

    public void publish() {
        this.status = PostStatus.PUBLISHED;
        // updatedAt, lastModifiedBy는 자동 갱신! 🎉
    }
}
```

---

## 🎯 다음 학습 주제

- Spring Event 심화 (`@EventListener`, `@Async`)
- JPA Auditing 고급 설정
- `@Embedded`와 `@Embeddable`
- Soft Delete 패턴 구현
- 변경 이력 추적 시스템 구축

---

## 📚 참고 자료

### 공식 문서
- [JPA Specification - Entity Listeners](https://jakarta.ee/specifications/persistence/)
- [Spring Data JPA - Auditing](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#auditing)

### 관련 어노테이션
- `@MappedSuperclass` - 공통 매핑 정보 상속
- `@EnableJpaAuditing` - Auditing 기능 활성화
- `@CreatedDate`, `@LastModifiedDate` - 시간 자동 관리
- `@CreatedBy`, `@LastModifiedBy` - 사용자 자동 관리

---

## 💬 FAQ

<details>
<summary><strong>Q1. @EntityListeners를 여러 개 등록할 수 있나요?</strong></summary>

네! 배열로 여러 Listener를 등록할 수 있습니다.

```java
@Entity
@EntityListeners({
    AuditingEntityListener.class,
    ValidationListener.class,
    LoggingListener.class
})
public class Member {
}
```

실행 순서는 배열 순서대로입니다.
</details>

<details>
<summary><strong>Q2. Listener에서 다른 Entity를 조회할 수 있나요?</strong></summary>

가능하지만 신중해야 합니다.

```java
@Component
public class RelationListener {
    
    @Autowired
    private EntityManager em;
    
    @PrePersist
    public void loadRelation(Order order) {
        // 가능하지만 N+1 문제 주의!
        Member member = em.find(Member.class, order.getMemberId());
    }
}
```

성능 이슈가 발생할 수 있으므로 꼭 필요한 경우만 사용하세요.
</details>

<details>
<summary><strong>Q3. @PostLoad는 언제 사용하나요?</strong></summary>

주로 데이터 후처리나 캐싱에 사용합니다.

```java
@PostLoad
public void decrypt(Member member) {
    // 암호화된 데이터 복호화
    member.decryptSensitiveData();
}
```

하지만 성능에 영향을 줄 수 있으므로 신중하게 사용하세요.
</details>

---
