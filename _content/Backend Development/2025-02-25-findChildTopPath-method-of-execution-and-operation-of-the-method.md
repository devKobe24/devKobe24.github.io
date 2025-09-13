---
title: "📚[Backend Development] findChildTopPath 메서드의 실행 결과와 동작 방식."
tags:
    - Backend Ddevelopment
date: "2025-02-25"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] findChildTopPath 메서드의 실행 결과와 동작 방식."
## ✅1️⃣ 예시 코드.

```java
package kobe.board.comment.entity;

import jakarta.persistence.Embeddable;
import lombok.AccessLevel;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.ToString;

@Getter
@ToString
@Embeddable
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class CommentPath {
	private String path;

	private static final String CHARSET = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz";

	private static final int DEPTH_CHUNK_SIZE = 5;
	private static final int MAX_DEPTH = 5;

	// MIN_CHUNK = "00000", MAX_CHUNK = "zzzzz"
	private static final String MIN_CHUNK = String.valueOf(CHARSET.charAt(0)).repeat(DEPTH_CHUNK_SIZE);
	private static final String MAX_CHUNK = String.valueOf(CHARSET.charAt(CHARSET.length() - 1)).repeat(DEPTH_CHUNK_SIZE);

	public static CommentPath create(String path) {
		if (isDepthOverflowed(path)) {
			throw new IllegalStateException("depth overflowed");
		}

		CommentPath commentPath = new CommentPath();
		commentPath.path = path;
		return commentPath;
	}

	private static boolean isDepthOverflowed(String path) {
		return calculateDepth(path) > MAX_DEPTH;
	}

	private static int calculateDepth(String path) {
		// 25개의 문자열 / 5 = 5depth
		return path.length() / DEPTH_CHUNK_SIZE;
	}

	// CommentPath 클래스의 path의 depth를 구하는 매서드
	public int getDepth() {
		return calculateDepth(path);
	}

	// root인지 확인하는 매서드
	public boolean isRoot() {
		// 현재의 depth가 1인지 확인해주면 됨
		return calculateDepth(path) == 1;
	}

	// 현재 path의 parentPath를 반환해주는 매서드
	public String getParentPath() {
		// 끝 5자리만 잘라내면 됨
		return path.substring(0, path.length() - DEPTH_CHUNK_SIZE);
	}

	// 현재 path의 하위 댓글의 path을 만드는 매서드
	public CommentPath createChildCommentPath(String descendantsTopPath) {
		if (descendantsTopPath == null) {
			return CommentPath.create(path + MIN_CHUNK);
		}
		String childrenTopPath = findChildrenTopPath(descendantsTopPath);
		return CommentPath.create(increase(childrenTopPath));
	}
	// 00a0z 00002 00000 <- descendantsTopPath
	private String findChildrenTopPath(String descendantsTopPath) {
		return descendantsTopPath.substring(0, (getDepth() + 1) * DEPTH_CHUNK_SIZE);
	}
}
```

## ✅2️⃣ findChildrenTopPath(String descendantsTopPath) 실행 결과.
- descendantsTopPath에 "00a0z0000200000" 값이 들어간다고 가정한다.
    - findChildrenTopPath 메서드는 **현재 객체의 depth**를 기반으로 descendantsTopPath의 특정 길이만큼 잘라낸 값을 반환합니다.
    - 현재 path의 depth는 getDepth() 메서드를 통해 계산됩니다.
    - getDepth()는 현재 path의 길이를 DEPTH_CHUNK_SIZE(5)로 나누어 구합니다.

### 📌 예제 실행.
- 예를 들어, CommentPath 객체가 path = "00a0z"라면:
    - getDepth() = 1(문자열 길이 5/5)
    - (getDepth() + 1) * DEPTH_CHUNK_SIZE = (1 + 1) * 5 = 10
    - descendantsTopPath.substring(0, 10)

### 📌 결과값:
```
"00a0z00002"
````

## ✅3️⃣ findChildrenTopPath 메서드의 동작 방식
```java
private String findChildrenTopPath(String descendantsTopPath) {
    return descendantsTopPath.substring(0, (getDepth() + 1) * DEPTH_CHUNK_SIZE);
}
```

### 📌 단계별 동작.
#### 📌1️⃣ 현재 객체의 depth를 구함.
- getDepth()를 호출하여 현재 path가 몇 단계인지 계산.
- getDepth()는 path.length() / DEPTH_CHUNK_SIZE로 계산됨.

#### 📌2️⃣ (getDepth() + 1) * DEPTH_CHUNK_SIZE 값 계산.
- 현재 depth에서 **하위 댓글의 depth**를 포함한 길이를 계산.
- 즉, 다음 depth까지 포함한 descendantsTopPath의 일부만 가져오도록 설정,

#### 📌3️⃣ descendantsTopPath의 일부를 잘라 반환.
- descendantsTopPath.substring(0, (getDepth() + 1) * DEPTH_CHUNK_SIZE);
- descendantsTopPath에서 앞 부분을 가져와 **childrenTopPath를 생성.**

## 🚀 결론
- findChildrenTopPath("00a0z0000200000")의 실행 결과는 **"00a0z00002"**
- 이 메서드는 descendantsTopPath에서 현재 depth 기준으로 한 단계만 더 포함한 경로를 잘라 반환.
- 결국 **현재 객체의 하위 댓글들이 공통적으로 가지는 prefix를 찾아주는 역할을 한다.**
