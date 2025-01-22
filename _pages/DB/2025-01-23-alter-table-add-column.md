---
title: "💾[Database] 이미 생성된 테이블에 새로운 컬럼 추가하기."
tags:
    - Database
    - Database Optimization
date: "2025-01-23"
thumbnail: "/assets/img/thumbnail/database.jpeg"
---

# 💾[Database] 이미 생성된 테이블에 새로운 컬럼 추가하기.
## 📌 Intro.
```sql
CREATE TABLE search_pages
(
	id      BIGINT PRIMARY KEY,
	title   VARCHAR(255) NOT NULL,
	episode VARCHAR(100) NOT NULL,
	content TEXT         NOT NULL,
	tags    VARCHAR(255) NOT NULL,
	FULLTEXT idx_search (title, episode, content, tags) WITH PARSER ngram
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4
  COLLATE = utf8mb4_general_ci;
```
- ↘︎ 위와 같은 테이블을 이미 만들었다고 가정하고 나머지 글을 이어 나가겠습니다. 🙌

## ✅ `ALTER TABLE` 문 사용하기.
- ↘︎ 컬럼을 새로 추가하려면 `ALTER TABLE`문을 사용해야 합니다.
    - ↘︎ 예시:
```sql
ALTER TABLE 'table_name'
ADD COLUMN 'column_name' VARCAHR(255) NOT NULL;
```
- ↘︎ 이번에는 실제로 위 테이블에 `created_at` 이라는 컬럼을 추가해보겠습니다.
    - ↘︎ 실제 코드:
```sql
ALTER TABLE search_pages
ADD COLUMN created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP;
```
- ↘︎ **설명**
    - ↘︎ 1. `ADD COLUMN` : 테이블에 새로운 컬럼을 추가합니다.
    - ↘︎ 2. `TIMESTAMP DEFAULT CURRENT_TIMESTAMP` : 새 컬럼의 타입을 TIMESTAMP로 설정하고 기본값을 현재 시간(CURRENT_TIMESTAMP)으로 지정합니다.

## ✅ 컬럼의 위치를 지정하고 싶다면? 🙋‍♂️
- ↘︎ 컬럼을 테이블의 특정 위치에 추가하려면 `AFTER` 또는 `FIRST`를 사용할 수 있습니다.
    - ↘︎ 예시:
```sql
ALTER TABLE search_pages
ADD COLUMN created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP AFTER tags;
```
- ↘︎ 이 경우, `created_at` 컬럼은 `tags` 컬럼 바로 뒤에 추가됩니다.
