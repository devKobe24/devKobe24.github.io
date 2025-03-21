---
title: "📚[Backend Development] 계층형 대댓글에서 댓글을 삭제하면 어떤 현상이 발생할까요? 2️⃣"
tags:
    - Backend Ddevelopment
date: "2025-02-12"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 계층형 대댓글에서 댓글을 삭제하면 어떤 현상이 발생할까요? 2️⃣"
## ✅ 댓글 1을 삭제할 때, 삭제 표시만 될 것인가?
### 📌 가정
- **댓글 2는 이미 삭제 상태("삭제된 댓글입니다.")로 표시됨.**
    - **그렇다면, 댓글 1을 삭제하면 댓글 1도 삭제 표시만 될 것이다.**
        - **즉, 댓글 1이 완전히 삭제되지 않고, "삭제된 댓글입니다."로 유지될 것입니다.**
- 👉 **이 가정이 맞는지 한 번 확인해봅시다.**

## ✅1️⃣ 댓글 1 삭제 전의 구조 (댓글 2는 삭제 상태)

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Hierarchy_Reply_Delete_soft_delete_reple_2_and_before_delete_1.png?raw=true">

- **댓글 2가 삭제 상태지만, 댓글 3과 댓글 5는 유지됨.**
- **이 상태에서 댓글 1을 삭제하면 어떻게 될까?**

## ✅2️⃣ 댓글 1 삭제 처리 방식에 따른 결과.
- **댓글이 삭제될 때, 적용할 수 있는 방법은 두 가지 입니다.**

### 📌1️⃣ 연쇄 삭제 (Cascade Delete)
#### 📝 설명.
- 댓글 1이 삭제되면 **그 하위 댓글도 모두 삭제됨.**
    - 즉, **댓글 1이 삭제되면 댓글 2, 댓글 3, 댓글 5도 함께 삭제됨.**

#### 👉 결과 구조 (Cascade Delete 적용 시)

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/Hierarchy_Comments_Cascade_Delete.png?raw=true">

- ❌ **이 방식은 가정과 다르게 댓글 1이 삭제되면서 하위 댓글도 모두 삭제됨.**

### 📌2️⃣ 삭제 표시 (Soft Delete)
#### 📝 설명.
- 댓글 1을 삭제하면 **댓글 2처럼 "삭제된 댓글입니다."로 표시됨.**
    - 즉, **댓글 1이 삭제되더라도 댓글 3과 댓글 5가 남아 있기 때문에 완전히 사라지지 않음.**
    - **가정과 동일한 방식**

#### 👉 결과 구조 (Soft Delete 적용 시)

<img src = "https://github.com/devKobe24/images2/blob/main/Hierarchy_Reply_Delete_soft_delete_reple_1_and_2.png?raw=true">

- ✅ **이 방식이 사용자의 가정과 일치합니다.**
- ✅ **댓글 1은 "삭제된 댓글입니다." 상태가 되고, 댓글 3과 댓글 5는 그대로 남음.**

## ✅3️⃣ 가정이 맞는가?
- ✔ **결론: 사용자의 가정은 "Soft Delete" 방식이 적용될 경우 맞습니다.**
- ✔ **즉, 댓글 1도 삭제 상태로 표시되지만, 하위 댓글이 남아 있기 때문에 완전히 사라지지 않습니다.**
- ✔ **만약 "Cascade Delete"가 적용되었다면, 댓글 1이 삭제되면서 하위 댓글(2, 3, 5)도 모두 삭제되므로 사용자의 가정과 다릅니다.**

## ✅4️⃣ 실제 서비스에서는 어떤 방식을 사용할까?

|삭제 방식|설명|적용 서비스|
| -------- | -------- | -------- |
|Cascade (완전 삭제)|부모 댓글이 삭제되면 하위 댓글도 삭제됨|일부 게시판, 블로그|
|Soft Delete(삭제 표시 유지)|부모 댓글이 삭제되더라도 하위 댓글이 있으면 "삭제된 댓글입니다."로 유지됨|네이버 카페, 인스타그램, 페이스북, 유튜브 댓글|

- 📌 **일반적으로 커뮤니티나 SNS 서비스에서는 Soft Delete를 사용하여 부모 댓글을 "삭제된 댓글입니다."로 유지하는 경우가 많습니다.**
- 📌 **즉, 가정이 실제 서비스에서 많이 사용되는 방식과 일치합니다.**
