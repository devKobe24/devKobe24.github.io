---
title: "📚[Backend Development] increase메서드 실행과정 분석"
tags:
    - Backend Ddevelopment
date: "2025-03-07"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] increase메서드 실행과정 분석"
## 📌 CommentPath 클래스.
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

	// 00a0z0000200000 <- descendantsTopPath
	private String findChildrenTopPath(String descendantsTopPath) {
		return descendantsTopPath.substring(0, (getDepth() + 1) * DEPTH_CHUNK_SIZE);
	}

	private String increase(String path) {
		// path에서 가장 마지막 5자리를 자른 것
		String lastChunk = path.substring(path.length() - DEPTH_CHUNK_SIZE);

		if (isChunkOverflowed(lastChunk)) {
			throw new IllegalStateException("chunk overflowed");
		}

		// Character set의 길이
		int charsetLength = CHARSET.length();

		// lastChunk를 10진수로 먼저 변환하기 위한 값을 저장
		int value = 0;

		for (char character : lastChunk.toCharArray()) {
			value = value * charsetLength + CHARSET.indexOf(character);
			System.out.println("value ====> " + value);
			System.out.println("CHARSET.indexOf ====> " +  CHARSET.indexOf(character));
		}

		value = value + 1;

		String result = "";

		for (int i = 0; i < DEPTH_CHUNK_SIZE; i++) {
			result = CHARSET.charAt(value % charsetLength) + result;
			value /= charsetLength;
		}

		return path.substring(0, path.length() - DEPTH_CHUNK_SIZE) + result;
	}

	private boolean isChunkOverflowed(String lastChunk) {
		return MAX_CHUNK.equals(lastChunk);
	}
}
```

## ✅1️⃣ 코드 실행 흐름 확인
```java
for (int i = 0; i < DEPTH_CHUNK_SIZE; i++) {
    result = CHARSET.charAt(value % charsetLength) + result;
    value /= charsetLength;
}
```

### 📌 코드 핵심 역할
- value를 CHARSET(62진수) 기반으로 **DEPTH_CHUNK_SIZE(=5) 길이의 문자열로 변환**하는 과정
- 각 반복에서 value의 마지막 자리를 CHARSET에서 찾아 문자열(result)에 추가하고, value를 62로 나눠서 다음 문자를 구함.

## ✅2️⃣ descendantsTopPath = "00000" 일 때 increas("00000")의 실행 과정
### 📌 increase("00000") 실행 과정
#### 1. lastChunk 추출
```java
String lastChunk = path.substring(path.length() - DEPTH_CHUNK_SIZE)
```

- "00000" ➞ lastChunk = "00000"

#### 2. lastChunk를 10진수(value)로 변환
```java
int value = 0;
for (char character : lastChunk.toCharArray()) {
    value = value * charsetLength + CHARSET.indexOf(cha)
}
```

- CHARSET.indexOf('0') = 0
- value 값 계산:

```java
value = (0 * 62) + 0 = 0
value = (0 * 62) + 0 = 0
value = (0 * 62) + 0 = 0
value = (0 * 62) + 0 = 0
value = (0 * 62) + 0 = 0
```

- 최종적으로 value = 0

#### 3. value + 1 증가.

```java
value = value + 1;
```

- value = 0 + 1 = 1

#### 4. value를 다신 62진수 문자열로 변환
```java
for (int i = 0; i < DEPTH_CHUNK_SIZE; i++) {
    result = CHARSET.charAt(value % charserLength) + result;
    value /= charsetLength;
}
```

- **반복 과정(DEPTH_CHUNK_SIZE = 5)**

```
i = 0: value % 62 = 1 → CHARSET[1] = '1' → result = "1"
i = 1: value /= 62 = 0 → CHARSET[0] = '0' → result = "01"
i = 2: value = 0 → CHARSET[0] = '0' → result = "001"
i = 3: value = 0 → CHARSET[0] = '0' → result = "0001"
i = 4: value = 0 → CHARSET[0] = '0' → result = "00001"
```

- 최종 result = "00001"
