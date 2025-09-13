---
title: 💻[Code Review] 로그인 기능 코드 리뷰.
tags:
    - Code review
date: "2025-09-12"
thumbnail: "/assets/img/thumbnail/codereview.jpg"
---

# 💻[Code Review] 로그인 기능 코드 리뷰.

## 🚀 로그인 기능

> Java Spring Boot 기반 로그인 시스템 코드 검토 및 개선 제안

---

## 📋 전체 평가

전반적으로 체계적이고 안정적인 구현입니다. 비즈니스 요구사항(BR-LOGIN-001, BR-LOGIN-002 등)을 코드 주석으로 명시하여 추적 가능성을 높인 점과 보안 관련 기능들이 적절히 적용된 점이 주목할 만합니다.

---

## 📁 파일별 상세 리뷰

### AuthController.java

#### 장점
- `@Valid` 어노테이션을 통한 요청 검증 적용
- 적절한 HTTP 상태 코드 사용 (200 OK, 201 Created)
- RESTful API 설계 원칙 준수

#### 개선 사항
**보안 - 로깅 정책**
```java
// 현재: 민감한 정보가 로그에 노출될 가능성
log.info("Login attempt for email: {}", request.getEmail());

// 개선: 마스킹 처리
log.info("Login attempt for email: {}", maskEmail(request.getEmail()));
```

---

### AuthServiceImpl.java

#### 장점
- **계정 잠금 처리**: 5회 실패 시 계정 잠금 로직 구현
- **로그인 이력 관리**: `@Transactional(propagation = REQUIRES_NEW)`로 트랜잭션 분리
- **RTR 패턴**: Refresh Token Rotation 적용으로 보안 강화

#### 개선 사항

**1. 타이밍 공격 방지**
```java
// 현재: 이메일 존재 여부에 따라 응답 시간 차이 발생
Member member = memberRepository.findByMemberEmail(request.getEmail())
    .orElseThrow(() -> new BadCredentialsException("인증 실패"));

// 개선: 일정한 응답 시간 유지
public TokenPair login(LoginRequest request) {
    String email = request.getEmail();
    String password = request.getPassword();
    
    // 항상 회원 조회 시도
    Optional<Member> memberOpt = memberRepository.findByMemberEmail(email);
    
    // 더미 해시와 비교하여 시간 일정하게 유지
    String targetHash = memberOpt.map(Member::getPassword)
        .orElse("$2a$12$dummy.hash.to.prevent.timing.attack");
    
    boolean passwordMatches = passwordEncoder.matches(password, targetHash);
    
    if (memberOpt.isEmpty() || !passwordMatches) {
        throw new BadCredentialsException("인증 실패");
    }
    
    // 로그인 성공 로직...
}
```

**2. IP 주소 처리 개선**
```java
// 현재: 하드코딩된 IP 주소
private void saveLoginHistory(String memberId, String email, boolean isSuccess) {
    // TODO: 실제 IP 주소 추출 로직 구현
    String ipAddress = "0.0.0.0";
}

// 개선: 컨트롤러에서 IP 주소 전달
@PostMapping("/login")
public ResponseEntity<LoginResponse> login(
    @Valid @RequestBody LoginRequest request,
    HttpServletRequest httpRequest) {
    
    String clientIp = getClientIpAddress(httpRequest);
    TokenPair tokenPair = authService.login(request, clientIp);
    // ...
}

private String getClientIpAddress(HttpServletRequest request) {
    String xForwardedFor = request.getHeader("X-Forwarded-For");
    if (xForwardedFor != null && !xForwardedFor.isEmpty()) {
        return xForwardedFor.split(",")[0].trim();
    }
    
    String xRealIp = request.getHeader("X-Real-IP");
    if (xRealIp != null && !xRealIp.isEmpty()) {
        return xRealIp;
    }
    
    return request.getRemoteAddr();
}
```

---

### SecurityConfig.java & JWT 관련 클래스

#### 장점
- 명확한 역할 분리 (설정, 토큰 관리, 필터 처리)
- Stateless 세션 정책 적용
- 외부 설정을 통한 토큰 만료 시간 관리

#### 개선 사항

**1. Secret Key 보안 강화**
```yaml
# 현재: application.yml에 직접 노출
jwt:
  secret-key: mySecretKey123

# 개선: 환경 변수 사용
jwt:
  secret-key: ${JWT_SECRET_KEY:defaultSecretForDev}
```

**2. JWT 필터 예외 처리**
```java
@Component
public class JwtAuthenticationEntryPoint implements AuthenticationEntryPoint {
    
    @Override
    public void commence(HttpServletRequest request, 
                        HttpServletResponse response,
                        AuthenticationException authException) throws IOException {
        
        response.setContentType("application/json;charset=UTF-8");
        response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
        
        ObjectMapper mapper = new ObjectMapper();
        String jsonResponse = mapper.writeValueAsString(
            ErrorResponse.builder()
                .errorCode("UNAUTHORIZED")
                .message("인증이 필요합니다")
                .build()
        );
        
        response.getWriter().write(jsonResponse);
    }
}
```

---

### 엔티티 클래스 (Member, LoginHistory, RefreshToken)

#### 장점
- **도메인 중심 설계**: 엔티티 내 비즈니스 로직 포함
- **적절한 연관 관계**: `@MapsId`를 통한 1:1 관계 구현
- **캡슐화**: 엔티티 상태 변경을 메서드로 제어

#### 개선 사항

**성능 최적화**
```java
// 현재: 매번 계산 수행
public boolean isAccountNonLocked() {
    if (accountLockedAt == null) return true;
    return LocalDateTime.now().isAfter(accountLockedAt.plusMinutes(10));
}

// 개선: 잠금 해제 시간 필드 추가
@Column(name = "account_unlock_at")
private LocalDateTime accountUnlockAt;

public void lockAccount() {
    this.accountLockedAt = LocalDateTime.now();
    this.accountUnlockAt = this.accountLockedAt.plusMinutes(10);
}

public boolean isAccountNonLocked() {
    if (accountUnlockAt == null) return true;
    return LocalDateTime.now().isAfter(accountUnlockAt);
}
```

---

## 🛠️ 우선순위별 개선 권장사항

### 높음 (보안 관련)
1. **타이밍 공격 방지**: 인증 실패 시 일정한 응답 시간 유지
2. **Secret Key 보안**: 환경 변수 또는 외부 보안 저장소 사용
3. **로그 마스킹**: 민감한 정보 로그 노출 방지

### 중간 (기능 개선)
4. **IP 주소 처리**: 실제 클라이언트 IP 주소 추출 로직 구현
5. **JWT 예외 처리**: 커스텀 AuthenticationEntryPoint 구현
6. **성능 최적화**: 계정 잠금 확인 로직 개선

### 낮음 (코드 품질)
7. **테스트 코드**: 단위 테스트 및 통합 테스트 추가
8. **메트릭 수집**: 로그인 성공/실패율 모니터링
9. **문서화**: API 명세서 및 보안 정책 문서 보완

---

## 결론

현재 구현은 보안과 안정성 측면에서 높은 수준의 코드입니다. 제안된 개선사항들을 단계적으로 적용하면 더욱 견고한 로그인 시스템을 구축할 수 있을 것입니다. 특히 보안 관련 개선사항들은 우선적으로 고려해볼 만합니다.
