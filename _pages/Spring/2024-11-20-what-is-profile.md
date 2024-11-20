---
title: 🍃[Spring] Profile이란 무엇일까요?
tags:
    - Spring
    - Framework
date: "2024-11-20"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Profile이란 무엇일까요?
- **Profile**은 애플리케이션의 실행 환경(예: 개발, 테스트, 운영)에 따라 다른 설정을 적용할 수 있도록 하는 기능입니다.
    - 이를 통해 환경에 따라 적합한 설정값(예: 데이터베이스 URL, 로깅 수준 등)을 간단하게 관리할 수 있습니다.

## 1️⃣ Profile의 주요 개념.
### 1️⃣ 환경별 설정 분리.
- 개발(Development), 테스트(Test), 운영(Production) 등 서로 다른 실행 환경에 따라 설정값을 분리하고, 특정 환경에 맞는 설정을 쉽게 적용할 수 있습니다.

### 2️⃣ 설정 파일 관리.
- application.properties 또는 application.yml 파일에서 환경별 설정을 작성할 수 있으며, 프로파일별로 파일을 분리해 관리할 수 있습니다.

### 3️⃣ 유연한 실행.
- `spring.profiles.active` 속성을 통해 실행 시 활성화할 프로파일을 지정할 수 있습니다.

## 2️⃣ Profile 설정 방법.
### 1️⃣ 기본 설정 파일 사용.
- 모든 환경에서 공통으로 사용하는 설정은 `application.properties` 또는 `application.yml` 파일에 작성합니다.
- 프로파일별 설정은 `application-{profile}.properties` 또는 `application-{profile}.yml` 파일로 작성합니다.

### 2️⃣ 예시.
- `application.properties`(공통 설정)
```properties
server.port=8080
spring.datasource.username=root
spring.datasource.password=common-password
```

- `application-dev.properties`(개발 환경)
```properties
server.port=8081
spring.datasource.url=jdbc:mysql://localhost:3306/dev_db
```

- `application-prod.properties`(운영 환경)
```properties
server.port=8082
spring.datasource.url=jdbc:mysql://prod-db-example.com:3306/prod_db
spring.datasource.password=prod-password
```

## 3️⃣ 활성화할 Profile 지정.
- `spring.profiles.active`를 사용하여 실행 시 활성화할 프로파일을 지정합니다.

### 1️⃣ application.properties에서 지정.
```properties
spring.profiles.active=dev
```

### 2️⃣ JVM 옵션으로 지정.
```bash
java -jar -Dspring.profiles.active=prod myapp.jar
```

### 3️⃣ 환경 변수로 지정.
```bash
export SPRING_PROFILES_ACTIVE=dev
```

### 4️⃣ IDE에서 지정.
실행 설정에서 `-Dspring.profile.active=dev`를 추가합니다.

## 4️⃣ 프로파일별 Bean 정의
- Spring에서는 프로파일별로 Bean을 다르게 설정할 수 있습니다.

### 1️⃣ 예시.
```java
@Configuration
public class DataSourceConfig {
    
    @Bean
    @Profile("dev")
    public DataSource devDataSource() {
        return DataSourceBuilder.create()
                .url("jdbc:mysql://localhost:3306/dev_db")
                .username("dev_user")
                .password("dev-password")
                .build();
    }
    
    @Bean
    @Profile("prod")
    public DataSource prodDataSource() {
        return DataSourceBuilder.create()
                .url("jdbc://prod-db.example.com:3306/prod_db")
                .username("prod-user")
                .password("prod-password")
                .build();
    }
}
```

## 5️⃣ Profile의 활용 예시.
### 1️⃣ 환경별 데이터베이스 설정.
- 개발 환경에서는 로컬 DB, 운영 환경에서는 클라우드 DB를 사용하도록 분리.

### 2️⃣ 로깅 수준.
- 개발 환경에서는 DEBUG 레벨의 상세 로그를 출력하고, 운영 환경에서는 ERROR 수준의 로그만 출력.

### 3️⃣ API 키나 민감 정보 관리.
- 테스트 환경에서는 Mock API 키를 사용하고, 운영 환경에서는 실제 API 키를 적용.

### 4️⃣ 서버 포트 변경.
- 각 환경에 따라 애플리케이션의 실행 포트를 다르게 설정.

## 6️⃣ Profile의 장점.
### 1️⃣ 유지보수성 향상.
- 설정 파일을 환경별로 분리해 코드와 설정 간의 의존도를 줄이고, 변경 사항을 쉽게 관리할 수 있습니다.

### 2️⃣ 배포 자동화에 유리.
- CI/CD 파이프라인에서 환경에 맞는 프로파일을 활성화하여 배포를 자동화할 수 있습니다.

### 3️⃣ 안정성
- 환경별로 설정을 격리함으로써 잘못된 설정(예: 운영 DB를 개발 환경에서 접근하는 문제)을 방지할 수 있습니다.
