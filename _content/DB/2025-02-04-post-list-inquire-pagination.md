---
title: "💾[Database] 게시글 목록 조회 - 페이지 번호"
tags:
    - Database
    - Native SQL Query
    - MySQL
date: "2025-02-04"
thumbnail: "/assets/img/thumbnail/database.jpeg"
---

# "💾[Database] 게시글 목록 조회 - 페이지 번호"
## 🍎 Intro
- 이 방식은 **사용자가 특정 페이지를 요청하면 해당 페이지의 데이터만 가져와서 성능을 최적화하는 방법입니다.**

## ✅1️⃣ 페이지 번호 방식이란?
- 게시글을 여러 페이지로 나누고, 사용자가 특정 페이지를 요청하면 **해당 페이지의 데이터만 조회하는 방식**입니다.
- 일반적으로 **"LIMIT"와 "OFFSET"을 활용하여 페이지 단위로 데이터를 가져옵니다.**

### 📌1️⃣ 페이지 번호 방식에서 LIMIT과 OFFSET의 의미.
- **LIMIT과 OFFSET**은 **페이지 번호 방식(Pagination)에서** 특정 페이지의 데이터만 조회할 때 사용되는 SQL 키워드입니다.
    - LIMIT ➞ **가져올 데이터 개수**
    - OFFSET ➞ **건너뛸 데이터 개수**
        - 즉, LIMIT과 OFFSET을 함께 사용하면 **특정 페이지에 해당하는 데이터만 가져올 수 있습니다.**

### 📌2️⃣ LIMIT의 의미.
- **LIMIT이란?**
    - LIMIT은 **최대 몇 개의 행(ROW)을 가져올지 결정하는 SQL 키워드 입니다.**
    - 주로 **페이지 크기(Page Size)를** 설정할 때 사용됩니다.

#### 📌 예제: LIMIT.

```sql
SELECT *
FROM article
ORDER BY created_at DESC
LIMIT 10;
```

- 실행 결과
    - 가장 최신(created_at DESC) 기준으로 **최대 10개의 데이터만 가져옵니다.**

### 📌3️⃣ LIMIT의 특징.
- 항상 **첫 번째 데이터부터 가져옵니다.**(즉, 1페이지).
- 특정 페이지를 조회하려면 OFFSET과 함께 사용해야 합니다.

### 📌2️⃣ OFFSET의 의미.
- **OFFSET이란?**
    - OFFSET은 **앞에서부터 건너뛸 데이터 개수를 지정합니다.**
    - 보통 LIMIT과 함께 사용하여 **특정 페이지의 데이터를 가져올 때 사용**됩니다.

#### 📌 예제: OFFSET(2페이지 조회)

```sql
SELECT *
FROM article
ORDER BY created_at DESC
LIMIT 10 OFFSET 10;
```

- 실행 결과
    - OFFSET 10 ➞ 앞의 **10개 데이터를 건너 뜀.**
    - LIMIT 10 ➞ **그 다음 10개 데이터 가져옴**(즉, 11~20번째 데이터).

## ✅2️⃣ 페이지 번호 방식의 SQL 쿼리.
- MySQL에서 일반적인 **페이지 번호 기반 게시글 조회 쿼리는** 다음과 같습니다.

```sql
SELECT *
FROM article
ORDER BY created_at DESC
LIMIT 10 OFFSET 20; -- 3페이지 (페이지 크기: 10)
```
- **설명**
    - ORDER BY created_at DESC ➞ 최신 게시글부터 정렬.
    - LIMIT 10 ➞ 한 번에 10개의 게시글을 가져옴.
    - OFFSET 20 ➞ 앞의 20개 데이터를 건너뛰고(=2페이지까지 건너뜀), 3페이지부터 가져옴.

## ✅3️⃣ 대규모 데이터에서 발생하는 문제점.
- 데이터가 많을 경우 **OFFSET을 사용한 페이지 조회 방식은 성능 저하**가 발생할 수 있습니다.

### ❌성능 문제.
- 1. **OFFSET 증가 시 검색 속도 저하**
    - OFFSET은 **건너뛴 데이터도 내부적으로 읽어야 하므로,** OFFSET 값이 클수록 조회 시간이 길어짐.
- 예시

```sql
SELECT *
FROM article
ORDER BY created_at DESC
LIMIT 10 OFFSET 100000;
```
- MySQL은 **처음부터 100,000개의 데이터를 읽은 후, 10개만 반환하므로 비효율적.**

- 2. **페이지가 뒤로 갈수록 성능 저하**
    - 페이지가 커질수록 OFFSET 값이 커지면서 데이터베이스 부하 증가.
- **예시: 1,000,000번째 게시글을 조회하는 경우**

```sql
SELECT * 
FROM article
ORDER BY created_at DESC
LIMIT 10 OFFSET 999990;
```
- 이 경우 **999,990개의 데이터를 먼저 스캔한 후, 마지막 10개만 반환** ➞ 매우 느림

## ✅4️⃣ 대규모 데이터에서 페이지 번호 방식 최적화.
- 대규모 데이터에서 성능을 개선하기 위해 여러 가지 최적화 방법이 있습니다.

### ✅1️⃣ 커서 기반 페이징(Cursor-Based Pagination)
- **OFFSET을 사용하지 않고, "현재 페이지의 마지막 데이터"를 기준으로 다음 데이터를 조회하는 방식입니다.**

```sql
SELECT *
FROM article
WHERE created_at < '2025-01-31 12:00:00'
ORDER BY created_at DESC
LIMIT 10;
```

- **장점.**
    - OFFSET을 사용자지 않아 **건너뛴 데이터의 불필요한 조회를 방지.**
    - 데이터가 많아도 성능이 일정하게 유지.
    - 일반적인 SNS(페이스북, 트위터) 및 대형 게시판에서 많이 사용.

### ✅2️⃣ ID 기반 페이징
- **게시글의 고유한 ID를 활용하여 페이징을 수행하는 방식입니다.**

```sql
SELECT *
FROM article
WHERE article_id < 1050
ORDER BY article_id DESC
LIMIT 10;
```

- **장점.**
    - 특정 ID를 기준으로 데이터를 가져와서 OFFSET보다 성능이 뛰어남.
    - 인덱스를 활용하여 효율적인 검색 가능.

