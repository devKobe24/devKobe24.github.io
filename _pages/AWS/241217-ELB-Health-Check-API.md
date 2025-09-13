---
title: "☁️[AWS] ELB(Elastic Load Balancer) Health Check API 구현하기!"
tags:
    - AWS
    - Network
date: "2024-12-17"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] ELB(Elastic Load Balancer) Health Check API 구현하기!
- Spring Boot 백엔드 애플리케이션에서 ELB(Elastic Load Balancer) Health Check API를 구현하는 방법은 간단합니다.
    - 일반적으로 Health Check API는 ELB가 애플리케이션 인스턴스가 정상인지 확인하는 데 사용되며, HTTP 상태 코드로 200(정상) 또는 500(비정상)을 반환합니다.

## 1️⃣ 구현 방법.
### 1️⃣ Spring Boot 컨트롤러 생성.
- Health Check API는 간단한 `@RestController`로 구현할 수 있습니다.
    - 기본적으로 `/health` 또는 `/ping` 경로로 설정하는 것이 일반적입니다.

### 2️⃣ 정상 상태 확인
- Health Check API는 기본적으로 항상 200 OK를 반환하거나, 애플리케이션의 특정 상태를 점검하여 상태를 반영할 수 있습니다.

## 2️⃣ 예제 코드.
```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestContoller;

@RestController
public class HealthCheckController {
    
    @GetMapping("/health")
    public ResponseEntity<String> healthCheck() {
        // 애플리케이션 상태 확인 로직 (예: DB 연결 상태, 기타 서비스 상태 등)
        boolean isHealthy = checkApplicationHealth();
        
        if (isHealthy) {
            return ResponseEntity.ok("Application is healthy");
        } else {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                                 .body("Application is not healthy");
        }
    }
    
    private boolean checkApplicationHealth() {
        // 간단한 헬스 체크 로직(예: 항상 true 반환)
        // 실제로는 DB 연결, 캐시 상태, 외부 서비스 상태 등을 점검
        return true;
    }
}
```

## 3️⃣ ELB에 연동
- ELB에서 Health Check를 설정할 때, Spring Boot에서 만든 `/health` 엔드포인트를 사용합니다.

### 1️⃣ ELB Health Check 설정.
- 1. Target Group 설정.
    - **Protocol :** HTTP
    - **Port :** 애플리케이션 포트(예: 8080)
    - **Path :** `/health`
- 2. Health Check 설정.
    - **Success codes :** 200
    - **Interval :** Health Check를 수행하는 간격(기본값: 30초)
    - **Timeout :** Health Check 요청에 대한 응답 시간(기본값: 5초)
    - **Unhealthy threshold :** 비정상으로 간주하기 전 실패 횟수(기본값: 2)
    - **Health threshold :** 정상으로 간주하기 위한 성공 횟수(기본값: 3)

## 4️⃣ 고급 사용 예
- Health Check API에서 더 복잡한 상태를 점검할 수도 있습니다.
    - 예를 들어, 데이터베이스 연결 상태나 외부 API 응답 시간을 확인하려면 아래와 같이 구현합니다:
```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import javax.sql.DataSource;

@RestController
public class HealthCheckController {
    
    private final DataSource dataSource;
    
    public HealthCheckController(DataSource dataSource) {
        this.dataSource = dataSource;
    }
    
    @GetMapping("/health")
    public ResponseEntity<String> healthCheck() {
        try {
            // 데이터베이스 연결 확인
            dataSource.getConnection().isValid(1);
            return ResponseEntity.ok("Application and DB are healthy");
        } catch (Exception e) {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                                 .body("Application or DB is not healthy");
        }
    }
}
```

## 5️⃣ Spring Actuator 사용(옵션)
- Spring Boot Actuator를 사용하면 헬스 체크를 자동으로 처리할 수 있습니다.
    - 1. 의존성 추가:
    ```gradle
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
    ````
    
    - 2. Actuator 헬스 엔드포인트 활성화:
        - `application.properties` 또는 `application.yml`에 다음을 추가:
        ```properties
        management.endpoints.web.exposure.include=health
        management.endpoint.health.show-details=always
        ```
    
    - 3. 헬스 체크 경로:
        - 기본적으로 `/actuator/health` 경로에서 헬스 체크 정보를 제공합니다.

## 6️⃣ 갈무리.
- 이제 ELB가 애플리케이션의 `/health` API를 사용하여 정상 상태를 확인할 수 있습니다.
    - 필요에 따라 추가적인 상태 점검 로직을 포함할 수 있습니다.
