---
title: "💾[Database] 데이터베이스 URL이란 무엇일까요?"
tags:
    - Database
date: "2024-10-17"
thumbnail: "/assets/img/thumbnail/database.jpeg"
---

# 💾[Database] 데이터베이스 URL이란 무엇일까요?

- **데이터베이스 URL**은 애플리케이션이 **데이터베이스에 연결할 때 사용되는 고유한 주소**입니다.
- URL은 애플리케시연이 데이터베이스 서버를 찾고, 연결할 데이터베이스를 지정하기 위한 정보를 포함하고 있습니다.
- 이 URL은 데이터베이스 종류에 따라 고유한 형식으로 작성되며, 보통 **프로토콜, 서버 위치(호스트), 포트 번호, 데이터베이스 이름 등을** 포함합니다.

## 1️⃣ 데이터베이스 URL의 기본 형식.

- 데이터베이스 URL은 보통 다음과 같은 형식을 따릅니다.

```php
jdbc:<데이터베이스 유형>://<호스트>:<포트>/<데이터베이스 이름>?<옵션들>
```

- 각 부분의 의미는 다음과 같습니다.
    - `jdbc`
        - Java Database Connectivity의 약자로, JDBC URL임을 나타냅니다.
        - JDBC(Java Database Connectivity)를 사용하여 데이터베이스와 연결하는 표준 방식입니다.
    - `<데이터베이스 유형>`
        - 사용하는 데이터베이스의 유형을 나타냅니다.
            - 예를 들어, MySQL, PostgreSQL, Oracle, SQL Server 등이 여기에 들어갑니다.
    - `<호스트>`
        - 데이터베이스가 실행되고 있는 서버의 주소를 나타냅니다.
            - 로컬에서 실행될 경우 `localhost`를 사용하고, 원격 서버의 경우 서버 IP 주소나 도메인 이름을 사용합니다.
    - `<포트>`
        - 데이터베이스 서버가 수신 대기 중인 네트워크 포트 번호입니다.
            - 각 데이터베이스는 기본 포트가 있지만, 필요에 따라 다른 포트를 사용할 수도 있습니다.
                - MySQL: 3306
                - PostgreSQL: 5432
                - Oracle: 1521
    - `<데이터베이스 이름>`
        - 연결할 데이터베이스의 이름입니다.
            - 같은 데이터베이스 서버에서 여러 데이터베이스를 운영할 수 있으므로, 연결할 데이터베이스를 명시합니다.
    - `<옵션들>`
        - 추가적으로 데이터베이스 연결을 위한 옵션을 전달할 수 있습니다.
            - 예를 들어, 인코딩 방식이나 SSL 사용 여부 등의 설정이 여기에 포함될 수 있습니다.

> 📝 SSL(Secure Socket Layer)
> 
> **네트워크 상에서 데이터를 안전하게 암호화하여 전송하기 위한 보안 프로토콜입니다.**
> SSL은 클라이언트와 서버 간의 통신을 암호화하여, 데이터가 전송되는 동안 제3자가 이를 도청하거나 위조하지 못하도록 보호합니다.
> SSL은 특히 **웹 브라우저와 웹 서버 간의 안전한 통신을 보장하는 데 널리 사용되었습니다.**
> 
> 현재는 SSL의 후속 버전인 **TLS(Transport Layer Security)가** SSL을 대체하여 사용되고 있지만, 보통 사람들은 여전히 SSL이라는 용어를 사용해 TLS도 함께 지칭합니다.

## 2️⃣ 데이터베이스 URL 예시.

### 1️⃣ MySQL
```properties
jdbc:mysql://localhost:3306/mydatabase?useSSL=false&serverTimezone=UTC
```

- `mysql` : MySQL 데이터베이스 사용
- `localhost` : 데이터베이스 서버의 주소 (로컬에서 실행 중인 경우)
- `3306` : MySQL의 기본 포트 번호
- `mydatabase` : 연결할 데이터베이스 이름
- `useSSL=false` : SSL 사용 여부 (이 예시에서는 사용하지 않음)
- `serverTimezone=UTC` : 서버의 타임존 설정

### 2️⃣ PostgreSQL
```properties
jdbc:postgresql://localhost:5432/mydatabase
```

- `postgresql` : PostgreSQL 데이터베이스 사용
- `localhost` : 데이터베이스 서버 주소
- `5432` : PostgreSQL의 기본 포트 번호
- `mydatabase` : 연결할 데이터베이스 이름

### 3️⃣ Oracle
```properties
jdbc:oracle:thin:@localhost:1521:ORCL
```

- `oracle:thin` : Oracle의 JDBC 드라이버 사용
- `localhost` : 데이터베이스 서버 주소
- `1521` : Oracle의 기본 포트 번호
- `ORCL` : 데이터베이스 서비스 이름

### 4️⃣ SQL Server
```properties
jdbc:sqlserver://localhost:1433;databaseName=mydatabase;integratedSecurity=true;
```

- `sqlserver` : Microsoft SQL Server 사용
- `localhost` : 데이터베이스 서버 주소
- `1433` : SQL Sever의 기본 포트 번호
- `mydatabase` : 연결할 데이터베이스 이름
- `integratedSecurity=true` : 통합 보안(Windows 인증) 사용

### 5️⃣ H2(In-Memory Database)
```properties
jdbc:h2:mem:testdb
```

- `h2:mem` : H2 데이터베이스의 메모리 모드 사용
- `testdb` : 메모리 내에서 사용할 데이터베이스 이름

## 3️⃣ 데이터베이스 URL 사용 방법.
- 데이터베이스 URL은 주로 **Spring Boot**와 같은 프레임워크나 **JDBC(Java Database Connectivity) API**를 통해 데이터베이스에 연결할 때 설정합니다.
    - 예를 들어, Spring Boot에서 `application.properties`나 `application.yml` 파일에 데이터베이스 URL을 지정할 수 있습니다.

### 👉 `application.properties`에서 데이터베이스 URL 설정.
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mydatabase?useSSL=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=secret
```

### 👉 JDBC API로 데이터베이스 연결 예시
```java
String url = "jdbc:mysql://localhost:3306/mydatabase?useSSL=false&serverTimezone=UTC";
String username = "root";
String password = "secret";

Connection conn = DriverManager.getConnection(url, username, password);
```

## 4️⃣ 데이터베이스 URL의 구성 요소 요약.
- **프로토콜(JDBC) :** `jdbc:`로 시작하며 JDBC(Java Database Connectivity)를 사용한 데이터베이스 연결을 의미합니다.
- **데이터베이스 유형 :** 사용할 데이터베이스의 종류(MySQL, PostgreSQL, Oracle 등)를 지정합니다.
- **서버 주소(호스트와 포트) :** 데이터베이스 서버의 위치를 나타냅니다(로컬 또는 원격).
- **데이터베이스 이름 :** 연결하려는 데이터베이스의 이름을 지정합니다.
- **추가 옵션 :** SSL 사용, 타임존 설정 등 추가적인 설정이 필요할 때 URL에 포함될 수 있습니다.

## 5️⃣ 요약.
- 데이터베이스 URL은 애플리케이션이 데이터베이스에 연결할 때 사용하는 고유한 주소로, 데이터베이스 유형, 서버 주소, 포트 번호, 데이터베이스 이름 등의 정보를 포함하여 연결 설정을 정의합니다.
