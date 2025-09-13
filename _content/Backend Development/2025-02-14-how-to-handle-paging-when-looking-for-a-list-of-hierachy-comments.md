---
title: "📚[Backend Development] 계층형 댓글 목록 조회 시 페이징 처리 방법"
tags:
    - Backend Ddevelopment
date: "2025-02-14"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 계층형 댓글 목록 조회 시 페이징 처리 방법"
## 💡 가정:
- **계층별 오래된 순으로 페이징됨.**
- **1페이지 당 2개의 댓글을 보여줌.**

👉 **이 가정이 맞는지 검토하고, 어떻게 페이징이 이루어지는지 확인해봅시다.**

## ✅1️⃣ 기본적인 계층형 정렬 방식.
### 📌 계층형 댓글 조회 시 일반적인 정렬 규칙.
- **1. 최상위 댓글(부모 댓글)이** 먼저 정렬됨 (오래된 순).
- **2. 각 부모 댓글의 하위 댓글(자식 댓글)이** 정렬됨 (오래된 순).
- **3. 같은 계층 내에서도 오래된 순으로 정렬됨.**

#### 📌 계층형 정렬된 목록.
<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Hierarchy_%20Sorted_Comments_List.png?raw=true">

## ✅2️⃣ 1페이지 당 2개의 댓글을 보여줄 경우.
- 계층형 구조에서는 단순 LIMIT & OFFSET을 사용하면 데이터가 끊어질 수 있음.
- **트리 구조를 유지하면서 정렬된 순서로 페이징해야 함.**

### 📌 전체 페이징 처리 모습.
<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Hierarchy_%20Sorted_Comments_List_2.png?raw=true">

### 📌 1페이지 (첫 2개 댓글)
<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Hierarchy_%20Sorted_Comments_Page_1.png?raw=true">

- **최상위 댓글 1개(댓글 1) + 그에 대한 하위 댓글(댓글 2)을 포함**
- **하위 댓글이 있는 경우, 다음 댓글을 포함할지 여부는 페이징 로직에 따라 결정됨.**

### 📌 2페이지 (다음 2개 댓글)
<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Hierarchy_%20Sorted_Comments_Page_2.png?raw=true">

- **첫 번째 페이지에서 댓글 1 ➞ 댓글 2까지 가져왔으므로, 이제 댓글 2의 하위 댓글부터 표시.**
- **즉, 댓글 3과 댓글 5가 다음 페이지에 노출됨.**

### 📌 3페이지 (다음 2개 댓글)
<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Hierarchy_%20Sorted_Comments_Page_3.png?raw=true">

- **댓글 1 트리가 끝났으므로, 이제 댓글 4를 표시.**
- **댓글 4의 하위 댓글인 댓글 6도 같이 표시됨.**

## ✅3️⃣ 정리

|페이지 번호|출력되는 댓글 목록|
| -------- | -------- |
|1 페이지|댓글 1, 댓글 2|
|2 페이지|댓글 3, 댓글 5|
|3 페이지|댓글 4, 댓글 6|

- **✔️ 즉, 최상위 댓글을 기준으로 정렬하되, 계층 구조를 유지하면서 페이징이 이루어짐.** 
- **✔️ 각 댓글의 하위 댓글을 보여줄 때, 부모 댓글이 포함된 상태에서 하위 댓글이 순차적으로 정렬됨.**

## ✅4️⃣ 추가적인 고려 사항.
### ✅ 페이징 성능을 고려한 SQL 쿼리.
- 계층형 댓글 페이징을 위한 **CTE(Common Table Expression)** 또는 **Recursive Query** 활용 가능.
- `ORDER BY parent_id, created_at ASC`와 같은 정렬 방식 적용.

### ✅ UI에서의 표시 방식
- 첫 번째 페이지에서 댓글 1 ➞ 댓글 2까지만 표시할 수 있고, 첫 번째 페이지에서 댓글 1 ➞ 댓글 2 ➞ 댓글 3 ➞ 댓글 5까지 한 번에 표시할 수도 있음
- **"더 보기" 버튼을 활용하여 동적 로딩 방식도 고려 가능.**
