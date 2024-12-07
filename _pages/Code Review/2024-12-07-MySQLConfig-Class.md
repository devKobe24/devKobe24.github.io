---
title: 💻[Code Review] MySQLConfig 클래스 코드 리뷰.
tags:
    - Code review
date: "2024-12-07"
thumbnail: "/assets/img/thumbnail/codereview.jpg"
---

# 💻[Code Review] MySQLConfig 클래스 코드 리뷰.
## 1️⃣ 전체 코드.
```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;
import org.springframework.transaction.support.TransactionTemplate;

import javax.sql.DataSource;

@Configuration
@EnableTransactionManagement
public class MySQLConfig {

    @Value("${spring.datasource.url}")
    private String url;

    @Value("${spring.datasource.username}")
    private String username;

    @Value("${spring.datasource.password}")
    private String password;

    @Value("${spring.datasource.driver-class-name}")
    private String driverClassName;

    @Bean
    public DataSourceTransactionManager transactionManager(DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }

    @Bean
    public TransactionTemplate transactionTemplate(PlatformTransactionManager transactionManager) {
        return new TransactionTemplate(transactionManager);
    }

    @Bean(name = "createUserTransactionManager")
    public PlatformTransactionManager createUserTranscationManager(DataSource dataSource) {
        DataSourceTransactionManager manager = new DataSourceTransactionManager(dataSource);
        return manager;
    }
}
```
## 2️⃣ 코드 리뷰 - 상세하게 뜯어보기 🔍
### 1️⃣ 클래스 선언부.
```java
@Configuration
@EnableTransactionManagement
public class MySQLConfig {...
```
- **`@Configuration`**
    - 이 클래스가 Spring의 설정 클래스임을 나타냅니다.
    - `@Bean` 어노테이션이 있는 메서드들을 통해 Bean을 생성 및 등록합니다.
- **`@EnableTransactioonManagement`**
    - 스프링에서 선언적 트랜잭션 관리를 활성화합니다.
    - `@Transactional` 어노테이션을 사용하는 트랜잭션 관리를 지원하도록 설정합니다.

### 2️⃣ 속성 주입
```java
    @Value("${spring.datasource.url}")
    private String url;

    @Value("${spring.datasource.username}")
    private String username;

    @Value("${spring.datasource.password}")
    private String password;

    @Value("${spring.datasource.driver-class-name}")
    private String driverClassName;
```
- **`@Value`**
    - Spring application.properties 또는 application.yml 파일에 정의된 설정 값을 가져옵니다.
    - 예를 들어, application.properties에 아래와 같은 항목이 정의 되어 있으면:
    ```java
    spring.datasource.url=jdbc:mysql://localhost:3306/mydb
    spring.datasource.username=root
    spring.datasource.password=1234
    spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
    ```
    - 이 값들이 url, username, password, driverClassName 필드에 주입됩니다.

### 3️⃣ 트랜잭션 매니저 생성.
```java
@Bean
public DataSourceTransactionManager transactionManager(DataSource dataSource) {
    return new DataSourceTransactionManager(dataSource);
}
```
- **`@Bean`**
    - 이 메서드가 반환하는 객체(`DataSourceTransactionManager`)를 Spring의 Bean으로 등록합니다.
- **`DataSourceTransactionManager`**
    - 데이터베이스 트랜잭션을 관리하는 Spring의 기본 트랜잭션 매니저 클래스입니다.
    - DataSource 객체를 받아서 트랜잭션을 관리합니다.
    - DataSource는 JDBC를 통해 MySQL 데이터베이스 연결을 제공합니다.

### 4️⃣ 트랜잭션 템플릿 생성.
```java
@Bean
public TransactionTemplate transactionTemplate(PlatformTransactionManager transactionManager) {
    return new TransactionTemplate(transactionManager);
}
```
- **`TransactionTemplate`**
    - 프로그래밍 방식으로 트랜잭션 관리를 지원하는 템플릿 클래스입니다.
    - PlatformTransactionManager를 사용하여 트랜잭션의 시작, 커밋, 롤백 등을 관리합니다.
    - 선언적 트랜잭션(`@Transaction`) 대신 프로그래밍 방식으로 트랜잭션을 제어하고자 할 때 사용됩니다.

### 5️⃣ 추가적이 트랜잭션 매니저 등록.
```java
@Bean(name = "createUserTransactionManager")
public PlatformTransactionManager createUserTransactionManager(DataSource dataSource) {
    DataSourceTransactionManager manager = new DataSourceTransactionManager(dataSource);
    return manager;
}
```
- **별도의 트랜잭션 매니저 등록**
    - `@Bean(name = "createUserTransactionManager")`로 지정하여 다른 이름의 트랜잭션 매니저를 등록합니다.
    - 이렇게 하면 여러 데이터베이스 또는 특정 작업에 대해 다른 트랜잭션 매니저를 사용할 수 있습니다.
    - 이 Bean은 필요에 따라 의존성 주입 시 이름으로 참조할 수 있습니다:
    ```java
    @Autowired
    @Qualifier("createUserTransactionManager")
    private PlatformTransactionManager transactionManager;
    ```
    
## 3️⃣ 이 코드의 주요 기능.
### 1️⃣ 데이터베이스 트랜잭션 관리.
- 데이터베이스 작업 시 트랜잭션(시작, 커밋, 롤백)을 제어합니다.

### 2️⃣ 선언적 및 프로그래밍적 트랜잭션 지원.
- `@Transactional`로 선언적 트랜잭션을 지원하며, `TransactionTemplate`을 통해 프로그래밍 방식으로 트랜잭션을 관리할 수 있습니다.

### 3️⃣ 유연한 트랜잭션 매니저.
- 하나 이상의 트랜잭션 매니저를 정의하여 다양한 트랜잭션 관리 요구를 충족합니다.

## 4️⃣ 추가로 알아두면 좋은 점.
### 1️⃣ 다중 데이터베이스 환경.
- 두 개 이상의 데이터베이스를 사용하는 경우, 각각의 데이터소스와 트랜잭션 매니저를 별도로 설정하여 관리할 수 있습니다.

### 2️⃣ DataSource Bean 생성.
- 이 코드에서는 DataSource가 주입된다고 가정했지만, DataSource를 직접 생성하려면 별도의 Bean 정의가 필요합니다:
    ```java
    @Bean
    public DataSource dataSource() {
        HikariDataSource dataSource = new HikariDataSource();
        dataSource.setJdbcUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        dataSource.setDriverClassName(driverClassName);
        return dataSource;
    }
    ```
