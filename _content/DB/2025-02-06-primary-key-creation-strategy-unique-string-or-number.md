---
title: "💾[Database] Primary Key 생성 전략 - 유니크 문자열 또는 숫자"
tags:
    - Database
    - Primary Key
    - DBMS
date: "2025-02-06"
thumbnail: "/assets/img/thumbnail/database.jpeg"
---

# "💾[Database] Primary Key 생성 전략 - 유니크 문자열 또는 숫자"
## 🍎 Intro.
- **PK(Primary Key)를 단순한 AUTO_INCREMENT가 아닌, 고유한 문자열(UUID) 또는 숫자(Snowflake, Nano ID 등)로 생성하는 방식입니다.**
- 이 방식은 **분산 시스템, 대용량 트래픽 처리, 보안성 강화** 등의 이유로 많이 사용됩니다.

## ✅1️⃣ "유니크 문자열 또는 숫자" PK 방식이 필요한 이유.
- 기존의 **AUTO_INCREMENT PK는 단순히 1씩 증가**하는 방식이므로:
    - **1. 보안에 취약 ➞** 공격자가 ID를 예상할 수 있음(/articles/1, /articles/2 ...).
    - **2. 다중 서버(샤딩) 환경에서 충돌 발생 가능 ➞** 각 서버에서 독립적인 ID 생성이 어려움.
    - **3. 데이터 마이그레이션 시 충돌 가능성 ➞** 다른 데이터베이스로 데이터를 옮길 때 ID가 겹칠 수 있음.
        - 위와 같은 문제를 해결하기 위해, **UUID, Snowflake, Nano ID 등**을 사용하여 **고유한 문자열 또는 숫자로 PK를 생성**하는 방식이 등장했습니다.

## ✅2️⃣ 유니크 문자열 또는 숫자를 이용한 PK 생성 전략.

|전략|예제|길이|특징|
| -------- | -------- | --- | -------- |
|UUID(Universally Unique Identifier)|550e8400-e29b-41d4-a716-446655440000|36자|전 세계적으로 유일한 문자열, 속도가 느릴 수 있음|
|Snowflake(Twitter ID 방식)|146789123456789012|19자|시간 기반으로 생성, 고유성 보장, 성능 최적화|
|Nano ID|V1StGXR8_Z5jdHi6B-myT|21자|랜덤 문자열, URL-safe, 짧고 충돌 확률이 낮음|
|KSUID(K-Sortable Unique ID)|0ujsswThIGTUYm2K8FjOOfXtY1K|27자|시간 정렬 가능, UUID보다 짧고 읽기 쉬움|

## ✅3️⃣ UUID(Universally Unique Identifier)
### 📌 UUID란?
- **128비트의 고유한 문자열을 생성하는 알고리즘.**
- **전 세계적으로 유일한 값을 만들 수 있음.**
- **랜덤성이 강하여 충돌 확률이 매우 낮음.**
- **길이가 길어(36자) 인덱싱 성능이 저하될 수 있음.**

### 🛠️ UUID를 PK로 사용하는 MySQL 테이블 예제.

```sql
CREATE TABLE article (
    article_id CHAR(36) PRIMARY KEY, -- UUID 사용
    title VARCHAR(255) NOT NULL,
    content TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

### 📝 Spring Boot JPA에서 UUID 적용.

```java
import jakarta.persistence.*;
import lombok.Getter;
import lombok.NoArgsConstructor;
import org.hibernate.annotations.GenericGenerator;

import java.util.UUID;

@Getter
@NoArgsConstructor
@Entity
@Table(name = "article")
public class Article {
    @Id
    @GeneratedValue(generator = "UUID")
    @GenericGenerator(name = "UUID", strategy = "org.hibernate.id.UUIDGenerator")
    @Column(name = "article_id", updatable = false, nullable = false, length = 36)
    private String articleId;
    
    private String title;
    private String content;
}
```
- ✅ **UUID를 자동으로 생성하여 PK로 사용**
- ✅ **JPA에서 @GeneratedValue(generator = "UUID")를 사용**
- ✅ **DB에는 CHAR(36)로 저장**

## ✅4️⃣ Snowflake(Twitter에서 개발한 분산 ID)
### 📌 Snowflake란?
- Twitter에서 **고유한 숫자 ID를 빠르게 생성하기 위해 개발한 알고리즘.**
- **64비트 정수 (19자리)를** 사용하여 **시간 정렬 가능.**
- **성능 최적화 & ID 충돌 없음.**
- **분산 시스템(여러 서버)에서 사용 가능.**

### 🛠️ MySQL 테이블 예제(Snowflake).

```sql
CREATE TABLE article (
    article_id BIGINT PRIMARY KEY, -- Snowflake ID 사용
    title VARCHAR(255) NOT NULL,
    content TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 📝 Spring Boot에서 Snowflake 적용.
```java
import java.time.Instant;

public class Snowflake {
    private static final Long EPOCH = 1640995200000L; // 2022-01-01 기준
    private static final Long MACHINE_ID = 1L;
    private static final Long SEQUENCE_BITS = 12L;
    
    private Long lastTimestamp = -1L;
    private Long sequence = 0L;
    
    public synchronized Long nextId() {
        Long timestamp = Instant.now().toEpochMilli();
        if (timestamp == lastTimestamp) {
            sequence = (sequence + 1) & ((1 << SEQUENCE_BITS) - 1);
            if (sequence == 0) {
                while (timestamp <= lastTimestamp) {
                    timestamp = Instant.now().toEpochMilli();
                }
            }
        } else {
            sequence = 0;
        }
        lastTimestamp = timestamp;
        return ((timestamp - EPOCH)) << (64 - 41) | (MACHINE_ID << (64 - 41 - 10)) | sequence;
    }
}
```

- ✅ **UUID보다 짧은 숫자(BIGINT)로 고유한 ID 생성.**
- ✅ **날짜 기반이므로 정렬 가능 (최근 데이터 조회 시 유리).**
- ✅ **서버가 여러 개여도 중복되지 않음.**

## ✅5️⃣ Nano ID (가볍고 짧은 랜덤 문자열)
### 📌 Nano ID란?
- UUID보다 **짧고 성능이 빠른 ID 생성기.**
- **URL-safe (URL에 사용 가능).**
- **랜덤성으로 인해 충돌 확률이 낮음.**
- **자바스크립트 환경에서도 사용 가능.**

### 🛠️ MySQL 테이블 예제 (Nano ID)
```sql
CREATE TABLE article (
    article_id VARCHAR(21) PRIMARY KEY, -- Nano ID 사용
    title VARCHAR(255) NOT NULL,
    content TEXT NOT NULL.
    created_at TIME
)
```

- **VARCHAR(21) ➞** Nano ID는 기본적으로 **21자 길이를** 가지므로 테이블에서도 VARCHAR(21)로 설정.

### 📝 Spring Boot에서 Nano ID 적용.
- Java에서는 **Nano ID 라이브러리(com.aventrix.jnanoid)를** 사용하여 Nano ID를 생성할 수 있습니다.

### 1️⃣ Maven 또는 Gradle에 Nano ID 라이브러리 추가
- **Maven 사용 시**

```xml
<dependency>
    <groupId>com.aventrix.jnanoid</groupId>
    <artifactId>jnanoid</artifactId>
    <version>2.0.0</version>
</dependency>
```

- **Gradle 사용 시**

```gradle
implementation 'com.aventrix.jnanoid:jnanoid:2.0.0'
```

### 2️⃣ Entity에서 Nano ID를 사용하여 Primary Key 설정.
```java
import jakarta.persistence.*;
import lombok.Getter;
import lombok.NoArgsConstructor;
import org.hibernate.annotations.GenericGenerator;
import org.hibernate.annotations.Parameter;
import com.aventrix.jnanoid.jnanoid.NanoIdUtils;

@Getter
@NoArgsConstructor
@Entity
@Table(name = "article")
public class Article {
    @Id
    @Column(name = "article_id", updatable = false, nullable = false, length = 21)
    private String articleId;

    private String title;
    private String content;

    @PrePersist
    public void generateId() {
        this.articleId = NanoIdUtils.randomNanoId(); // Nano ID 생성
    }
}
```

### ✅ Nano ID 방식의 장점.
- **1. UUID보다 짧음(21자) ➞ 성능 최적화**
- **2. 랜덤한 값이므로 보안성 우수 (ID 예측 불가능)**
- **3. URL-safe ➞ URL에서 안전하게 사용 가능**
- **4. 서버 부담이 적고, 빠른 속도로 생성 가능**

### ❌ Nano ID 방식의 단점.
- **1. UUID보다 충돌 확률이 높지만, 충분히 낮은 수준.**
- **2. 정렬이 어려움 ➞ 시간 기반이 아느므로 최근 데이터 정렬이 필요할 경우 적합하지 않음.**
