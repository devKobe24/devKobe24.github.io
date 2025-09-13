---
title: "📚[Backend Development] 계층형 대댓글에서 댓글을 삭제하면 어떤 현상이 발생할까요? 1️⃣"
tags:
    - Backend Ddevelopment
date: "2025-02-12"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 계층형 대댓글에서 댓글을 삭제하면 어떤 현상이 발생할까요? 1️⃣"
## 🍎 Intro.
- **댓글을 삭제하는 방식에는 여러 가지가 있습니다.**
    - 데이터베이스의 **외래 키(Foreign Key) 설정 및 삭제 정책에 따라 다른 현상이 발생할 수 있습니다.**

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Hierarchy_Reply.png?raw=true">

## ✅1️⃣ 댓글 2의 구조 분석.
- **댓글 2**는 **댓글 1의 자식 댓글(대댓글).**
- **댓글 3, 댓글 5**는 댓글 2의 자식 대댓글.
- 즉, **댓글 2를 삭제하면 댓글 3과 댓글 5가 고아 상태(부모 없는 상태)가 됨.**

## ✅2️⃣ 댓글 2 삭제 시 발생할 수 있는 시나리오
### 📌 시나리오 1️⃣: 연쇄 삭제 (Cascading Delete)
- **댓글 2를 삭제하면 댓글 3과 댓글 5도 함께 삭제됨.**
- **ON DELETE CASCADE** 옵션이 설정되어 있는 경우 발생.

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Hierarchy_Reply_2.png?raw=true">

#### 👉 결과.
- **삭제 후 남아있는 댓글 목록:**

```
댓글 1
댓글 4
댓글 6
```

- 댓글 3과 댓글 5까지 함께 삭제되므로, **해당 스레드 전체가 사라짐.**

### 📌 시나리오 2️⃣: 고아 댓글 처리 (Orphan Handling)
- 댓글 2를 삭제하면 **댓글 3과 댓글 5의 부모(parent_id)를 NULL로 설정.**
    - 즉, **댓글 3과 댓글 5가 독립적인 최상위 댓글이 됨.**
- **댓글 4와 댓글 6의 계층 구조는 유지됨 ➞** 댓글 6의 parent_id = 4

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Hierarchy_Reply_Orphan_Handling.png?raw=true">

#### 👉 결과.
- **삭제 후 남아있는 댓글 목록:**

```
댓글 1
댓글 3  (기존 댓글 2의 자식 → 최상위 댓글로 이동)
댓글 5  (기존 댓글 2의 자식 → 최상위 댓글로 이동)
댓글 4
  └ 댓글 6
```

### 📌 시나리오 3️⃣: 부모 댓글 대체 (Reparenting)
- 댓글 2를 삭제하면 **댓글 3과 댓글 4의 부모를 댓글 1로 변경.**
    - 즉, 댓글 2의 자식들이 **댓글 1의 직접적인 자식 댓글이 됨**

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Hierarchy_Reply_Reparenting.png?raw=true">

#### 👉 결과.
- **삭제 후 남아있는 댓글 목록:**

```
댓글 1
  └ 댓글 3
  └ 댓글 5
댓글 4
  └ 댓글 6
```
## ✅3️⃣ 댓글 2 삭제를 처리하는 방법 선택.

| 삭제 방식 | 설명 | 장점 | 단점 |
| ---------- | ---------- | ---------- | ---------- |
| 연쇄 삭제 (Cascade) | 댓글 2를 삭제하면 댓글 3, 5도 삭제 | 데이터 정합성 유지 | 유저가 예상치 못한 삭제 발생 가능 |
| 고아 댓글 처리 (Orphan) | 댓글 3과 5가 최상위 댓글이 됨 | 삭제 후에도 데이터 보존 | UI에서 댓글 관계가 깨질 수 있음 |
| 부모 댓글 대체(Reparenting) | 댓글 3과 댓글 5가 댓글 1의 자식이 됨 | 대댓글 구조 유지 | 데이터 수정이 필요 |

## ✅4️⃣ MySQL에서 삭제 처리 방식 예제.
### 1️⃣ ON DELETE CASCADE (연쇄 삭제)

```sql
ALTER TABLE comments ADD CONSTRAINT fk_parent
FOREIGN KEY (parent_id) REFERENCES comments (comment_id) ON DELETE CASCADE;
```

- 댓글 2를 삭제하면 댓글 3과 댓글 5도 자동으로 삭제됨.

### 2️⃣ 부모 댓글을 NULL로 설정 (고아 댓글 처리)

```sql
UPDATE comments SET parent_id = NULL WHERE parent_id = 2;
DELETE FROM comments WHERE comment_id = 2;
```

- 댓글 3과 댓글 5가 부모 없이 최상위 댓글로 변경됨.

### 3️⃣ 부모 댓글 변경 (Reparenting)
```sql
UPDATE comments SET parent_id = 1 WHERE parent_id = 2;
DELETE FROM comments WHERE comment_id = 2;
```

- 댓글 3과 댓글 5가 댓글 1의 자식이 됨.

## ✅5️⃣ 결론
- **댓글 2를 삭제하면 댓글 3과 댓글 5의 처리 방법에 따라 다른 결과가 발생.**
- **어떤 방식이 가장 적절한지는 비즈니스 로직과 UX에 따라 결정해야 함.**
- **보통은 부모 댓글을 삭제해도 대댓글이 남도록 처리(고아 댓글 처리 or 부모 댓글 변경)하는 경우가 많음.**
