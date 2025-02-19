---
title: "📚[Backend Development] 최대 2 Depth 댓글 정렬 구조의 '페이지 번호 방식'이란 무엇일까요?"
tags:
    - Backend Ddevelopment
date: "2025-02-20"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 최대 2 Depth 댓글 정렬 구조의 '페이지 번호 방식'이란 무엇일까요?"
## ✅ 최대 2 Depth 댓글 정렬 구조의 "페이지 번호 방식" 설명.
- 최대 2 Depth 댓글을 **페이지 번호 기반으로 조회하는 방식**은 **고정된 개수의 댓글을 불러오는 전통적인 페이징 방식**입니다.
    - 이를 통해 오래된 댓글부터 순서대로 불러올 수 있습니다.

## 🏗️1️⃣ 페이지 번호 방식이란?
- 페이지 번호 방식은 **특정 페이지의 댓글을 불러오기 위해 OFFSET과 LIMIT을 활용**하는 방식입니다.

```sql
SELECT * FROM comment
WHERE article_id = ?
ORDER BY parent_comment_id ASC, comment_id ASC
LIMIT ?, ?;
```

|SQL 키워드|설명|
| -------- | -------- |
|ORDER BY parent_comment_by ASC, comment_id ASC|댓글을 부모-자식 관계에 맞게 정렬|
|LIMIT ?, ?|몇 개의 데이터를 가져올지 지정|
|OFFSET|특정 페이지의 댓글을 건너뛴 후 가져옴|

## 🏗️2️⃣ 정렬 방식
- 페이지 번호 방식에서는 댓글을 **부모-자식 관계를 유지하면서 정렬**해야 합니다.
- **정렬 기준은** 다음과 같습니다.

```sql
ORDER BY parent_comment_id ASC, comment_id ASC
```

- 1. **최상위 댓글을 먼저 정렬 ➞** parent_comment_id IS NULL 순서대로 정렬
- 2. **대댓글은 같은 부모 아래에서 정렬 ➞** comment_id ASC 순서로 정렬
- 3. **페이징 처리 ➞** LIMIT ?, ? 사용

## 🏗️3️⃣ SQL 예제 (페이지 번호 기반 조회)
- 예를 들어, **한 페이지당 3개 댓글을** 가져오도록 설정하고, 2번째 페이지(page = 2)를 조회한다고 가정해보겠습니다.

```sql
SELECT * FROM comment
WHERE article_id = ?
ORDER BY parent_comment_id ASC, comment_id ASC
LIMIT 3 OFFSET 3;
```

### 📌 페이지 번호 공식.
```sql
OFFSET = (page - 1) * pageSize
```

- page = 1 ➞ OFFSET = (1-1) * 3 = 0 (첫 번째 페이지)
- page = 2 ➞ OFFSET = (2-1) * 3 = 3 (두 번째 페이지)
- page = 3 ➞ OFFSET = (3-1) * 3 = 6 (세 번째 페이지)

## 🏗️4️⃣ 데이터 예시
### 📌 데이터베이스에 저장된 댓글 데이터.

|comment_id|parent_comment_id|내용|
| -------- | -------- | -------- |
|1|NULL|댓글1 (최상위 댓글)|
|2|1|댓글2 (댓글1의 대댓글)|
|3|NULL|댓글3 (최상위 댓글)|
|4|1|댓글4 (댓글1의 대댓글)|
|5|3|댓글5 (댓글3의 대댓글)|
|6|NULL|댓글6 (최상위 댓글)|

### 📌 페이지 번호 기반 조회 결과 (한 페이지에 3개씩)
#### ✅ 1페이지 조회 (page = 1, pageSize = 3)
```sql
SELECT * FROM comment WHERE article_id = ?
ORDER BY parent_comment_id ASC, comment_id ASC
LIMIT 3 OFFSET 0;
```

|comment_id|parent_comment_id|내용|
| -------- | -------- | -------- |
|1|NULL|댓글1 (최상위 댓글)|
|2|1|댓글2 (댓글1의 대댓글)|
|4|1|댓글4 (댓글1의 대댓글)|

#### ✅ 2페이지 조회 (page = 2, pageSize = 3)
```sql
SELECT * FROM comment WHERE article_id = ?
ORDER BY parent_comment_id ASC, comment_id ASC
LIMIT 3 OFFSET 3;
```

|comment_id|parent_comment_id|내용|
| -------- | -------- | -------- |
|3|NULL|댓글3 (최상위 댓글)|
|5|3|댓글5 (댓글3의 대댓글)|
|6|NULL|댓글6 (최상위 댓글)|

- 📌 **페이지를 넘길 때마다 다음 pageSize 만큼의 데이터를 가져옵니다.**

## 🏗️5️⃣ 장점과 단점.

|장점|단점|
| -------- | -------- |
|간단하고 직관적인 페이징 구현 가능|페이지 번호가 커질수록 OFFSET이 증가하여 성능 저하|
|댓글을 정렬된 순서대로 가져올 수 있음|대량의 데이터에서 OFFSET이 클 경우 속도가 느려질 수 있음|

- 📌 **대체 방법**
    - OFFSET이 큰 경우 **"Keyset Pagination (무한스크롤 방식)"을** 사용하는 것이 더 효율적일 수 있음
    - ORDER BY parent_comment_id ASC, comment_id ASC 정렬을 유지하면서 WHERE comment_id > ? 방식을 활용하는 방식도 있음

## 🚀6️⃣ 정리.

- ✅ **페이지 번호 기반 댓글 조회는 LIMIT ?, OFFSET ?을 사용**
- ✅ **ORDER BY parent_comment_id ASC, comment_id ASC를 사용해 계층 구조 유지**
- ✅ **OFFSET 값이 클 경우 성능 저하 가능 ➞ Keyset Pagination 고려 가능**
- ✅ **최대 2 Depth 댓글 구조에서는 성능 이슈가 적고 직관적인 방식으로 구현 가능**
    - 📌 **최대 2 Depth 댓글 구조에서는 페이지 번호 방식이 효율적이며, 오래된 댓글부터 순서대로 불러오기에 적합합니다.**
