---
title: "📚[Backend Development] increase메서드 내부 for문 실행 과정 상세 분석"
tags:
    - Backend Ddevelopment
date: "2025-03-07"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] increase메서드 내부 for문 실행 과정 상세 분석"
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

## 📌 for 문 실행 과정 상세 분석
- 해당 for ansdms **주어진 value(10진수)를 CHARSET(62진수)로 변환하여 문자열(result)을 생성하는 과정**입니다.

### 1️⃣ for 문 코드
```java
for (int i = 0; i < DEPTH_CHUNK_SIZE; i++) {
    result = CHARSET.charAt(value % charsetLength) + result;
    value /= charsetLength;
}
```

#### ✅ 주요 개념
- value % charsetLength ➞ **62진수에서 가장 낮은 자리수(오른쪽 끝)를 구함**
- CHARSET.charAt(value % charsetLenht) ➞ **CHARSET에서 해당 인덱스의 문자(0~9, A~Z, a~z)를 가져옴**
- value /= charsetLength ➞ **다음 자리수를 계산하기 위해 value를 62로 나눔**
- result = CHARSET.charAt(value % charsetLenght) + resultl ➞ **문자열의 앞에 추가하여 변환 결과를 만든다**

### 2️⃣ for 문 실행 과정 단계별 분석
#### 📌 예제 1: value = 1, charsetLenght = 62, DEPTH_CHUNK_SIZE = 5
#### ✅ 초기 값
```java
value = 1;
resutl = "";
```

#### ✅ 반복 과정

| 반복 | value | value % 62 | CHARSET.charAt(value % 62) | result  | value /= 62 |
| ---- | ----- | ---------- | -------------------------- | ------- | ----------- |
| i=0  | 1     | 1          | '1'                        | "1"     | 0           |
| i=1  | 0     | 0          | '0'                        | "01"    | 0           |
| i=2  | 0     | 0          | '0'                        | "001"   | 0           |
| i=3  | 0     | 0          | '0'                        | "0001"  | 0           |
| i=4  | 0     | 0          | '0'                        | "00001" | 0           |

#### ✅ 최종 결과

```java
result = "00001";
```

#### 📌 예제 2: value = 62
#### ✅ 초기 값
```java
value = 62;
resutl = "";
```

#### ✅ 반복 과정

| 반복 | value | value % 62 | CHARSET.charAt(value % 62) | result  | value /= 62 |
| ---- | ----- | ---------- | -------------------------- | ------- | ----------- |
| i=0  | 62    | 0          | '0'                        | "0"     | 1           |
| i=1  | 1     | 1          | '1'                        | "10"    | 0           |
| i=2  | 0     | 0          | '0'                        | "010"   | 0           |
| i=3  | 0     | 0          | '0'                        | "0010"  | 0           |
| i=4  | 0     | 0          | '0'                        | "00010" | 0           |

#### ✅ 최종 결과

```java
result = "00010";
```

#### 📌 예제 3: value = 3843
#### ✅ 초기 값
```java
value = 3843;
resutl = "";
```

#### ✅ 반복 과정

| 반복 | value | value % 62 | CHARSET.charAt(value % 62) | result  | value /= 62 |
| ---- | ----- | ---------- | -------------------------- | ------- | ----------- |
| i=0  | 3843  | 61         | 'z'                        | "z"     | 61          |
| i=1  | 61    | 61         | 'z'                        | "zz"    | 0           |
| i=2  | 0     | 0          | '0'                        | "0zz"   | 0           |
| i=3  | 0     | 0          | '0'                        | "00zz"  | 0           |
| i=4  | 0     | 0          | '0'                        | "000zz" | 0           |

#### ✅ 최종 결과

```java
result = "000zz";
```

## 📌 핵심 정리
### 📝 for 문이 하는 일
- 1. value(10진수)를 **62진수 문자열로 변환한다.**
- 2. CHARSET.charAt(value % 62)를 사용하여 **가장 낮은 자리수부터 변환한다.**
- 3.변환된 문자를 result 앞에 추가(+ result)
- 4. value /= 62 하여 다음 자리수를 계산.
- 5. **최종적으로 DEPTH_CHUNK_SIZE(5)만큼 반복하여 5자리 문자열을 만든다.**

## 📌 결론
- value의 작은 자리수부터 변환하여 문자열을 구성한다.
- 62진법 변환원리를 사용하여 댓글의 path를 증가시키는 데 활용한다.
