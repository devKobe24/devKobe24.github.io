---
title: 🏗️[Architecture] 멀티테넌트 아키텍처(Multi-Tenant Architecture)란 무엇일까요?
tags:
    - Architecture
date: "2024-12-11"
thumbnail: "/assets/img/thumbnail/architecture.jpeg"
---

# 🏗️[Architecture] 멀티테넌트 아키텍처(Multi-Tenant Architecture)란 무엇일까요?
- 멀티테넌트 아키텍처(Multi-Tenant Architecture)는 하나의 애플리케이션 인스턴스와 데이터베이스가 여러 사용자 그룹(테넌트,Tenant) 간에 공유되면서, 각 테넌트(Tenant)가 독립적으로 동작하도록 설계된 소프트웨어 아키텍처입니다.
    - 이는 클라우트 서비스, SaaS(Software as a Service) 애플리케이션에서 자주 사용됩니다.

## 1️⃣ 멀티테넌트 아키텍처(Muti-Tenant Architecture)의 핵심 개념.
### 1️⃣ 테넌트(Tenant)란?
- 테넌트(Tenant)는 하나의 독립된 사용자 그룹이나 고객을 의미합니다.
    - 예: 특정 회사, 조직 또는 사용자 그룹.
### 2️⃣ 멀티테넌트의 목적.
- 여러 테넌트가 동일한 소프트웨어 애플리케이션을 사용하면서도 **데이터와 환경이 서로 독립적으로 관리되도록 하는 것 입니다.**

## 2️⃣ 멀티테넌트 아키텍처 종류
### 1️⃣ 공유형 데이터베이스 및 스키마.
- **특징** 
    - 모든 테넌트가 같은 데이터베이스와 테이블을 공유합니다.
- **장점**
    - 리소스 사용이 가장 효율적입니다.
    - 확장이 쉽습니다.
    - 관리가 간단합니다.
- **단점**
    - 데이터 격리와 보안 구현이 복잡합니다.
    - 특정 테넌트의 트래픽 증가가 다른 테넌트의 영향을 줄 수 있습니다.

### 2️⃣ 예시
```sql
CREATE TABLE message (
    id INT PRIMARY KEY AUTO_INCREMENT,
    tenant_id INT NOT NULL,
    sender_id INT NOT NULL,
    message TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
- 테이블에 tenant_id를 추가하여 데이터를 구분합니다.

### 2️⃣ 분리형 데이터베이스, 공유 스키마.
- **특징**
    - 모든 테넌트가 별도의 데이터베이스를 가지지만, 동일한 테이블 구조(스키마)를 공유합니다.
- **장점**
    - 데이터가 물리적으로 분리되어 보안과 데이터 격리가 용이.
    - 특정 테넌트의 성능 요구에 따라 개별적으로 스케일링 가능.
- **단점**
    - 데이터베이스가 늘어날수록 관리 복잡성이 증가
    - 데이터베이스 연결 비용 증가.

### 3️⃣ 완전 분리형 데이터베이스 및 스키마.
- **특징**
    - 각 테넌트마다 고유한 데이터베이스와 테이블 구조를 사용합니다.
- **장점**
    - 완벽한 데이터 격리와 보안 제공.
    - 테넌트별로 맞춤형 스키마와 기능 제공 가능.
- **단점**
    - 테넌트 수가 많아지면 확장성이 떨어짐.
    - 관리와 배포가 매우 복잡.

## 3️⃣ 멀티테넌트 아키텍처의 장단점.
### 1️⃣ 장점.
- **1. 비용 효율성**
    - 여러 테넌트가 동일한 인프라를 공유하므로 리소스 활용도가 높음.
- **2. 운영 효율성**
    - 한 번의 배포로 모든 테넌트에 업데이트 가능.
- **3. 확장성**
    - 테넌트 수가 증가해도 서버를 추가하여 확장 가능(수평 확장).

### 2️⃣ 단점.
- **1. 복잡한 데이터 격리**
    - 테넌트별 데이터를 분리하기 위한 추가적인 보안 및 로직 필요.
- **2. 리소스 공유 문제**
    - 특정 테넌트의 리소스 사용량이 많을 경우 다른 테넌트의 영향을 줄 수 있음.
- **3. 맞춤화 제한**
    - 개별 테넌트별로 맞춤 기능을 제공하기 어렵거나 구현이 복잡.

## 4️⃣ 멀티테넌트 구현 방법.
### 1️⃣ 데이터베이스 설계.
#### 공유형 데이터베이스 예시
```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    tenant_id INT NOT NULL,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL
)
```
- tenant_id를 기반으로 데이터 필터링.

#### 분리형 데이터베이스 예시
- **1. 테넌트 식별을 위한 Interceptor**
    - 요청마타 테넌트를 식별하여 적절한 데이터베이스로 연결합니다.
```java
@Component
public class TenantInterceptor implements HandlerInterceptor {
    
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String tenantId = request.getHeader("X-Tenant-ID");
        if (tenantId == null) {
            throw new RuntimeException("Tenant ID is not provided");
        }
        TenantContext.setCurrentTenant(tenantId);
        return true
    }
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handeler, Exception ex) throws Exception {
        TenantContext.clear();
    }
}
```

- **2. 테넌트 컨텍스트 저장소**
    - 현재 요청의 테넌트를 저장하고 관리합니다.
```java
public class TenantContext {
    private static final ThreadLocal<String> CURRENT_TENANT = new ThreadLocal<>();
    
    public static void setCurrentTenant(String tenant) {
        CURRENT_TENANT.set(tenant);
    }
    
    public static String getCurrentTenant() {
        return CURRENT_TENANT.get();
    }
    
    public static void clear() {
        CURRENT_TENANT.remove();
    }
}
```

- **3. 동적 데이터 소스 구성**
    - 테넌트별 데이터베이스를 동적으로 선택합니다.
```java
@Configuration
public class DataSourceConfig {
    
    @Bean
    public DataSource dataSource() {
        return new AbstractRoutingDataSource() {
            @Override
            protected Object determineCurrentLookupKey() {
                return TenantContext.getCurrentTenant();
            }
        };
    }
    
    @Bean
    public DataSourceInitializer dataSourceInitializer(DataSource dataSource) {
        DataSourceInitializer initializer = new DataSourceInitializer();
        initializer.setDataSource(dataSource);
        return initializer;
    }
    
    @Bean
    public Map<Object, Object> tenantDataSources() {
        Map<Object, Object> dataSources = new HashMap<>();
        
        // 데이터베이스 A
        DataSource tenantADataSource = DataSourceBuilder.create()
            .url("jdbc:mysql://localhost:3306/tenant_a_db")
            .username("root")
            .password("password")
            .driverClassName("com.mysql.cj.jdbc.Driver")
            .build();
        dataSources.put("tenant_a", tenantADataSource);

        // 데이터베이스 B
        DataSource tenantBDataSource = DataSourceBuilder.create()
            .url("jdbc:mysql://localhost:3306/tenant_b_db")
            .username("root")
            .password("password")
            .driverClassName("com.mysql.cj.jdbc.Driver")
            .build();
        dataSources.put("tenant_b", tenantBDataSource);

        return dataSources;
    }
}
```
- **4. 요청 처리 흐름**
    - 클라이언트가 요청을 보낼 때 X-Tenant-ID 헤더를 추가합니다.
```http
GET /api/resource HTTP/1.1
Host: example.com
X-Tenant-ID: tenant_a
```
- TenantInterceptor가 X-Tenant-ID를 읽어 TenantContext에 저장합니다.
- AbstractRoutingDataSource가 TenantContext에서 테넌트 정보를 읽어 해당 테넌트의 데이터베이스를 사용합니다.

#### 데이터베이스 구조
- Tenant A: tenant_a_db

|Table| Columns |
| -------- | -------- |
|users|id, name, email|
|orders|id, user_id, order_data, total|
|products|id, name, price, stock_quantity|

- Tenant B: tenant_b_db

|Table| Columns |
| -------- | -------- |
|users|id, name, email|
|orders|id, user_id, order_data, total|
|products|id, name, price, stock_quantity|

#### 장점.
- 1. 데이터가 물리적으로 분리되어 보안이 강력
- 2. 테넌트 간 데이터 간섭 가능성이 없음
- 3. 개별 테넌트별로 데이터베이스 성능을 독립적으로 관리 가능

#### 단점.
- 1. 데이터베이스가 많아질수록 관리 복잡성이 증가.
- 2. 스키마 변경시 모든 데이터베이스에 동일한 작업을 반복해야 함.

### 2️⃣ Spring Boot 구현
- 엔티티
```java
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private Long tenantId;

    private String name;

    private String email;

    private String password;

    // Getters and setters
}
```

- Repository
```java
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByTenantId(Long tenantId);
}
```

- Sevice
```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public List<User> getUsersByTenantId(Long tenantId) {
        return userRepository.findByTenantId(tenantId);
    }
}
```

- 컨트롤러
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping
    public List<User> getUsers(@RequestParam Long tenantId) {
        return userService.getUsersByTenantId(tenantId);
    }
}
```

## 5️⃣ 멀티테넌트와 SaaS
- SaaS(Software as a Service) 애플리케이션은 멀티테넌트를 사용하는 대표적인 사례입니다.
    - 예: Slack, Salesforce, Google Workspace 등
        - 각 조직이 고유한 데이터를 가지지만, 동일한 애플리케이션 인스턴스를 사용.

## 6️⃣ 멀티테넌트 설계 시 고려 사항.
- **1. 보안**
    - 테넌트 간 데이터 접근 방지.
    - 인증 토근에 tenant_id 포함.
- **2. 성능**
    - 특정 테넌트의 과도한 리소스 사용이 다른 테넌트에 영향을 주지 않도록 제한 설정.
- **3. 스케일링**
    - 테넌트 수가 증가할 때 서버, 데이터베이스의 수평적 확장이 가능해야 함.

## 7️⃣ 결론.
- 멀티테넌트 아키텍처는 하나의 시스템으로 여러 사용자 그룹을 지원하면서, 데이터와 환경을 독립적으로 관리할 수 있는 효율적인 방식입니다.
    - 구현 방식은 서비스의 요구사항과 테넌트 수에 따라 결정되며, 설계 단계에서 데이터 격리와 확장성을 철저히 고려해야 합니다.
