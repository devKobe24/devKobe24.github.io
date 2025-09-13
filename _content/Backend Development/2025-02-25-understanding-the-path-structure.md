---
title: "📚[Backend Development] Path 구조의 이해."
tags:
    - Backend Ddevelopment
date: "2025-02-25"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] Path 구조의 이해."
## ✅1️⃣ "00a0z 00002"의 하위 댓글은 무엇일까요?

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/infinite-depth-example_7.png?raw=true">

- "00a0z 00002"의 하위 댓글은 **"00a0z 00002 00000" 입니다.**
- 즉, "00a0z 00003"이 아니라 **"00a0z 00002 00000"이 하위 댓글입니다.**

## ✅2️⃣ 하위 댓글이 "00a0z 00002 00000"인 이유?
### 📌1️⃣ Path 구조 이해.
- CommentPath에서 댓글의 path는 **부모 댓글의 path + 하위 댓글의 5자리 문자열**로 구성됩니다.
    - 각 댓글의 path는 DEPTH_CHUNK_SIZE = 5 기준으로 **5자리씩 증가**합니다.
    - depth가 깊어질수록 path 문자열 길이가 길어집니다.

#### 📝 예시:
```
부모 댓글: 00a0z
    -> 첫 번째 하위 댓글: 00a0z00000
        -> 두 번째 하위 댓글: 00a0z0000000000
```

- 즉, **하위 댓글의 path는 부모 댓글 path를 포함하며, 5자리씩 추가됩니다.**

### 📌2️⃣ descendantsTopPath vs childrenTopPath
#### 1️⃣ childrenTopPath (현재 댓글의 직접적인 하위)
```java
String childrenTopPath = findChildrenTopPath(descendantsTopPath);
```
```java
private String findChildrenTopPath(String descendantsTopPath) {
    return descendantsTopPath.substring(0, (getDepth() + 1) * DEPTH_CHUNK_SIZE);
}
```

- descendantsTopPath = "00a0z0000200000"
- childrenTopPath = "00a0z00002" (5자리씩 자름)

#### 2️⃣ descendantsTopPath (현재 댓글의 모든 자손 중 가장 큰 path)
- descendantsTopPath는 **현재 댓글이 포함된 전체 하위 트리에서 가장 마지막 댓글의 path**입니다.
- 즉, **00a0z00002**의 모든 자손 댓글 중 가장 마지막 댓글이 **00a0z0000200000** 입니다.

#### 📌 결론
- 00a0z00002의 **직접적인 하위 댓글**은 **00a0z0000200000** 입니다.
- 00a0z00003은 **00a0z00002와 같은 depth에서 생성될 새로운 sibling 댓글일 뿐, 00a0z00002의 하위 댓글이 아닙니다.**

## 🚀 정리.
- ✅ 00a0z00002의 하위 댓글은 00a0z0000200000이다.
- ✅ 댓글 구조에서 부모 댓글의 path + 5자리 문자열이 하위 댓글의 path가 된다.
- ✅ descendantsTopPath를 활용해 모든 자손 중 가장 마지막 댓글을 찾고, 이를 기반으로 새로운 댓글의 path를 결정한다.
- ✅ "00a0z00003"은 같은 depth의 sibling(형제 댓글)이지, 하위 댓글이 아니다.
