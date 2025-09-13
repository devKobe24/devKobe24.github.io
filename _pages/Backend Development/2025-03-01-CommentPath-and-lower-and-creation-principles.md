---
title: "📚[Backend Development] CommentPath 및 하위 댓글 생성 원리"
tags:
    - Backend Ddevelopment
date: "2025-03-01"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] CommentPath 및 하위 댓글 생성 원리"
## 📝 Intro
- 댓글 시스템에서 각 댓글의 경로(path)를 관리하는 방식은 계층적 구조를 유지하면서도 빠르게 검색할 수 있도록 설계되어야 합니다.
- 본 글에서는 `CommentPath` 클래스를 분석하고, 하위 댓글이 생성되는 과정과 관련된 로직을 설명합니다.

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

## ✅1️⃣ CommentPath 클래스 Intro
- CommentPath 클래스는 댓글의 고유한 경로(path)를 관리하는 역할을 합니다.
    - 경로는 문자열로 표현되며, 각 댓글의 depth를 유지하면서 하위 댓글을 쉽게 찾을 수 있도록 구성됩니다.

### 📌1️⃣ 주요 상수 및 변수.
- **CHARSET :** 0-9, A-Z, a-z로 구성된 총 62개의 문자 셋
- **DEPTH_CHUNK_SIZE = 5 :** 댓글 깊이를 나타내는 단위 크기
- **MAX_DEPTH = 5 :** 댓글의 최대 깊이
- **MIN_CHUNK = "00000" :** 경로에서 사용될 최소 단위 문자열
- **MAX_CHUNK = "zzzzz" :** 경로에서 사용될 최대 단위 문자열

## ✅2️⃣ CommentPath.create(String path) 메서드 동작 과정
- 이 메서드는 새로운 **CommentPath** 객체를 생성하는 **정적 팩토리 메서드**입니다.

### 📌1️⃣ 동작 과정.
- **1. isDepthOverflowed(path)를** 호출하여 **path**의 깊이가 **MAX_DEPTH = 5**를 초과하는지 확인합니다.
    - **calculateDepth(path) > MAX_DEPTH**이면 **IllegalStateException**을 던집니다.
- **2. CommentPath** 객체를 생성합니다.
- **3. 객체의 path** 필드를 **path**로 설정합니다.
- **4. 새로 생성된 CommentPath** 객체를 반환합니다.

## ✅3️⃣ CommentPath.createChildCommentPath(String descendantsTopPath) 메서드 동작 과정
- 이 메서드는 주어진 **descendantsTopPath**를 기반으로 새로운 **하위 댓글의 path**를 생성하는 역할을 합니다.

### 📌1️⃣ 동작 과정.
- **1. descedantsTopPath**가 **null**이면 **path + MIN_CHUNK("00000")를** 추가하여 **CommentPath를** 생성하고 반환합니다.
- **2. findChildrenTopPath(descendantsTopPath)를** 호출하여 **childrenTopPath를** 찾습니다.
    - **descendantsTopPath**에서 **depth + 1** 길이만큼 문자열을 자릅니다.
- **3. increase(childrenTopPath)를** 호출하여 **이전 하위 댓글 path  값에서 1을 증가**시킵니다.
- **4.** 증가된 **childrenTopPath를** 기반으로 새로운 **CommentPath** 객체를 생성하고 반환합니다.

## ✅4️⃣ "00000"이 생성되려면 어떤 과정을 거쳐야 할까?
- **1. createChildCommentPath(null)이** 호출됩니다.
- **2. descendantsTopPath**가 **null**이므로 **path + MIN_CHUNK**가 **CommentPath.creat()에** 전달됩니다.
- **3. CommentPath.create()** 내부에서 **isDepthOverflowed** 검사를 통과하면 새로운 **CommentPath** 객체가 생성 됩니다.
- **4. path** 값은 **부모 path + "00000"이** 됩니다.

## ✅5️⃣ "0000000000"이 생성되려면 어떤 과정을 거쳐야 할까?
- **1. createdChildCommentPath(null)이 두 번 호출**되어야 합니다.
- **2. 첫 번째 호출:**
    - **descendantsTopPath = null**
    - **path + "00000"을 CommentPath.create()로 전달하여 "00000"이 생성됨.**
- **3. 두 번째 호출:**
    - **descendantsTopPath = "00000"**
    - **"00000"의 하위 댓글을 만들 때 path + "00000"을 추가하여 "0000000000"을 생성.**
- **즉, 2단계 하위 댓글 생성 과정이 필요합니다.**

## ✅6️⃣ 특정 descendantsTopPath가 주어졌을 때 childrenTopPath가 어떻게 변할까요?
### 📌 예시: descendantsTopPath = "00000", childrenTopPath = "00001"
- **1. createChildCommentPath("00000")가 호출됨.**
- **2. findChildrenTopPath("00000") 호출:**
    - **descendantsTopPath = "00000"**
    - **getDepth() + 1 = 2 → depth 2** 까지의 5자리(00000)를 가져옴.
    - **childrenTopPath = "00000"**
- **3. increase("00000") 실행:**
    - **62진수 연산으로 "00000" → "00001"로 변환.**
- **4. "00001"이 path로 설정된 CommentPath 객체가 생성됨.**
- **즉, increase("00000") 연산을 통해 "00001"이 생성됩니다.**

## ✅7️⃣ 특정 상황에서 새로운 댓글의 path를 생성하는 과정
### 📌 예시: descendantsTopPath = "0000z", 하위 댓글이 "abcdz" > "zzzzz" > "zzzzz" 일 때, "abcdz"의 sibling 댓글 생성

- **1. 현재 descendantsTopPath는** "0000z"이므로 이 댓글이 속한 하위 댓글들이 존재함.
- **2. createChildCommentPath("zzzzz") 실행 →** 가장 큰 하위 **path**를 기준으로 새로운 path를 생성해야 함.
- **3. findChildCommentTopPaht("zzzzz") 실행:**
    - **"zzzzz"의 depth + 1 길이까지 자름 → "zzzzz"**
- **4. increase("zzzzz") 실행:**
    - **"zzzzz"에서 증가된 값 "aaaaa"가 나옴**
    - 하지만 **"abcdz"** 와 같은 depth의 새로운 댓글을 만들려면 **"abcdz"의** sibling 댓글이 되어야 함.
- **5. increase("abcdz") 실행:**
    - **"abcdz"에서 증가된 값 "abce0"가 생성됨.**
- 📌 **결론 : 최종적으로 생성될 "abce0"의 "path"는 부모 "path" + "abce0" 즉, path = "0000zabce0"이 됩니다.** 

## ✅8️⃣ 결론
- **CommentPath**를 이용하면 계층적 댓글 시스템을 효과적으로 구현할 수 있습니다.
- 하위 댓글의 **path**를 **62진수 문자열 증가 방식**으로 관리하여, **빠른 정렬 및 검색**이 가능합니다.
- **increase** 연산을 통해 하위 댓글을 동적으로 생성하며, 경로 오버플로우를 방지할 수 있습니다.
- 이러한 방식은 **트리 구조를 효율적으로 저장하고 조회하는 방법으로, 대규모 댓글 시스템에서도 유용하게 적용될 수 있습니다.**
