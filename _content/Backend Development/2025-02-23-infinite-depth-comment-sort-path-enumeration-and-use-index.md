---
title: "📚[Backend Development] 무한 Depth 댓글 정렬 - Path Enumeration(경로 열거) 방식과 인덱스 활용"
tags:
    - Backend Ddevelopment
date: "2025-02-23"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] Path Enumeration 방식에서 댓글의 경로(Path) 결정 과정."
## ✅1️⃣ 댓글 트리 구조에서 새로운 댓글의 path 결정하기
- Path Enumeration(경로 열거) 방식을 사용할 때, 새로운 댓글이 추가될 경우 해당 댓글의 path를 결정하는 과정이 중요합니다.
- 댓글은 **부모 댓글의 path를 상속받으며,** 가장 마지막에 있는 댓글의 path를 기준으로 새로운 path가 생성됩니다.

### 📌1️⃣ 기존 댓글 트리 구조.
- 새로운 댓글이 추가되기 전의 상태를 확인합니다.
    - 최상위 댓글 00a0z가 있고, 하위 댓글들이 계층적으로 존재합니다.
    - path는 부모 댓글의 path를 상속받아 생성되며, 댓글이 추가될수록 순차적으로 증가합니다.

### 📌2️⃣ 새로운 댓글 추가 요청.
- 어떤 사용사용자가 00a0z 댓글의 하위에 새로운 댓글을 추가하려고 합니다.
- 하지만, 현재 00a0z의 하위 댓글들이 존재하기 때문에 새로운 path를 결정해야 합니다.
        - **현재 00a0z의 하위 댓글 중 가장 큰 path(childernTopPath)를 찾아야 합니다.**

### 📌3️⃣ childrenTopPath 찾기
- 새로운 댓글을 추가할 때는, **현재 존재하는 하위 댓글 중 가장 큰 path(childrenTopPath)를 찾고** 해당 값에 +1을 하여 새로운 댓글의 path를 생성합니다..
    - 현재 00a0z의 하위 댓글 중에서 가장 큰 path는 00a0z 00002입니다.
    - 따라서, 새로운 댓글의 path는 00a0z 00003이 됩니다.

## ✅2️⃣ Path Enumeration 방식에서 인덱스 활용
- Path Enumeration 방식에서는 **빠른 검색을 위해 인덱스를 활용할 수 았습니다.**
- 특히 **descendantsTopPath를 찾을 때 Backward Index Scan을 사용하면 빠른 조회가 가능합니다.**

### 📌1️⃣ descendantsTopPath를 찾는 과정.
- 새로운 댓글을 추가할 때, 가장 큰 descendantsTopPath를 찾아야 합니다.
- path 정렬 상태를 유지하면서 **역순으로 인덱스를 탐색**하면, **가장 큰 path(descendantsTopPath)를 빠르게 찾을 수 있습니다.**
- Backward Index Scan을 활용하면 **로그 시간(log time)** 내에 조회가 가능합니다.
- 먼저 아래의 쿼리를 이용하여 descendantsTopPath를 DB에서 찾을 수 있습니다:

```sql
SELECT path FROM comment_v2
WHERE article_id = {article_id}
AND path > {parentPath} // parent 본인은 미포함 검색 조건
AND path like {parentPath}% // parentPath를 prefix로 하는 모든 자손 검색 조건
ORDER BY path DESC LIMIT 1; // 조회 결과에서 가장 큰 path
```

### 📌2️⃣ Backward Index Scan 활용
- path 필드에 **오름차순(ASC) 인덱스**가 설정되어 있어도, **내림차순(DESC) 정렬을 통해 가장 큰 path를 빠르게 찾을 수 있습니다.**
- **인덱스 트리(Leaf Node) 간의 양방향 포인터를 활용하여 역순 검색을 수행합니다.**
- 역순으로 스캔하여 LIMIT 1을 적용하면 **가장 큰 path를 빠르게 찾을 수 있습니다.**

## ✅3️⃣ MySQL Query Plan 분석 (EXPLAIN)
- MySQL에서 **descendantsTopPath를 찾는 쿼리의 실행 계획을 분석해봅니다.**

```sql
EXPLAIN SELECT path FROM comment_v2
WHERE article_id = 1
AND path > '00a0z'
AND path LIKE '00a0z%'
ORDER BY path DESC LIMIT 1;
```

### 📌 EXPLAIN 분석 결과
- 1. **idx_article_id_path 인덱스 사용됨**
    - → 인덱스를 활용하여 빠르게 path를 조회할 수 있음
- 2. **Backward Index Scan 적용됨**
    - → ORDER BY path DESC LIMIT 1을 통해 역순 탐색 수행
- 3. **Using Index 적용됨**
    - → 인덱스에서 직접 데이터를 가져오기 때문에 성능 최적화 가능

## ✅4️⃣ 결론

### 🚀 Path Enumeration(경로 열거) 방식에서 댓글을 추가할 때, path를 결정하는 과정.

- ✅ **가장 큰 childrenTopPath를 찾고, +1을 하여 새로운 path를 생성**
- ✅ **descendantsTopPath를 찾아 계층 구조를 유지하면서 댓글을 정렬**
- ✅ **MySQL의 Backward Index Scan을 활용하여 빠르게 descendantsTopPath를 검색**
- ✅ **대규모 데이터에서도 인덱스를 활용하여 빠른 성능을 유지**

📌 **Path Enumeration 방식을 사용할 때는, Backward Index Scan을 활용하여 최적의 성능을 보장하는 것이 중요합니다.**
