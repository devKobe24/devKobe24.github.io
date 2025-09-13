---
title: "📚[Backend Development] 최대 2 Depth 댓글 정렬 구조의 '무한 스크롤 방식'이란 무엇일까요?"
tags:
    - Backend Ddevelopment
date: "2025-02-20"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 최대 2 Depth 댓글 정렬 구조의 '무한 스크롤 방식'이란 무엇일까요?"
## ✅ 최대 2 Depth 댓글 정렬 구조의 "무한 스크롤 방식" 설명.
- 무한 스크롤 방식은 **페이지 번호 방식(LIMIT ?, OFFSET ?)을 사용하지 않고, 마지막으로 불러온 댓글의 ID를 기준으로 다음 댓글을 불러오는 방식(Keyset Pagination)입니다.**
- 이 방식은 **페이지 번호 방식보다 성능이 우수하여, 대량의 데이터를 빠르게 로드할 수 있습니다.**

## 🏗️1️⃣ 무한 스크롤 방식이란?
- 무한 스크롤 방식은 **마지막으로 불러온 댓글(lastCommentId)을 기준으로 그 이후 데이터를 가져오는 방식**입니다.

```sql
SELECT * FROM comment
WHERE article_id = ?
AND comment_id > ?
ORDER BY parent_comment_id ASC, comment_id ASC
LIMIT ?;
```

|SQL 키워드|설명|
| -------- | -------- |
|WHERE comment_id > ?|마지막 댓글 ID 이후 데이터만 가져옴|
|ORDER BY parent_comment_id ASC, comment_id ASC|부모-자식 관계를 유지하면서 정렬|
|LIMIT ?|한 번에 가져올 최대 개수 지정|

- 📌 **이 방식을 사용하면 OFFSET을 사용하지 않기 때문에 성능이 훨씬 우수합니다.**

## 🏗️2️⃣ SQL 예제 (무한 스크롤 방식)
- 예를 들어, **한 번에 3개의 댓글을 불러오도록 설정**하고, 마지막으로 불러온 댓글의 ID(lastCommentId)가 3이라고 가정합니다.

```sql
SELECT * FROM comment
WHERE article_id = ?
AND comment_id > 3
ORDER BY parent_comment_id ASC, comment_id ASC
LIMIT 3;
```

## 🏗️3️⃣ 정렬 방식
### 📌 정렬 기준.
```sql
ORDER BY parent_comment_id ASC, comment_id ASC
```

- 1. **최상위 댓글을 먼저 정렬 ➞** parent_comment_id IS NULL 순서대로 정렬
- 2. **대댓글은 같은 부모 아래에서 정렬 ➞** comment_id ASC 순서로 정렬
- 3. **페이징 없이 WHERE comment_id > lastCommentId 방식으로 조회**

## 🏗️4️⃣ 데이터 예시.
### 📌 데이터베이스에 저장된 댓글 데이터

|comment_id|parent_comment_id|내용|
| -------- | -------- | -------- |
|1|NULL|댓글1 (최상위 댓글)|
|2|1|댓글2 (댓글1의 대댓글)|
|3|NULL|댓글3 (최상위 댓글)|
|4|1|댓글4 (댓글1의 대댓글)|
|5|3|댓글5 (댓글3의 대댓글)|
|6|NULL|댓글6 (최상위 댓글)|

### 📌 무한 스크롤 방식으로 데이터 조회.
#### ✅ 첫 번째 요청(lastCommentId = 0)
```sql
SELECT * FROM comment
WHERE article_id = ?
AND comment_id > 0
ORDER BY parent_comment_id ASC, comment_id ASC
LIMIT 3;
```

### 📌 결과

|comment_id|parent_comment_id|내용|
| -------- | -------- | -------- |
|1|NULL|댓글1 (최상위 댓글)|
|2|1|댓글2 (댓글1의 대댓글)|
|4|1|댓글4 (댓글1의 대댓글)|

- 📌 **마지막 댓글 ID = 4**

#### ✅ 두 번째 요청(lastCommentId = 4)
```sql
SELECT * FROM comment
WHERE article_id = ?
AND comment_id > 4
ORDER BY parent_comment_id ASC, comment_id ASC
LIMIT 3;
```

### 📌 결과

|comment_id|parent_comment_id|내용|
| -------- | -------- | -------- |
|3|NULL|댓글3 (최상위 댓글)|
|5|3|댓글5 (댓글3의 대댓글)|
|6|NULL|댓글6 (최상위 댓글)|

- 📌 **마지막 댓글 ID = 6**

## 🏗️5️⃣ 장점과 단점.

|장점|단점|
| -------- | -------- |
|OFFSET 없이 빠른 조회 가능 (성능 최적화)|lastCommentId를 클라이언트가 유지해야 함|
|대량의 댓글이 있는 경우 효율적|댓글이 삭제될 경우 정렬이 흐트러질 가능성이 있음|
|페이지 번호 방식보다 확장성이 좋음|정렬 순서가 유지되도록 조심해야 함|

## 🚀6️⃣ 정리.

- ✅ **무한 스크롤 방식은 WHERE comment_id > lastCommentId를 사용하여 데이터 조회**
- ✅ **ORDER BY parent_comment_id ASC, comment_id ASC를 사용해 계층 구조 유지**
- ✅ **페이지 번호 방식(LIMIT ?, OFFSET ?)보다 성능이 우수하며 대량 데이터 처리에 적합**
- ✅ **마지막 댓글 ID(lastCommentId)를 유지해야 함**
- 📌 **최대 2 Depth 댓글 구조에서는 무한 스크롤 방식이 성능 최적화에 유리하며, 빠르게 댓글을 불러올 수 있습니다.**
