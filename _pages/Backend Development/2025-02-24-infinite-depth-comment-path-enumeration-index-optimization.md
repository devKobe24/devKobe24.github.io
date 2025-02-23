---
title: "📚[Backend Development] 무한 Depth 댓글 조회 - Path Enumeration & 인덱스 최적화"
tags:
    - Backend Ddevelopment
date: "2025-02-24"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 무한 Depth 댓글 조회 - Path Enumeration & 인덱스 최적화"
## ✅1️⃣ Path Enumeration 방식에서 댓글 path 결정
- Path Enumeration(경로 열거) 방식은 **각 댓글의 path를 계층적으로 저장하여 정렬 및 검색을 빠르게 수행하는 기법**입니다.
- 댓글이 추가될 때 **부모 댓글의 path를 상속받고,** 하위 댓글 중 **가장 큰 path**를 기준으로 새로운 path가 결정됩니다.

### 📌1️⃣ 현재 댓글 트리 구조

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/infinite-depth-example_3.png?raw=true">

- 현재 댓글 트리의 path는 다음과 같습니다.
    - 00a0z 댓글 아래에 계층적으로 정렬된 하위 댓글들이 있습니다.
    - 댓글의 path는 부모 댓글의 path를 상속받아 00000, 00001, 00002 등으로 추가됩니다.

### 📌2️⃣ 새로운 댓글 추가 요청
- 사용자가 00a0z 댓글의 하위에 새로운 댓글을 추가하려고 합니다.
    - 새로운 댓글이 추가될 path를 결정해야 합니다.
    - 기존 댓글 중 **가장 큰 path**를 찾아 이를 기준으로 +1을 적용하여 새로운 path를 생성합니다.

### 📌3️⃣ childrenTopPath 찾기

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/infinite-depth-example_5.png?raw=true">

- 새로운 댓글을 추가할 때 **현재 존재하는 하위 댓글 중 가장 큰 path(childrenTopPath)를 찾고** 해당 값에 +1을 하여 새로운 댓글의 path를 생성합니다.
    - 현재 00a0z의 하위 댓글 중 **가장 큰 path**는 00a0z 00002입니다.
    - 따라서, 새로운 댓글의 path는 00a0z 00003이 됩니다.

### 📌4️⃣ descendantsTopPath를 고려한 최종 path 결정

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/infinite-depth-example_6.png?raw=true">

- 하지만 **자식 댓글이 존재하는 경우,** 단순히 childrenTopPath만 고려하면 안 됩니다. 가장 깊은 depth까지 고려한 descendantsTopPath를 찾아야 합니다.
    - descendantsTopPath는 **부모 댓글을 포함한 모든 자식 댓글 중 가장 큰 path**입니다.
    - childrenTopPath = 00a0z 00002이므로, 새로운 댓글의 path는 00a0z 00003이 됩니다.

## ✅2️⃣ MySQL에서 descendantsTopPath 찾기.
- Path Enumeration 방식에서는 **빠른 검색을 위해 인덱스를 활용**할 수 있습니다. 특히 descendantsTopPath를 찾을 때 Backward Index Scan을 사용하면 성능을 최적화할 수 있습니다.

### 📌1️⃣ descendantsTopPath를 찾는 SQL

```sql
SELECT path FROM comment_v2
WHERE article_id = {article_id}
AND path > {parentPath} -- 부모 댓글 제외
AND path LIKE {parentPath}% -- 부모 댓글 prefix를 포함하는 모든 자식 댓글 조회
ORDER BY path DESC LIMIT 1; -- 가장 큰 path를 찾기 위해 내림차순 정렬
```

- **가장 큰 path(descendantsTopPath)를 찾을 때 내림차순 정렬을 활용**합니다.
- **LIMIT 1을 사용하여 불필요한 데이터 조회를 줄이고 성능을 최적화**합니다.

### 📌2️⃣ Backward Index Scan 활용
- MySQL에서는 ORDER BY path DESC를 사용할 때 **역순으로 인덱스를 탐색하는 Backward Index Scan을 수행합니다.**
    - path 필드에 **오름차순(ASC) 인덱스**가 설정되어 있어도, **내림차순(DESC) 정렬을 통해 가장 큰 path를 빠르게 찾을 수 있습니다.**
    - **인덱스 트리(Leaf Node) 간의 양방향 포인터**를 활용하여 역순 검색을 수행합니다.

## ✅3️⃣ MySQL Query Plan 분석(EXPLAIN)
- MySQL에서 descendantsTopPath를 찾는 쿼리의 실행 계획을 분석해봅니다.

```sql
EXPLAIN SELECT path FROM comment_v2
WHERE article_id = 1
AND path > '00a0z'
AND path LIKE '00a0z%'
ORDER BY path DESC LIMIT 1;
```

### 📌EXPLAIN 결과 분석
- **1.idx_article_id_path 인덱스 사용됨**
    - ➞ 인덱스를 활용하여 빠르게 path를 조회할 수 있음
- **2. Backward Index Scan 적용됨**
    - ➞ ORDER BY path DESC LIMIT 1을 통해 역순 탐색 수행
- **3. Using Index 적용됨**
    - ➞ 인덱스에서 직접 데이터를 가져오기 때문에 성능 최적화 가능

## ✅4️⃣ descendantsTopPath를 활용한 정렬 최적화
- Path Enumeration 방식에서는 **정렬 상태를 유지한 채 descendantsTopPath를 검색할 수 있습니다.**
    - path 정렬 상태를 유지하면서 **역순으로 인덱스를 탐색**하면, **가장 큰 path(descendantsTopPath)를 빠르게 찾을 수 있습니다.**
    - Backward Index Scan을 활용하면 **로그 시간(log time)** 내에 조회가 가능합니다.

## ✅5️⃣ 결론

### 🚀 Path Enumeration 방식에서 댓글을 추가할 때, path를 결정하는 과정.
- ✅ **가장 큰 childrenTopPath를 찾고, +1을 하여 새로운 path를 생성**
- ✅ **descendantsTopPath를 찾아 계층 구조를 유지하면서 댓글을 정렬**
- ✅ **MySQL의 Backward Index Scan을 활용하여 빠르게 descendantsTopPath를 검색**
- ✅ **대규모 데이터에서도 인덱스를 활용하여 빠른 성능을 유지**

📌 **Path Enumeration 방식을 사용할 때는, Backward Index Scan을 활용하여 최적의 성능을 보장하는 것이 중요.**
