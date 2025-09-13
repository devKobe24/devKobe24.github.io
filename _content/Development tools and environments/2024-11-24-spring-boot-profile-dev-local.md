---
title: 🛠️[개발 도구 및 환경] Spring Boot에서 Profile을 활용하여 개발 환경 분리하기.
tags:
    - Development tools
    - Enviroments
date: "2024-11-24"
thumbnail: "/assets/img/thumbnail/tools.jpg"
---

# 🛠️[개발 도구 및 환경] Spring Boot에서 Profile을 활용하여 개발 환경 분리하기.
## 1️⃣ 프로파일 분리 방법.
### 방법 1: application.yml 파일에서 분리.
- application.yml 파일은 여러 프로파일을 하나의 파일로 관리할 수 있도록 지원합니다.
- 환경별로 설정을 나누기 위해 `---` 구분자를 사용합니다.

#### 예시: application.yml
```yaml
# 기본 설정 (spring.profiles.active를 지정하지 않으면 이 설정이 사용됨)
spring:
    profiles:
        default: local
    datasource:
        url: jdbc:h2:mem:localdb
        username: local_user
        password: local_pass

---
# 개발 환경(dev) 설정.
spring:
    profiles: dev
    datasource:
        url: jdbc:mysql://dev.example.com:3306/devdb
        username: dev_user
        password: dev_pass
        
---
# 운영 환경(prod) 설정.
spring:
    profiles: prod
    datasource:
        url: jdbc:mysql://prod.example.com:3306/proddb
        username: prod_user
        password: prod_pass
```

### 방법 2: applicatioon-{profile}.yml 파일로 분리.
- 각 프로파일을 별도 파일로 관리할 수도 있습니다.
- 파일 이름에 `{profile}` 부분을 프로파일 이름으로 지정합니다.

#### 파일 구조
```bash
src/main/resources/
├── application.yml        # 기본 설정 파일
├── application-local.yml  # local 프로파일 설정
├── application-dev.yml    # dev 프로파일 설정
├── application-prod.yml   # prod 프로파일 설정
```

#### application.yml(공통 성정 및 활성화 프로파일 지정)
```yaml
spring:
    profile:
        active: local # 기본 활성화 프로파일 설정
```

#### application-local.yml
```yaml
spring:
    datasource:
        url: jdbc:h2:mem:localdb
        username: local_user
        password: local_pass
```

#### application-dev.yml
```yaml
spring:
    datasource:
        url: jdbc:mysql://dev.example.com:3306/devdb
        username: dev_user
        password: dev_pass
```

#### application-prod.yml
```yaml
spring:
    datasource:
        url: jdbc:mysql://prod.example.com:3306/proddb
        username: prod_user
        password: prod_pass
```

## 2️⃣ 실행 시 프로파일 지정 방법.
### 방법 1: 명령어 옵션으로 프로파일 지정.
```bash
java -jar library-app-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev
```

### 방법 2: 환경 변수로 프로파일 지정.
```bash
SPRING_PROFILES_ACTIVE=dev java -jar library-app-0.0.1-SNAPSHOT.jar
```

### 방법 3: IDE를 통해 프로파일 지정
```yaml
server:
  port: 8080
spring:
  config:
    activate:
      on-profile: local
  datasource:
    url: "jdbc:h2:mem:library;MODE=MYSQL;NON_KEYWORDS=USER"
    username: "sa"
    password: ""
    driver-class-name: org.h2.Driver
  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
        show_sql: true
        format_sql: true
        dialect: org.hibernate.dialect.H2Dialect
  h2:
    console:
      enabled: true
      path: /h2-console
---
server:
  port: 8080
spring:
  config:
    activate:
      on-profile: dev
  datasource:
    url: "jdbc:mysql://localhost/library"
    username: "root"
    password: ""
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    hibernate:
      ddl-auto: none
    properties:
      hibernate:
        show_sql: true
        format_sql: true
        dialect: org.hibernate.dialect.MySQL8Dialect
```
- 1. **Run/Debug Configuration 창 열기**
<img src = "https://github.com/devKobe24/images2/blob/main/IDE_PROFILE_1.png?raw=true">

- 2. **Active profiles에 지정할 환경 추가**
<img src = "https://github.com/devKobe24/images2/blob/main/IDE_PROFILE_2.png?raw=true">

## 3️⃣ 코드로 프로파일 확인 및 적용.
- Spring Boot 애플리케이션 코드에서 현재 활성화된 프로파일을 확인하거나 특정 로직을 환경에 따라 분기할 수도 있습니다.

### 활성 프로파일 확인.
```java
import org.springframework.core.env.Enviroment;
import org.springframework.stereotype.Component;

@Component
public class ProfileChecker {
    
    private final Environment environment;
    
    public ProfileChecker(Environment environment) {
        this.environment = this.environment;
    }
    
    public void printActiveProfiles() {
        String[] activeProfiles = environment.getActiveProfiles();
        System.out.println("Active Profiles: " + String.join(", ", activeProfiles));
    }
}
```

### 특정 프로파일에서만 동작하는 코드.
```java
import org.springframework.context.annotation.Profile;
import org.springframework.stereotype.Service;

@Service
@Profile("dev") // dev 환경에서만 활성화
public class DevOnlyService {
    public void run() {
        System.out.println("This service runs only in the dev environment!");
    }
}
```

## 4️⃣ 주의사항.
### 1️⃣ 환경 변수 우선 순위.
- Spring Boot는 프로파일 설정에서 아래 순서로 우선순위를 따릅니다.
    - 명령줄 인수
    - SPRING_PROFILES_ACTIVE 환경 변수
    - appilcation.yml 또는 applicatioon.properties의 spring.profiles.active 값

### 2️⃣ 운영 환경에 대한 보안
- 운영 환경에서는 application-prod.yml 같은 파일에 민감한 정보를 포함하지 않고 **외부 설정 파일**이나 **환경 변수를 활용**하는 것이 좋습니다.

### 3️⃣ 프로파일에 따른 빈(Bean) 생성 관리.
- 특정 프로파일에서만 사용해야 하는 빈은 `@Profile` 애너테이션을 사용해 관리합니다.

## 5️⃣ 결론.
- Spring Boot의 프로파일을 통해 개발 환경에 따른 설정을 효과적으로 분리하고 관리할 수 있습니다.
- application.yml 내에서 분리하거나 별도 파일로 분리하여 설정하며, 실행 시 --spring.profiles.active로 활성화할 프로파일을 지정하면 됩니다.
- 운영 환경에서는 보안을 고려해 민감 정보를 환경 변수로 관리하는 것이 권장됩니다.
