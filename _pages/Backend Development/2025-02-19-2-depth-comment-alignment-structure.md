---
title: "📚[Backend Development] 최대 2depth 댓글 정렬 구조란 무엇일까요?"
tags:
    - Backend Ddevelopment
date: "2025-02-19"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 최대 2depth 댓글 정렬 구조란 무엇일까요?"

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/2depth-comment.png?raw=true">

## ✅ 최대 2 Depth 댓글 정렬 구조 설명.
- 최대 2 Depth(계층이 최대 2단계)까지만 허용하는 댓글 시스템의 정렬 구조는 비교적 단순하면서도 효율적입니다.

## 🏗️1️⃣ 댓글 테이블 구조.
- 최대 2 Depth 댓글을 저장하는 테이블 구조는 다음과 같습니다.

|필드명|설명|
| -------- | -------- |
|comment_id|댓글의 고유 ID (PK)|
|parent_comment_id|부모 댓글 ID (최상위 댓글이면 NULL)|
|article_id|해당 댓글이 속한 게시글 ID|
|content|댓글 내용|
|created_at|댓글 작성 시간|

### 📌 특징.
- **최상위 댓글은** parent_comment_id = NULL (예: 댓글 1, 댓글 3)
- **자식 댓글은** parent_comment_id = 부모의 comment_id (예: 댓글2, 댓글 4, 댓글 5)
- **2 Depth까지만 허용** (댓글의 댓글까지만 가능, 대댓글의 대댓글은 불가능)

## 🏗️2️⃣ 정렬 방식
- **최대 2 Depth 댓글 정렬은 다음과 같은 순서로 진행됩니다.**

```sql
ORDER BY parent_comment_id ASC, comment_id ASC
```

### 📌 정렬 기준.
- 1. parent_comment_id ASC ➞ 같은 부모 댓글을 기준으로 그룹화.
- 2. comment_id ASC ➞ 작성된 순서대로 정렬 (오래된 댓글이 먼저 출력됨).

## 🏗️3️⃣ 정렬 데이터 예시.
- 위 정렬 방식에 따라 댓글 데이터를 조회하면 다음과 같은 형태가 됩니다.

|parent_comment_id|comment_id|내용|
| -------- | -------- | -------- |
|NULL|1|댓글1 (최상위 댓글)|
|1|2|댓글2 (댓글1의 대댓글)|
|1|4|댓글4 (댓글 1의 대댓글)|
|NULL|3|댓글3 (최상위 댓글)|
|3|5|댓글5 (댓글 3의 대댓글)|

### 📌 이 정렬 방식의 장점.
- parent_comment_id를 기준으로 먼저 정렬하여 **최상위 댓글이 먼저 출력됨**
- 같은 parent_comment_id를 가진 댓글(대댓글)은 **작성 순서대로 정렬됨**
- LIMIT ?, ?을 활용하여 **페이징 처리 가능**

## 🏗️4️⃣ SQL 정렬 예제
```sql
SELECT * FROM comment
WHERE article_id = ?
ORDER BY parent_comment_id ASC, comment_id ASC
LIMIT ?, ?;
```

## 🏗️5️⃣ 페이징 처리.
- **각 페이지에서 N개 댓글을 불러올 수 있도록 LIMIT ?,? 사용**
- **최상위 댓글과 대댓글을 함께 불러오기 위해 parent_comment_id 기준 정렬 유지**
- **최대 depth가 2이므로 성능 최적화에 유리함**

## ✅6️⃣ 정리.
- ✅ **최대 2 Depth 구조 ➞** parent_comment_id를 활용해 부모-자식 관계 유지
- ✅ **정렬 순서 ➞** ORDER BY parent_comment_id ASC, comment_id ASC
- ✅ **조회 결과 ➞** 부모 댓글이 먼저, 대댓글이 뒤에 정렬됨
- ✅ **페이징 가능 ➞** LIMIT ?, ?를 활용하여 오래된 순으로 페이징 처리
