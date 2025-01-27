---
title: "💾[Database] MySQL의 쿼리를 기반으로 작성된 네이티브 SQL 쿼리 분석."
tags:
    - Database
    - Native SQL Query
    - MySQL
date: "2025-01-28"
thumbnail: "/assets/img/thumbnail/database.jpeg"
---

# 💾[Database] MySQL의 쿼리를 기반으로 작성된 네이티브 SQL 쿼리 분석.
## 📝 Intro
- 아래의 Query 문은 **"MySQL"의** 쿼리를 기반으로 작성된 Native SQL Query입니다.
    - **"Spring Data JPA"에서** `@Query` 어노테이션을 통해 사용되며, 특정 boardId에 속한 게시물(Article)을 페이지네이션 방식으로 조회하는 역할을 합니다.

```sql
@Query(
    value = "SELECT article.article_id, article.title, article.board_id, article.writer_id, " +
			"article.created_at, article.modified_at " +
			"FROM (" +
			"   SELECT article_id FROM article " +
			"   WHERE board_id = :boardId " +
			"   ORDER BY article_id DESC " +
			"   LIMIT :limit OFFSET :offset " +
			") t LEFT JOIN article ON t.article_id = article.article_id ",
	nativeQuery = true
)
List<Article> findAll(
	@Param("boardId") Long boardId,
	@Param("offset") Long offset,
	@Param("limit") Long limit
);
```

## ✅1️⃣ Query 문 분석.
```sql
SELECT article.article_id, article.title, article.board_id, article.writer_id,
       article.created_at, article.modified_at
FROM (
    SELECT article_id
    FROM article
    WHERE board_id = :boardId
    ORDER BY article_id DESC
    LIMIT :limit OFFSET :offset
) t
LEFT JOIN article ON t.article_id = article_article_id
```

### 1️⃣ 내부 서브쿼리.
```sql
SELECT article_id
FROM article
WHERE board_id = :boardId
ORDER BY article_id DESC
LIMIT :limit OFFSET :offset
```

- **목적 :** 특정 게시판(board_id)에서 필요한 게시물의 ID만 추출.
- **세부 내용:**
    - **board_id = :boardId**
        - ↘︎ 전달 받은 boardID에 해당하는 게시물만 조회
    - **ORDER BY article_id DESC**
        - ↘︎ article_id를 지군으로 내림차순으로 정렬.(최신 게시물 순)
    - **LIMIT :limit OFFSET :offset**
        - ↘︎ 페이지네이션 처리를 위한 키워드.
            - **:limit 👉** 한 페이지에 표시할 게시물 수.
            - **:offset 👉** 몇 번째부터 데이터를 가져올지 결정.

### 2️⃣ 외부 쿼리.
```sql
SELECT article.article_id, article.title, article.board_id, article.writer_id,
       article.created_at, article.modified_at
FROM ... LEFT JOIN article ON t.article_id = article.article_id
```
- **목적 :** 서브쿼리세어 가져온 article_id를 기준으로 게시물을 상세 정보를 조회.
- **세부 내용 :**
    - **LEFT JOIN**
        - ↘︎ 서브쿼리(t)와 article 테이블을 조인.
        - ↘︎ t.article_id와 article.article_id가 일치하는 데이터를 가져옴.
    - **SELECT ...**
        - ↘︎ 게시물의 주요 정보를 선택적으로 가져옴.

### 3️⃣ Query의 동작 과정.
#### 1️⃣ 서브쿼리 실행.
- ↘︎ article 테이블에서 board_id가 :boardId인 게시물의 ID를 최신 순으로 정렬.
- ↘︎ LIMIT와 OFFSET을 사용해 필요한 게시물 ID만 선택.

#### 4️⃣ 외부 쿼리 실행.
- ↘︎ 서브 쿼리에서 가져온 article_id를 기준으로 article 테이블의 나머지 데이터를 가져옴.
- ↘︎ 각 게시물의 ID, 제목, 게시판 ID, 작성자 ID, 생성/수정 시간을 반환.

### 4️⃣ 파라미터.
- @Param("boardId") Long boardId
    - ↘︎ 특정 게시판의 ID를 나타냅니다.
    - ↘︎ WHERE board_id = :boardId 조건에 사용됩니다.
- @Param("offset") Long offset
    - ↘︎ 페이지네이션의 시작 지점을 나타냅니다.
    - ↘︎ 예: 0이면 첫 번째 데이터부터, 10이면 11번째 데이터부터 조회.
- @Param("limit") Long limit
    - ↘︎ 한 페이지에 가져올 데이터의 수를 나타냅니다.
    - ↘︎ 예: 10이면 한 번에 10개의 데이터를 반환.
