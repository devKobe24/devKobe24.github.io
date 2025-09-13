---
title: "📚[Backend Development] CommentPath 클래스 분석 및 설명"
tags:
    - Backend Ddevelopment
date: "2025-03-05"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] CommentPath 클래스 분석 및 설명"
## 📌 CommentPath 클래스 분석 및 설명.
- CommentPath 클래스는 계층형 댓글 시스템에서 각 댓글의 경로(path)를 관리하는 역할을 합니다.
- **각 댓글은 path라는 문자열로 표현되며,** path는 일정한 규칙을 따라 댓글의 부모-자식 관계를 나타냅니다.
- 이 클래스는 댓글의 깊이(depth), 부모 댓글(parentPath), 새로운 자식 댓글(createChildCommentPath) 등을 관리하는 기능을 제공합니다.

## ✅1️⃣ 클래스 전반적인 개요.
### 📌 핵심 개념
#### 1️⃣ path 필드
- path는 댓글의 계층 구조를 표현하는 문자열입니다.
- 각 댓글은 5자리씩(DEPHT_CHUNK_SIZE = 5)의 문자열을 가지며, 댓글이 깊어질수록 path가 길어집니다.

#### 📝 예시:
```
루트 댓글: "00000"
첫 번째 자식 댓글: "0000000000"
두 번째 자식 댓글: "000000000000000"
```

- 이를 통해 **댓글이 어느 계층에 속하는지, 부모가 누구인지, 자식이 어떻게 배치될지를 결정할 수 있습니다.**

#### 2️⃣ CHARSET (문자 집합)
- 0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz (총 62개 문자)
- path의 각 5자리(chunk)는 이 문자 집합을 사용해 표현됩니다.
- 새로운 댓글이 추가될 때, path는 문자 집합 내에서 증가(increase)하는 방식으로 생성됩니다.

## ✅2️⃣ 주요 필드
```java
private static final String CHARSET = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz";
private static final int DEPTH_CHUNK_SIZE = 5; // 각 depth가 5자리로 표현됨
private static final int MAX_DEPTH = 5; // 최대 depth 5까지 허용

private static final String MIN_CHUNK = String.valueOf(CHARSET.charAt(0)).repeat(DEPTH_CHUNK_SIZE);
private static final String MAX_CHUNK = String.valueOf(CHARSET.charAt(CHARSET.length() - 1)).repeat(DEPTH_CHUNK_SIZE);
```

- MIN_CHUNK = "00000" : 최소 chunk 값
- MAX_CHUNK = "zzzzz" : 최대 chunk 값 (즉, 더 이상 증가 불가능한 값)
- 댓글의 path는 MIN_CHUNK부터 시작해 점진적으로 증가하는 방식입니다.

## ✅3️⃣ 주요 메서드 분석
- 각 메서드의 **역할, 사용 시기, 동작 방식을 설명하겠습니다.**

### 1️⃣ create(String path)
```java
public static CommentPath create(String path) {
    if (isDepthOverflowed(path)) {
        throw new IllegalStateException("depth overflowed");
    }
    CommentPath commentPath = new CommentPath();
    commentPath.path = path;
    return commentPath;
}
```

- ✅ 역할:
    - 주어진 path로 CommentPath 객체를 생성한다.
    - path의 깊이가 MAX_DEPTH를 초과하면 예외 발생.
- ✅ 사용 시기:
    - 댓글을 DB에 저장할 때, CommentPath를 생성할 때 사용.
- ✅ 동작 방식:
    - 1. isDepthOverflowed(path)를 호출하여 최대 깊이를 초과하는지 확인한다.
    - 2. 문제가 없으면 CommentPath 객체를 생성하여 반환한다.

### 2️⃣ calculateDepth(String path)
```java
private static int calculateDepth(String path) {
    return path.length() / DEPTH_CHUNK_SIZE;
}
```

- ✅ 역할:
    - 현재 path의 깊이를 계산한다.
    - path.length()를 DEPTH_CHUNK_SIZE로 나누면 깊이가 된다.
- ✅ 사용 시기:
    - 댓글이 몇 번째 깊이인지 확인 할 때.
    - isRoot(), isDepthOverflowed() 등의 메서드에서 사용.
- ✅ 동작 방식:
    - path.length()를 5로 나눈다.
    - 예: "0000000000" (10글자) ➞ calculateDepth("0000000000") ➞ 10 / 5 = 2

### 3️⃣ getDepth()
```java
public int getDepth() {
    return calculateDepth(path);
}
```

- ✅ 역할:
    - 현재 댓글의 깊이를 반환한다.
- ✅ 사용 시기:
    - 댓글의 깊이를 확인할 때.
- ✅ 동작 방식:
    - calculateDepth(path)를 호출하여 깊이를 구한다.

### 4️⃣ isRoot()
```java
public boolean isRoot() {
    return calculateDepth(path) == 1;
}
```

- ✅ 역할:
    - 현재 댓글이 루트 댓글인지 확인한다.
- ✅ 사용 시기:
    - 부모 댓글이 없는 루트 댓글인지 확인할 때.
- ✅ 동작 방식:
    - 깊이가 1이면 루트 댓글로 판단.

### 5️⃣ getParentPath()
```java
public String getParentPath() {
    return path.substring(0, path.length() - DEPTH_CHUNK_SIZE);
}
```

- ✅ 역할:
    - 부모 댓글의 path를 반환한다.
- ✅ 사용 시기:
    - 댓글의 부모를 찾을 때.
- ✅ 동작 방식:
    - 현재 path에서 마지막 5자리를 잘라내어 반환한다.

### 6️⃣ createChildCommentPath(String descendantsTopPath)
```java
public CommentPath createChildCommentPath(String descendantsTopPath) {
    if (descendantsTopPath == null) {
        return CommentPath.creat(path + MIN_CHUNK);
    }
    String childrenTopPath = findChildrenTopPath(descendantsTopPath);
    return CommentPath.create(increase(childrenTopPath));
}
```

- ✅ 역할:
    - 현재 댓글의 자식 댓글의 path를 생성한다.
- ✅ 사용 시기:
    - 새로운 대댓글을 추가할 때.
- ✅ 동작 방식:
    - 1. descendantsTopPath가 null이면 MIN_CHUNK를 붙여서 자식 댓글을 생성.
    - 2. 자식 댓글의 path를 가져온 후 increase()를 호출하여 증가.

### 7️⃣ findChildrenTopPath(String descendantsTopPath)
```java
private String findChildrenTopPath(String descendantsTopPath) {
    return descendantsTopPath.substring(0, (getDepth() + 1) * DEPTH_CHUNK_SIZE);
}
```

- ✅ 역할:
    - 자손 댓글 중 가장 상위 댓글의 path를 반환한다.
- ✅ 사용 시기:
    - 새로운 댓글을 추가할 때, **어떤 댓글이 현재 댓글의 가장 최근 자식 댓글인지 찾을 때.**
- ✅ 동작 방식:
    - 현재 깊이보다 한 단계 더 깊은 path 부분을 잘라서 반환.

### 8️⃣ increase(String path)
```java
private String increase(String path) {
    String lastChunk = path.substring(path.length() - DEPTH_CHUNK_SIZE);

    if (isChunkOverflowed(lastChunk)) {
        throw new IllegalStateException("chunk overflowed");
    }

    int charsetLength = CHARSET.length();
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
```

- ✅ 역할:
    - 댓글 path를 증가시켜 새로운 자식 댓글 생성
- ✅ 사용 시기:
    - 새로운 대댓글을 추가할 때
### 📝 동작 방식:
#### 📌 입력(path)
- path는 댓글의 계층 구조를 나타내는 문자열.
- 예를 들어 "0000000000"(2번째 깊이의 댓글)이라는 주어졌다고 가정.

#### 📌 1단계: 마지막 5자리(Chunk) 추출

```java
String lastChunk = path.substring(path.length() - DEPTH_CHUNK_SIZE);
```

- path에서 마지막 DEPTH_CHUNK_SIZE(=5)만큼의 문자열을 잘라낸다.
- 예제:

```java
path = "0000000000";
lastChunk = path.substring(5); // "00000"
```

#### 📌 2단계: lastChunk 값이 최대값인지 확인
```java
if (isChunkOverflowed(lastChunk)) {
    throw new IllegalStateException("chunk overflowed");
}
```

- isChunkOverflowed(lastChunk) 메서드를 호출하여 lastChunk가 "zzzzz"(최대값)인지 확인.
- 만약 "zzzzz"라면 더 이상 증가할 수 없으므로 예외를 던진다.

#### 📌 3단계: lastChunk 값을 10진수로 변환
```java
int charsetLength = CHARSET.length();
int value = 0;

for (char character : lastChunk.toCharArray()) {
    value = value * charsetLength + CHARSET.indexOf(character);
}
```

- CHARSET은 0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz (총 62개 문자).
- lastChunk(현재 5자리 문자열)를 **62진수에서 10진수로 변환**한다.

#### 📝 예제 1: "00000" 변환
- CHARSET.index('0') = 0
- 변환 과정:
```
value = (0 * 62) + 0 = 0
value = (0 * 62) + 0 = 0
value = (0 * 62) + 0 = 0
value = (0 * 62) + 0 = 0
value = (0 * 62) + 0 = 0
```

#### 📝 예제 2: "0000z" 변환
- 'z'의 인덱스는 61
```
value = (0 * 62) + 0 = 0
value = (0 * 62) + 0 = 0
value = (0 * 62) + 0 = 0
value = (0 * 62) + 0 = 0
value = (0 * 62) + 61 = 61
````

#### 📌 4단계: 값 증가 (+ 1 연산)
```java
value = value + 1;
```

- 10진수의 값이 1증가한다.

#### 📝 예제 1: "00000" ➞ "00001"
```
value = 0 + 1 = 1
```

#### 📝 예제 2: "0000z" ➞ "00010"
```
value = 61 + 1 = 62
````

#### 📌 5단계: 다시 62진수 문자열로 변환
```java
String result = "";

for (int i = 0; i < DEPTH_CHUNK_SIZE; i++) {
    result = CHARSET.charAt(value % charsetLength) + result;
    value /= charsetLength;
}
```

- 증가된 10진수 값을 다시 62진수 문자열로 변환한다.

#### 📝 예제 1: value = 1
- value % 62 = 1 ➞ '1'
- value /= 62 = -
- 결과: "00001"

#### 📝 예제 2: value = 62
- value % 62 = 0 ➞ '0'
- value /= 62 = 1
- value % 62 = 1 ➞ '1'
- 최종 결과: "00010"

#### 📌 6단계: 기존 path에서 마지막 chunk를 새로운 값으로 교체
```java
return path.substring(0, path.length() - DEPTH_CHUNK_SIZE) + result;
```

- 원래 path의 마지막 5자리를 새로운 값으로 변경한 문자열을 반환.

#### 📝 예제
```java
path = "0000000000";
result = "00001";
최종 반환값: "0000000001"
```

### 📌 전체 동작 예제
#### 📌 예제 1
```java
increase("0000000000"); // 기존 댓글의 path
```

- 1. "0000000000"에서 마지막 5자리 **"00000"을 추출.**
- 2. "00000" → 10진수 변환 ➞ 0
- 3. 0 + 1 = 1
- 4. 1 ➞ 62진수 변환 ➞ "00001"
- 5. "0000000000" ➞ "0000000001"로 변환
- ✅ 최종 결과 : "0000000001"

#### 📌 예제 2
```java
increase("000000000z"); // 기존 댓글의 path
```

- 1. "000000000z"에서 마지막 5자리 **"0000z"을 추출.**
- 2. "0000z" → 10진수 변환 ➞ 61
- 3. 61 + 1 = 62
- 4. 62 ➞ 62진수 변환 ➞ "00010"
- 5. "000000000z" ➞ "0000000010"로 변환
- ✅ 최종 결과 : "0000000010"
