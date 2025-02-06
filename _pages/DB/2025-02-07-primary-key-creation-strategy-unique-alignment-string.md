---
title: "💾[Database] Primary Key 생성 전략 - 유니크 정렬 문자열"
tags:
    - Database
    - Primary Key
    - DBMS
date: "2025-02-07"
thumbnail: "/assets/img/thumbnail/database.jpeg"
---

# "💾[Database] Primary Key 생성 전략 - 유니크 정렬 문자열"
## 🍎 Intro.
- **Primary Key(PK)를 생성할 때,** 각 ID가 **고유하면서도 생성된 순서대로 정렬이 가능**하도록 만드는 방식입니다.
- 이 방식은 **UUID처럼 충돌 없는 고유성을 유지하면서도, 정렬이 가능하도록 시간 기반의 요소를 포함**하는 것이 핵심입니다.

## ✅1️⃣ 유니크 정렬 문자열 방식이 필요한 이유.
- 기존의 PK 생성 방식에는 몇 가지 문제점이 있습니다.

### ❌1️⃣ UUID의 문제점.
- UUID는 **충돌 없이 유니크하지만, 정렬이 불가능합니다.**
- ID가 **랜덤하게 생성**되기 때문에 **최근 데이터 정렬이 어렵습니다.**
- **문자열 길이가 길고 인덱싱 성능 저하** 가능성이 있습니다.

### ❌2️⃣ AUTO_INCREMENT(숫자 증가 방식)의 문제점.
- 여러 서버(분산 환경)에서 사용하면 **중복 발생 가능성이 있습니다.**
- ID가 예측 가능하여 **보안에 취약점이 존재합니다.**

### ✅ 유니크 정렬 문자열 방식이 해결하는 문제.
- **UUID처럼 고유하지만, 시간순으로 정렬이 가능합니다.**
- **서버가 여러 개여도 중복되지 않습니다 (분산 환경에서도 안정적).**
- **ID가 일정한 패턴을 가지므로, 인덱싱 성능이 더 좋습니다.**

## ✅2️⃣ 유니크 정렬 문자열 방식 종류


|전략|예제|길이|특징|
| -------- | -------- | --- | -------- |
|KSUID|0ujsswThIGTUYm2K8FjOOfXtY1K|27자|UUID보다 짧고, 시간 정렬 가능|
|ULID|01F8MECHZX3TBXYN5RRTG1X3J6|26자|UUID보다 짧고, 생성 순서대로 정렬 가능|
|Sonyflake|404168231342172160|19자|Snowflake와 유사, 빠른 속도|
|Short UUID + Timestamp|20250131235959-8f9c7d3a|가변|날짜 기반으로 정렬 가능|

- 각 방식은 **시간 정보가 포함되어 생성된 순서대로 정렬이 가능**하도록 설계되어 있습니다.

## ✅3️⃣ 유니크 정렬 문자열 방식의 예제 코드.
### 🛠️1️⃣ ULID(Universally Unique Lexicographically Sortable Identifier)
- **ULID는 UUID보다 짧고, 생성 순서대로 정렬이 가능한 ID 방식입니다.**

### 📌 ULID 특징.
- **UUID보다 10자 정도 짧음 (26자)**
- **생성된 순서대로 정렬 가능 (시간 정보 포함)**
- **URL-safe (특수문자가 없음)**
- **대소문자 구분 (Base32 사용)**

### 📝 Spring Boot에서 ULID 사용하기.
- ULID를 사용하려면 **ulid 라이브러리**를 추가해야 합니다.

### 1️⃣ Maven 또는 Gradle에 라이브러리 추가.
```xml
<dependency>
    <groupId>de.huxhorn.sulky</groupId>
    <artifactId>de.huxhorn.sulky.ulid</artifactId>
    <version>8.3.0</version>
</dependency>
```

### 2️⃣ ULID 기반 ID 생성.
```java
import de.huxhorn.sulky.ulid.ULID;
import jakarta.persistence.*;
import lombok.Getter;
import lombok.NoArgsConstructor;

@Getter
@NoArgsConstructor
@Entity
@Table(name = "article")
public class Article {
    @Id
    @Column(name = "article_id", updatable = false, nullable = false, length = 26)
    private String articleId;

    private String title;
    private String content;

    @PrePersist
    public void generateId() {
        ULID ulid = new ULID();
        this.articleId = ulid.nextULID(); // ULID 생성
    }
}
```

### ✅ ULID의 주요 장점.
- nextULID()를 호출할 때마다 새로운, 정렬 가능한 고유 ID가 생성됨.
- 시간 정렬이 가능하여 ORDER BY article_id ASC로 최근 데이터를 쉽게 조회 가능.

### 🛠️2️⃣ KSUID(K-Sortable Unique ID)
- KSUID는 ULID와 비슷하지만, 더 긴 ID(27자)를 사용하며 **UUID보다 정렬이 쉽습니다.**

### 📌 KSUID의 특징.
- **시간 기반 정렬 기능.**
- **UUID보다 짧고 읽기 쉬움.**
- **고유성이 보장됨,**

### 📝 Spring Boot에서 KSUID 적용.
### 1️⃣ Maven 또는 Gradle에 라이브러리 추가.
```xml
<dependency>
    <groupId>com.github.ksuid</groupId>
    <artifactId>ksuid</artifactId>
    <version>1.0.0</version>
</dependency>
```

### 2️⃣ KSUID 기반 ID 생성.
```java
import com.github.ksuid.Ksuid;
import jakarta.persistence.*;
import lombok.Getter;
import lombok.NoArgsConstructor;

@Getter
@NoArgsConstructor
@Entity
@Table(name = "article")
public class Article {
    @Id
    @Column(name = "article_id", updatable = false, nullable = false, length = 27)
    private String articleId;

    private String title;
    private String content;

    @PrePersist
    public void generateId() {
        this.articleId = Ksuid.newKsuid().toString(); // KSUID 생성
    }
}
```

### ✅ KSUID의 주요 장점
- newKsuid().toString()을 호출할 때마다 새로운, 정렬 가능한 고유 ID가 생성됨.
- **UUID보다 짧고, 더 빠름.**
- **정렬이 가능하여 최근 데이터 조회가 쉽다.**

### 🛠️3️⃣ Short UUID + Timestamp (커스텀 방식)
- 만약 직접 **UUID를 짧게 변형하고, 시간 정보와 조합**하여 고유한 정렬 문자열을 만들 수도 있습니다.

### 📌 Short UUID + Timestamp의 특징.
- **날짜 기반으로 정렬 가능**
- **UUID보다 짧고, 가독성이 좋음**
- **특정 비즈니스 로직에 맞게 커스텀 가능**

### 📝 Java에서 Short UUID + Timestamp 적용
```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.UUID;

public class CustomIdGenerator {

    public static String generateId() {
        String timestamp = LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyyMMddHHmmss"));
        String shortUUID = UUID.randomUUID().toString().replaceAll("-", "").substring(0, 8);
        return timestamp + "-" + shortUUID;
    }

    public static void main(String[] args) {
        System.out.println(generateId()); // 예: 20250201093045-8f9c7d3a
    }
}
```

### ✅ 이 방식의 장점
- **정렬 기능 (날짜 기반)**
- **UUID보다 짧고 가독성이 좋음**
- **UUID의 충돌 방지 기능을 유지**

## ✅4️⃣ 유니크 정렬 문자열 방식의 비교

|전략|예제|길이|특징|
| -------- | -------- | --- | -------- |
|ULID|01F8MECHZX3TBXYN5RRTG1X3J6|26자|UUID보다 짧고, 생성 순서대로 정렬 가능|
|KSUID|0ujsswThIGTUYm2K8FjOOfXtY1K|27자|UUID보다 짧고, 시간 정렬 가능|
|Short UUID + Timestame|20250201093045-8f9c7d3a|가변|날짜 기반으로 정렬 가능|

## ✅5️⃣ 결론.
- **UUID는 정렬이 어렵고 길다.**
- **ULID, KSUID 같은 방식은 유니크하면서도 생성된 순서대로 정렬 가능.**
- **Short UUID + Timestamp는 직접 커스텀하여 사용할 수도 있음.**

