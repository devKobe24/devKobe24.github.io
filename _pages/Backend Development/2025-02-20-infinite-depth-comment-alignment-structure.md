---
title: "📚[Backend Development] 무한 depth 댓글 정렬 구조란 무엇일까요?"
tags:
    - Backend Ddevelopment
date: "2025-02-20"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 무한 depth 댓글 정렬 구조란 무엇일까요?"
## ✅ 무한 Depth 댓글 정렬 구조
- 무한 depth 댓글을 페이징 처리하기 위해서는 **트리 구조를 유지하면서 정렬하는 전략**이 필요합니다.
- 보통 **parent_comment_id + comment_id 정렬 방식**을 사용하는 2-depth 댓글과는 달리, **무한 depth의 경우 댓글의 계층 구조를 유지할 수 있도록 정렬 방식이 개선**되어야 합니다.

## 🚀1️⃣ 트리 구조 기반 정렬 방법
- 무한 depth의 댓글을 정렬하려면 다음과 같은 방식이 가능합니다.

### 1️⃣ 정렬 방식
- **root_comment_id (최상위 부모 ID) 오름차순**
- **path (트리 순서) 오름차순**
- **comment_id (작성 순서) 오름차순**
    - 이렇게 정렬하면 **트리 구조를 유지하면서 댓글을 시간순으로 정렬**할 수 있습니다.

## 🚀2️⃣ 트리 구조를 유지하는 정렬 필드

|필드명|설명|
| -------- | -------- |
|comment_id|댓글의 고유 ID (기본 키)|
|parent_comment_id|부모 댓글의 ID (최상위 댓글이면 NULL)|
|root_comment_id|최상위 부모 댓글의 ID (최상위 댓글이면 자기 자신 comment_id)|
|depth|댓글의 깊이 (0부터 시작)|
|path|트리 구조를 나타내는 정렬용 문자열|

## 🚀3️⃣ 정렬 순서 SQL 예제
### 📌 무한 Depth 정렬을 위한 ORDER BY
```sql
SELECT * FROM comment
WHERE article_id = ?
ORDER BY root_comment_id ASC, path ASC, comment_id ASC
LIMIT ?, ?;
```

### 📌 정렬 기준.
- **1. root_comment_id ASC ➞** 최상위 부모 댓글 기준으로 정렬
- **2. path ASC ➞** 트리 구조를 유지하면서 정렬
- **3. comment_id ASC ➞** 같은 depth 내에서 작성 순서대로 정렬

## 🚀4️⃣ path 필드란?
- **트리 구조를 표현하기 위해 path 필드를 활용**할 수 있습니다.
    - path는 부모-자식 관계를 명확하게 하여 정렬을 용이하게 합니다.
    - 예를 들어, path는 다음과 같은 방식으로 저장될 수 있습니다.

|comment_id|parent_comment_id|root_comment_id|depth|path|
| -------- | -------- | --- | --- | -------- |
|1|NULL|1|0|00001|
|2|1|1|1|00001.00002          |
|3|1|1|1|00001.00003|
|4|2|1|2|00001.00002.00004|
|5|4|1|3|00001.00002.00004.00005|

- 📌 **정렬 시 ORDER BY path ASC를 사용하면 계층 구조를 유지하면서 정렬 가능!**

## 🚀5️⃣ 정리

- ✅ **무한 depth 댓글 정렬을 위헤 path 또는 lft/rgt 방식이 필요**
- ✅ **정렬 순서는 root_comment_id ASC, path ASC, comment_id ASC 방식 사용**
- ✅ **페이징 처리 시 LIMIT ?,? 적용 가능**
    - 📌 **기존 2-depth 방식처럼 parent_comment_id 정렬만으로는 무한 depth 정렬이 어려우므로 path를 활용하는 것이 가장 적절합니다.**
