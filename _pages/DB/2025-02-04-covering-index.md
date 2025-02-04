---
title: "💾[Database] Covering Index는 무엇일까요?"
tags:
    - Database
    - Native SQL Query
    - MySQL
date: "2025-02-04"
thumbnail: "/assets/img/thumbnail/database.jpeg"
---

# "💾[Database] Covering Index는 무엇일까요?"
## 🍎 Intro.
- **쿼리가 요청하는 모든 데이터가 인덱스에 포함되어 있어, 테이블에 실제 데이터를 조회하지 않고도 결과를 반환할 수 있는 인덱스를 말합니다.**
- 즉, **인덱스만으로 쿼리를 처리할 수 있는 경우를 Covering Index**라고 부릅니다.
    - 이는 데이터베이스의 성능을 크게 향상시킬 수 있습니다.

## ✅1️⃣ Covering Index의 동작 방식.
- **1. 일반적인 인덱스 사용:**
    - 쿼리가 실행되면 먼저 인덱스를 탐색하여 데이터의 위치를 찾고, 이후 **테이블의 데이터를 조회**합니다.
        - 이를 **"인덱스 룩업(Index Lookup)"이라고 합니다.**
- **2. Covering Index 사용:**
    - 쿼리가 요청하는 모든 데이터가 인덱스에 포함되어 있으면, **인덱스만으로 결과를 반환할 수 있습니다.**
    - 테이블 데이터를 조회할 필요가 없으므로 **디스크 I/O를 줄이고 성능을 개선합니다.**

## ✅2️⃣ Covering Index의 조건.
- 쿼리의 **SELECT절에 포함된 컬럼과 WHERE, ORDER BY 또는 GROUP BY에 사용된 컬럼**이 모두 인덱스에 포함되어 있어야 합니다.
    - 이러한 인덱스를 **"Covering Index"라고** 부릅니다.

## ✅3️⃣ Covering Index의 예제.
### 1️⃣ 테이블 생성 및 데이터 삽입
```sql
CREATE TABLE article (
    article_id INT NOT NULL,
    board_id INT NOT NULL,
    created_at DATETIME NOT NULL,
    title VARCHAR(255),
    content TEXT,
    PRIMARY KEY (article_id)
);
```

### 2️⃣ 인덱스 생성.
```sql
CREATE INDEX idx_board_created_at ON article (board_id, created_at);
```

### 3️⃣ Covering Index 활용 쿼리.
```sql
EXPLAIN
SELECT board_id, created_at
FROM article
WHERE board_id = 1
ORDER BY created_at DESC;
```

- **인덱스 동작 설명:**
    - 쿼리에서 요청한 컬럼(board_id, created_at)이 인덱스 idx_board_created_at에 모두 포함되어 있으므로:
        - **Covering Index가 적용**됩니다.
        - MySQL은 테이블의 실제 데이터를 조회하지 않고 **인덱스만으로 결과를 반환합니다.**

## ✅4️⃣ Covering Index의 장점.
- **1. 성능 향상:**
    - 쿼리가 요청한 데이터가 모두 인덱스에 초함되어 있으므로 **테이블의 데이터를 읽지 않아도 됩니다.**
    - 디스크 I/O가 줄어들고, 쿼리 실행 시간이 크게 단축됩니다.
- **2. 효율적인 스토리지 활용:**
    - 테이블 데이터를 읽지 않고, 인덱스만으로 처리되므로 더 적은 자원을 사용합니다.
- **3. 특정 쿼리에 최적화 가능:**
    - 특정 쿼리에서 자주 사용하는 컬럼만 포함하여 설계하면, 쿼리 성능을 최적화할 수 있습니다.

## ✅5️⃣ Covering Index의 한계.
- **1. 인덱스 크기 증가:**
    - 인덱스에 많은 컬럼을 추가하면, 인덱스 크기가 커져 **삽입/삭제 성능이 저하될 수 있습니다.**
- **2. 모든 쿼리에 적용 불가능:**
    - `SELECT *`처럼 테이블에 모든 컬럼을 요청하는 쿼리에는 적용되지 않습니다.
    - 쿼리에 포함되지 않은 컬럼은 여전히 케이블에서 조회해야 합니다.
- **3. 복잡한 설계 필요:**
    - 특정 쿼리에 맞춘 커버링 인덱스를 설계하려면 **쿼리 분석**이 필요하며, 인덱스 관리가 복잡해질 수 있습니다.

## ✅6️⃣ Covering Index가 적합한 경우.
- **1. 읽기(SELECT) 작업이 많은 경우:**
    - 읽기 성능이 중요한 애플리케이션에서 유용합니다.
- **2. 자주 사용되는 특정 쿼리가 있는 경우:**
    - 쿼리 패턴을 분석하여, 필요한 컬럼만 포함하는 커버링 인덱스를 설계합니다.
- **3. 범위 검색과 정렬이 중요한 경우:**
    - 예: 날짜별 정렬, 특정 조건에 따라 필터링된 결과 조회 등.

## ✅7️⃣ Covering Inde와 EXPLAIN
- EXPLAIN을 사용하면 쿼리에서 커버링 인덱스가 적용되었는지 확인할 수 있습니다.
- **EXPLAIN 결과에서 확인 방법:**
    - Extra 열(Column)에 **Using index**가 표시되면 Covering Index가 적용된 것입니다.
- **예제:**

```sql
EXPLAIN
SELECT board_id, created_at
FROM article
WHERE board_id = 1
ORDER BY created_at DESC;
```

|id|select_type|table|type|key|key_len|ref|rows|Extra|
| -------- | -------- | --- | --- | --- | --- | --- | --- | -------- |
|1|SIMPLE|article|index|idx_board_created_at|8|const|100|**Using Index**|

- **결과 해석:**
    - Extra에 **Using index**가 표시되면, 쿼리가 커버링 인덱스를 사용했음을 의미합니다.

## 🚀 정리.
- **Covering Index**는 쿼리가 요청하는 모든 데이터가 인덱스에 포함된 경우, 테이블을 조회하지 않고도 쿼리를 처리할 수 있는 인덱스입니다.
- **장점 :** 디스크 I/O 감소, 쿼리 성능 향상.
- **단점 :** 인덱스 크기 증가, 모든 쿼리에 적용 불가능.
- MySQL에서 EXPLAIN으로 인덱스 사용 여부를 확인할 수 있으며, Extra에 **Using index**가 표시되면 Covering Index가 적용된 것입니다.
