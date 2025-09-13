---
title: "💾[Database] Primary Key 생성 전략 - DB auto increment(2)"
tags:
    - Database
    - Primary Key
    - DBMS
date: "2025-02-06"
thumbnail: "/assets/img/thumbnail/database.jpeg"
---

# "💾[Database] Primary Key 생성 전략 - DB auto increment(2)"
## ✅1️⃣ PK와 Unique Index를 분리하는 이유.
- **"PK는 데이터베이스 내에서만 식별자로 사용하고, 애플리케이션에서의 식별자는 별도의 Unique Index를 사용할 수도 있다."**
    - 위 개념은 데이터베이스 내에서 관리하는 **PK**와 **애플리케이션에서 사용하는 식별자를 분리하는 방식입니다.**
    - 즉, **PK로 id(Auto Increment)를 사용하고, 애플리케이션에서 article_id(Snowflake 또는 UUID)를 활용하는 구조**입니다.

## ✅2️⃣ 예제 시나리오.
### 📌1️⃣ PK: id(AUTO_INCREMENT)
- DB에서 **기본 키** 역할을 수행하며, 정수(BIGINT) 타입.
- 데이터가 추가될 때 **1씩 자동 증가**.
- **데이터베이스 내에서 관계(Join, Indexing, 검색 속도)를 최적화**하는 용도로 사용.

### 📌2️⃣ Unique Index: article_id(Snowflake 또는 UUID)
- 애플리케이션에서 **외부 시스템과 연동하거나 API 응답에서 사용하는 식별자**.
- UUID 또는 Snowflake를 사용하여 **전역적으로 고유한 값**을 생성.
- Unique Index를 설정하여 **중복되지 않도록 보장**.

## ✅3️⃣ DDL 예제(MySQL 기준).
```sql
CREATE TABLE article (
    id BIGINT AUTO_INCREMENT PRIMARY KEY, -- PK (DB 내부에서 사용)
    article_id VARCHAR(36) UNIQUE NOT NULL, -- UUID 또는 Snowflake (API에서 사용)
    title VARCHAR(255) NOT NULL,
    content TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    modified_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
)
```

- **id ➞** AUTO_INCREMENT를 사용하여 기본 키 역할 수행.
- **article_id ➞** UUID 또는 Snowflake를 사용하며, UNIQUE INDEX 설정.
- **article_id를 API를 클라이언트와 데이터를 주고받을 때 사용** (예: REST API)

## ✅4️⃣ Spring Boot JPA Entity 예제.
```java
import jakarta.persistence.*;
import lombok.*;

import java.time.LocalDateTime;

@Getter
@ToString
@Entity
@Table(name = "article")
@NoArgsconstructor(access = AccessLevel.PROTECTED)
public class Article {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // DB Auto Increment
    private Long id;
    
    @Column(unique = true, nullable = false, length = 36) // UUID or Snowflake
    private String articleId;
    
    private String title;
    private String content;
    private LocalDateTime createdAt;
    private LocalDateTime modifiedAt;
    
    // 정적 팩토리 메서드로 UUID 또는 Snowflake ID 생성
    public static Article create(String title, String content) {
        Article article = new Article();
        article.articleId = generateUniqueId(); // UUID 또는 Snowflake 사용
        article.title = title;
        article.content = content;
        article.createdAt = LocalDateTime.now();
        article.modifiedAt = article.createdAt;
        
        return article;
    }
    
    public void update(String title, String content) {
        this.title = title;
        this.content = content;
        this.modifiedAt = LocalDateTime.now();
    }
    
    // Snowflake 또는 UUID ID 생성 메서드
    private static String generateUniqueId() {
        return java.util.UUID.randomUUID().toString(); // UUID 사용 예제
    }
}
```

- **id: 데이터베이스가 관리하는 자동 증가 값.** 
- **articleId: 애플리케이션에서 UUID를 사용하여 생성.**
- generateUniqueId()는 **UUID를 사용했지만 Snowflake를 적용할 수도 있음**

## ✅5️⃣ 데이터 삽입 예제.
### ✅ JPA를 사용한 데이터 저장.
```java
Article article = Article.create("My Title", "This is content.");
articleRepository.save(article);
```

### ✅ DB에 저장된 데이터 예시.
```sql
SELECT * FROM article;
```

|id|article_id|title|content|created_at|modified_at|
| -------- | -------- | --- | --- | --- | -------- |
|1|550e8400-e29b-41d4-a716-446655440000|My Title|This is content|2024-02-05 12:00:00|2024-02-05 12:00:00|

- id는 **자동 증가(AUTO_INCREMENT)**
- article_id는 **UUID를 기반으로 생성됨**
- API 응답에서 article_id를 제공하여 클라이언트와 데이터 주고받기 용도로 사용

## ✅6️⃣ 왜 PK와 Unique Index를 분리할까?
### 1️⃣ Auto Increment PK는 DB 성능 최적화에 유리.
- BIGINT AUTO_INCREMENT는 **정수 값이므로 Join, Indexing이 빠름.**
- UUID(VARCHAR)를 PK로 사용하면 **인덱스 성능이 떨어짐** (특히 MySQL InnoDB에서).
- **Primary Key는 내부적인 데이터 관리 용도로만 사용**하는 것이 일반적.

### 2️⃣ UUID(Snowflake)는 애플리케이션 외부 식별자로 적합.
- UUID나 Snowflake는 **전역적으로 고유한 값**을 생성하므로 **다른 시스템과 연동할 때 유리.**
- id(Auto Increment)를 사용하면 **다른 DB로 데이터를 이동할 때 충돌할 가능성이 있음.**

### 3️⃣ 데이터 마이크레이션 & 분산 시스템에 유리.
- UUID/Snowflake는 **서버가 여러 개 있어도 고유한 값 생성 가능.**
- **Auto Increment는 단일 DB에서만 고유 보장됨 ➞** 샤딩(Sharding) 환경에서는 충돌 가능.

## ✅7️⃣ API 설계에서의 차이점.
### 1️⃣ id(Auto Increment)를 API에서 노출할 경우.

```json
{
    "id": 1,
    "title": "My Title",
    "content": "This is content",
    "createdAt": "2024-02-05T12:00:00"
}
```

- 보안상 문제가 될 수 있음(예: ID를 연속적으로 증가시키며 조회 가능)

### 2️⃣ articleId(UUID/Snowflake)를 API에서 노출할 경우 (권장)
```json
{
    "articleId": "550e8400-e29b-41d4-a716-446655440000",
    "title": "My Title",
    "content": "This is content",
    "createdAt": "2024-02-05T12:00:00"
}
```

- **UUID 또는 Snowflake는 보안성이 높고, 분산 환경에서도 안전함.**
- 클라이언트는 articleId를 사용하여 데이터를 조회하고 업데이트할 수 있음.

## ✅8️⃣ API 컨트롤러에서 articleId 활용.

```java
@GetMapping("/v1/articles/{articleId}")
public ArticleResponse getArticle(@PathVariable String articleId) {
    Article article = articleRepository.findByArticleId(articleId)
        .orElseThrow(() -> new RuntimeException("Article not found"));
    return ArticleResponse.from(article);
}
```

- **id(PK)가 아니라 articleId를 기반으로 조회 ➞** 클라이언트가 예측 불가능한 ID 사용 가능.
- findByArticleId()는 **Unique Index를 활용하여 빠르게 조회 가능.**

## ✅9️⃣ 결론.

|PK(id)|Unique Index(article_id)|
| -------- | -------- |
|AUTO_INCREMENT로 생성됨|UUID 또는 Snowflake로 생성됨|
|**DB에서만 사용**(Join, Index 최적화)|**API와 외부 시스템에서 사용**|
|**연속적인 숫자**(예: 1,2,3...)|**랜덤 값**(예: UUID 550e8400-e29b...)|
|**데이터베이스 성능 최적화**|**보안성과 확장성 증가**|
|**마이그레이션 시 충돌 위험**|**분산 시스템에서 안전함**|

## 🚀 마무리
- **DB 내부 관리용 ID(AUTO_INCREMENT)와 API 식별용 ID(UUID/Snowflake)를 분리하는 것이 베스트 프랙티스.**
- **API에서는 articleId(UUID/Snowflake)만 노출**하여 보안성과 확장성을 확보.
- **데이터베이스 성능을 유지하면서 애플리케이션 레벨에서 유니크한 식별자를 가질 수 있음.**
