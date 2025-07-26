---
title: "📚[Backend Development] 기본 단위 테스트는 어떤 단위로 해야할까?"
tags:
    - Backend Ddevelopment
    - Test
    - Server
    - Build System
date: "2025-07-26"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 📚[Backend Development] 기본 단위 테스트는 어떤 단위로 해야할까?

**실무에서는 기본 단위 테스트를 보통 각 계층(레이어) 별로** 나누어 작성하는 것이 일반적이며 권장되는 방식입니다.
아래와 같이 계층별로 나누어 관리함으로써 테스트의 목적과 범위를 명확히 구분할 수 있고, 유지보수성과 가독성이 좋아집니다.

---

## ✅ 실무에서 계층별로 나누는 단위 테스트 구조

| 계층 | 테스트 명칭 | 예시 파일명 | 주요 테스트 대상 |
| -------- | -------- | --- | -------- |
| Entity | Entity 단위 테스트 | UserTest.java | 비즈니스 메서드, equals/hashCode, 유효성 검사 등|
| Repository | Repository 단위 테스트 | UserRepositoryTest.java | 쿼리 메서드, JPA 동작 검증, 쿼리 결과 확인 |
| Service | Service 단위 테스트 | UserServiceTest.java | 서비스 로직, 트랜잭션 처리, 예외 처리 |
| Controller | Controller 단위 테스트 | UserControllerTest.java | API 요청/응답, 상태 코드, DTO 매핑 |
| Integration | 통합 테스트 | UserIntegrationTest.java | 서비스 + DB + 인증 등 복합 시나리오 |

---

## ✅ 예시 구조 (Spring Boot 기준)
```plaintext
src/test/java/com/example/project/
├── entity/
│   └── UserTest.java
├── repository/
│   └── UserRepositoryTest.java
├── service/
│   └── UserServiceTest.java
├── controller/
│   └── UserControllerTest.java
└── integration/
    └── UserIntegrationTest.java
```

---

## ✅ 이유: 왜 계층별로 나누는가?

| 이유 | 설명 |
| -------- | -------- |
| 책임 명확화 | 어떤 문제가 발생했는지 빠르게 파악 가능 |
| 테스트 유지보수 용이 | 테스트 범위가 좁아져 디버깅 및 수정이 쉬움 |
| 계층 분리 설계와 맞물림 | 도메인/서비스/컨트롤러 레이어 설계 철학 반영 |
| 테스트 커버리지 향상 | 각 레이어를 독립적으로 검증하므로 빠짐없이 테스트 가능 |

---

## ✅ 실무 팁
- **@SpringBootTest** 는 통합 테스트에서만 사용하고, 단위 테스트에서는 **Mockito/MockMvc** 조합을 주로 사용
- Repository 테스트는 **H2 + @DataJpaTest** 로, Service 테스트는 **@MockBean으로 Repository 주입**
- Entity 테스트는 **순수 Java 단위 테스트로 처리**

---

## 🙌 결론
> ✅ **실무에서는 Entity, Repository, Service, Controller 단위로 테스트 코드를 나누는 것이 일반적이고 바람직한 방식입니다.**
