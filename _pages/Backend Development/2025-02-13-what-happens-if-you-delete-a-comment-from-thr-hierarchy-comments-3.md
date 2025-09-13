---
title: "📚[Backend Development] 계층형 대댓글에서 댓글을 삭제하면 어떤 현상이 발생할까요? 3️⃣"
tags:
    - Backend Ddevelopment
date: "2025-02-13"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 계층형 대댓글에서 댓글을 삭제하면 어떤 현상이 발생할까요? 3️⃣"
## ✅ "삭제 표시(Soft Delete)" 방식에서 댓글 5를 삭제하면 어떻게 될까?
### 💡 가정.
- **Soft Delete(삭제 표시) 방식을 사용 중.**
- **댓글 5는 하위 댓글이 없으므로 완전히 삭제될 것이다.**

👉 **이 가정이 맞는지 확인해 봅시다.**



## ✅1️⃣ 댓글 5 삭제 전의 구조.

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Hierarchy_Reply_Delete_1_2.png?raw=true">

- **댓글 1과 댓글 2는 삭제 표시(Soft Delete) 상태.**
- **댓글 3과 댓글 5는 남아 있음.**
    - **이 상태에서 댓글 5를 삭제하면 어떻게 될까? 🤔**

## ✅2️⃣ Soft Delete vs Physical Delete 차이점

|삭제 방식|설명|댓글 5 삭제 시 결과|
| -------- | -------- | -------- |
|Soft Delete (논리 삭제)|데이터베이스에서 삭제하지 않고 is_deleted = TRUE로 표시만 함|댓글 5가 "삭제된 댓글입니다."로 남음|
|Physical Delete (물리 삭제)|데이터베이스에서 실제 삭제|댓글 5가 완전히 제거됨|

- ✔️ **Soft Delete라면 "삭제된 댓글입니다."로 남지만, Physical Delete라면 댓글 5가 실제로 삭제됨.**
- ✔️ **댓글 5는 하위 댓글이 없기 때문에 완전 삭제(Physical Delete)되는 것이 일반적.**

## ✅3️⃣ 댓글 5 삭제 후의 새로운 구조
### ✅ Soft Delete 적용 시 (논리 삭제)

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Hierarchy_Reply_Delete_soft_delete_reple_5.png?raw=true">

- **댓글 5가 삭제 표시로 남음(Soft Delete) → "삭제된 댓글입니다."로 보임.**
- **댓글 3이 남아 있으므로 댓글 2의 구조는 유지됨.**

### ✅ Physical Delete 적용 시 (완전 삭제)

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Hierarchy_Reply_Delete_physical_delete_reple_5.png?raw=true">

- **댓글 5가 완전히 삭제된(Physical Delete)**
- **댓글 2하위에서 댓글 3만 남음.**

## ✅4️⃣ 결론: 댓글 5는 완전 삭제가 이루어질 가능성이 높음
- **Soft Delete를 적용하더라도, 댓글 5는 하위 댓글이 없기 때문에 완전히 삭제(Physical Delete) 되는 것이 일반적.**
- **댓글 5를 Soft Delete 처리할 필요가 없으며, 실제로 DB에서 제거되는 것이 최적의 방식.**
