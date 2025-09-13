---
title: "📚[Backend Development] findChildrenTopPath(String descendantsTopPath) 메서드의 사용 방법, 사용 시기, 동작 방법"
tags:
    - Backend Ddevelopment
date: "2025-03-03"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] findChildrenTopPath(String descendantsTopPath) 메서드의 사용 방법, 사용 시기, 동작 방법"
## ✅1️⃣ findChildrenTopPath() 메서드의 역할
- findChildrenTopPath() 메서드는 **주어진 descendantsTopPath(자손 댓글의 경로)에서 현재 댓글의 직계 자식 댓글의 path를 추출하는 메서드**입니다.
- 즉, descendantsTopPath가 현재 댓글의 여러 자식 댓글 중 하나일 때, **현재 댓글의 자식들 중 최상위 댓글의 path를 가져오는 역할**을 합니다.

## ✅2️⃣ findChildrenTopPath() 메서드의 동작 방식

```java
private String findChildrenTopPath(String descendantsTopPath) {
    return descendantsTopPath.substring(0, (getDepth() + 1) * DEPTH_CHUNK_SIZE);
}
```

- 이 메서드는 다음과 같은 방식으로 동작합니다.
    - 1. **descendantsTopPath.substring(0, (getDepth() + 1) * DEPTH_CHUNK_SIZE)**
        - descendantsTopPath(자손 댓글의 경로)에서 **현재 댓글의 바로 다음 깊이(자식 댓글)의 path 부분만 가져옵니다.**
        - getDepth()는 현재 댓글의 깊이를 계산하는 메서드로, path.length() / DEPTH_CHUNK_SIZE를 반환합니다.
        - (getDepth() + 1) * DEPTH_CHUNK_SIZE를 통해 **현재 댓글보다 한 단계 더 깊은 위치까지의 문자열을 추출**합니다.

## ✅3️⃣ findChildrenTopPath() 메서드의 사용 예시
### 📝 예제 1: 기본적인 부모-자식 관계에서 사용.
```java
CommentPath parent = CommentPath.create("00000"); // 부모 댓글
CommentPath child = CommentPath.creat("0000000000"); // 자식 댓글
CommentPath grandChild = CommentPath.create("000000000000000"); // 손자 댓글

String childrenTopPath = parent.findChildrenTopPath(grandChild.getPath());
System.out.println(childrenTopPath); // 출력: "0000000000"
```

#### ▶️ 실행 과정.
- 1. parent.getPath() ➞ "00000" (부모 댓글)
- 2. grandChild.getPath() ➞ "000000000000000" (손자 댓글)
- 3. findChildrenTopPath(grandChild.getPath()) 실행:
    - parent.getDepth() = 1
    - (getDepth() + 1) * DEPTH_CHUNK_SIZE = (1 + 1) * 5 = 10
    - "000000000000000".substring(0, 10) ➞ "0000000000" (부모의 직계 자식 댓글 경로 반환)
        - 즉, **손자 댓글 path를 입력받아 현재 댓글의 첫 번째 자식의 path를 반환합니다.**

## ✅4️⃣ findChildrenTopPath() 메서드 사용 시기
### 1️⃣ 새로운 대댓글을 생성할 때(createChildCommentPath()에서 사용)
```java
public CommentPath createChildCommentPath(String descentsTopPath) {
    if (descentsTopPath == null) {
        return CommentPath.creat(path + MIN_CHUNK);
    }
    String childrenTopPath = findChildrenTopPath(descendantsTopPath);
    return CommentPath.creat(increase(childrenTopPath));
}
```

- descendantsTopPath가 존재하면 findChildrenTopPath()를 사용하여 **현재 댓글의 첫 번째 자식 댓글의 path를 가져옴.**
- 이후 increase()를 호출하여 새 댓글의 path를 하나 증가시켜 **새로운 자식 댓글을 생성함**

### 2️⃣ 계층형 댓글 조회 시 부모-자식 관계를 파악할 떄
- 부모 댓글과 자식 댓글의 관계를 파악하여 **트리 구조를 구성할 때** 사용될 수 있음.
- 예를 들어, 특정 댓글이 descentdantsTopPath(자손 댓글의 path)를 가질 때, 부모 댓글의 직계 자식이 무엇인지 판단하는 데 활용될 수 있음.

## ✅5️⃣ findChildrenTopPath() 메서드 실행 예제
```java
CommentPath parent = CommentPath.create("00000"); // 부모 댓글
CommentPath child = CommentPath.create("0000000000"); // 자식 댓글
CommentPath grandChild = CommentPath.create("000000000000000"); // 손자 댓글
    
String childPath = parent.findChildrenTopPath(grandChild.getPath());
System.out.println("부모 댓글의 첫 번째 자식 path: " + childPath);
```

### 📝 출력

```
부모 댓글의 첫 번째 자식 path: 0000000000
```

## ✅6️⃣ 정리.
- **역할 :** descendantsTopPath(자손 댓글의 경로)에서 **현재 댓글의 직계 자식 댓글의 path를 추출.**
- **동작 방식 :** descendantsTopPath.substring(0, (getDepth() + 1) * DEPTH_CHUNK_SIZE)을 사용하여 특정 위치까지의 문자열을 반환.
- **사용 시기**
    - **새로운 대댓글 생성시 (createChildCommentPath() 내부에서 사용됨)**
    - **계층형 댓글을 조회할 때 부모-자식 관계 파악**
- **주의할 점**
    - descendantsTopPath가 null이면 substring()에서 NullPointerException이 발생할 수 있음.
    - 댓글이 너무 깊어지면 MAX_DEPTH를 초과할 수 있음.
        - 즉, findChildrenTopPath()는 **현재 댓글의 자식 중 첫 번째 댓글의 path를 가져오는 역할을 하며, 새로운 대댓글을 생성할 때 매우 중요한 역할을 합니다.🚀**
