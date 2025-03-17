---
title: "📚[Backend Development] 페이징 방식."
tags:
    - Backend Ddevelopment
date: "2025-03-17"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 페이징 방식."
## 📝 Intro.
- 대규모 데이터에서 게시글 목록을 조회할 때 **효율적인 페이징(Pagination) 처리**는 성능 최적화의 핵심입니다.
- 데이터가 많아질수록 OFFSET 방식의 성능 저하가 발생하므로, **Keyset Pagination(Seek 방식)** 등의 최적화 기법을 적용하는 것이 중요합니다.

## ✅1️⃣ 페이징 방식 비교.
- 대규모 데이터를 조회할 때 사용할 수 있는 대표적인 페이징 방법은 다음과 같습니다.

| 방식|장점|단점|적용 예제|
| -------- | -------- | --- | -------- |
|OFFSET + LIMIT|간단한 구현, SQL 표준 지원|OFFSET이 커질수록 성능 저하|블로그, 게시판|
|Keyset Pagination (Seek 방식)|성능 최적화, 인덱스 효율적 사용|특정 컬럼(정렬 기준)이 필요|뉴스 피드, 타임라인|
|Cursor 기반 페이징|유저 맞춤형 데이터 최적화|구현이 복잡|페이스북, 인스타그램|
|Redis 캐싱 활용|조회 속도 향상|데이터 동기화 문제 발생 가능|인기 게시글 목록|

## ✅2️⃣ 기본적인 OFFSET + LIMIT 방식
### 📌 개념
- OFFSET을 사용하여 특정 위치부터 LIMIT 개수만큼 데이터를 조회하는 방식
- 일반적인 게시판에서 많이 사용하지만, 대규모 데이터에서는 OFFSET 값이 커질수록 성능이 저하됨

### 📝 SQL 예제
```sql
SELECT * FROM posts ORDER BY created_at DESC LIMIT 10 OFFSET 10000;
```

- OFFSET 10000은 **앞의 10,000개 행을 스캔한 후 버리고,** 그 다음 10개만 가져옴.
- 데이터가 많아질수록 성능이 급격히 저하됨 → **"페이징이 깊어질수록 느려지는 문제 발생"**

### 📝 Java (Spring Data JPA) 예제
```java
Pageable pageable = PageRequest.of(page, 10, Sort.by(Sort.Direction.DESC, "createdAt"));
Page<Post> posts - postRepository.findAll(pageable);
```

### ❌ 문제점
- OFFSET이 크면 불필요한 데이터 검색으로 인해 성능 저하 발생
- 인덱스가 있어도 **데이터를 읽고 버리는 비용(I/O 비용)이 발생**

## ✅3️⃣ Keyset Pagination (Seek 방식)
### 📌 개념
- **OFFSET을 사용하지 않고,** 마지막 조회한 게시글의 ID(또는 created_at)을 기준으로 다음 데이터를 가져오는 방식
- **"마지막 조회된 데이터의 키를 기억하고, 그 이후 데이터를 조회"**

### 📝 SQL 예제
```sql
SELECT * FROM posts
WHERE created_at < '2024-03-11 12:00:00'
ORDER BY created_at DESC
LIMIT 10;
```

- created_at을 기준으로 **이전 페이지의 마지막 게시글 시간보다 작은 데이터만 조회**
- 데이터 양이 많아도 OFFSET이 없으므로 **빠르게 조회 가능**

### 📝 Java (Spring Data JPA) 예제
```java
public List<Post> getNextPage(LocalDateTime lastCreatedAt, int limit) {
    return postRepository.findByCreatedAtBeforeOrderByCreatedAtDesc(lastCreatedAt, PageRequest.of(0, limit));
}
```

- lastCreatedAt을 기준으로 다음 게시글을 조회
- LIMIT을 적용하여 페이징 처리

### ✅ 장점.
- **✅ OFFSET이 없으므로 속도가 빠름**
- **✅ 인덱스를 활용하여 빠른 검색 가능**
- **✅ 데이터가 많아도 성능이 일정하게 유지됨**

### ❌ 단점.
- **❌ created_at 또는 id가 반드시 있어야 함**
- **❌ ORDER BY를 위한 적절한 인덱스 설정 필요**

## ✅4️⃣ Cursor 기반 페이징.
### 📌 개념
- 페이스북, 인스타그램 같은 SNS에서 사용하는 방식
- **이전 페이지의 마지막 ID(또는 created_at)를 클라이언트에서 저장하고, 이를 이용해 다음 데이터를 조회**
- **Keyset Pagination과 유사하지만, API에서 cursor 값을 반환하고 이를 사용**

### 📝 SQL 예제.
```sql
SELECT * FROM posts
WHERE id > 1050
ORDER BY id ASC
LIMIT 10;
```

- id가 1050보다 큰 게시글을 가져옴
- ORDER BY id ASC를 사용하여 오름차순 정렬

### 📝 Java (Spring Data JPA) 예제
```sql
public List<Post> getNextPosts(Long lastPostId, int limit) {
    return postRepository.findByIdGreaterThanOrderByIdAsc(lastPostId, PageRequest.of(0, limit));
}
```

- 클라이언트에서 **이전 페이지의 마지막 id 값을 저장**하고 이를 이용하여 다음 데이터를 조회

### ✅ 장점
- **✅ Keyset Pagination과 마찬가지로 OFFSET 없이 성능 최적화**
- **✅ 페이스북, 인스타그램 같은 무한 스크롤(Scroll) UI에 적합**
- **✅ API 응답에서 cursor(마지막 id) 값을 포함하여 다음 요청에 사용 가능**

### ❌ 단점
- **❌ 클라이언트에서 cursor(마지막 id) 값을 저장해야 함**
- **❌ 특정 필드(id, created_at) 기준으로 정렬해야 하므로 복잡한 정렬이 어려움.**

## ✅5️⃣ Redis를 활용한 캐싱 페이징
### 📌 개념
- 인기 게시글, 조회수가 많은 데이터는 **DB에서 직접 조회하지 않고 Redis에 저장하여 빠르게 제공**
- 일정 시간마다(예: 5분) 인기 게시글 목록을 업데이트
- ZSET(Sorted Set)을 사용하여 정렬된 데이터를 빠르게 조회

### 📝 Java(Spring + Redis) 예제
```java
@Service
public class PostCacheService {
    private static final String CACHE_KEY = "latest_posts";

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    @Autowired
    private PostRepository postRepository;

    public List<Post> getLatestPosts() {
        List<Post> cachedPosts = (List<Post>) redisTemplate.opsForValue().get(CACHE_KEY);
        if (cachedPosts != null) {
            return cachedPosts;
        }

        // 캐시가 없으면 DB에서 조회 후 Redis에 저장
        List<Post> posts = postRepository.findTop10ByOrderByCreatedAtDesc();
        redisTemplate.opsForValue().set(CACHE_KEY, posts, Duration.ofSeconds(60)); // 60초 캐싱
        return posts;
    }
}
```

### ✅ 장점
- **✅ 인기 게시글을 빠르게 조회 가능**
- **✅ DB 부하를 줄이고, 응답 속도 향상**

### ❌ 단점
- **❌ 실시간 최신 데이터가 필요할 경우 캐시 동기화 문제가 발생**

## ✅6️⃣ 최적의 페이징 방식 선택

| 페이징 방식 | 성능 | 장점 |단점|사용 사례|
| ----------- | ---- | ---- | --- | ---- |
|OFFSET + LIMIT|❌ 느림|간단한 구현|데이터 많아지면 성능 저하|기본적인 페이지네이션|
|Keyset Pagination|✅ 빠름|인덱스 최적화, 성능 일정|특정 정렬 필드 필요|뉴스 피드, 블로그|
|Cursor 기반|✅ 빠름|SNS, 무한 스크롤 최적|클라이언트에서 cursor 저장 필요|인스타그램, 페이스북|
|Redis 캐싱|🚀 매우 빠름|인기 게시글 빠른 조회|실시간 데이터 반영 어려움|인기 게시글, 랭킹|

## ✅7️⃣ 결론
- **✅ 대규모 데이터에서 OFFSET 방식은 비효율적**
- **✅ Keyset Pagination(Seek 방식)이 성능 최적화에 유리**
- **✅ Cursor 방식은 SNS나 무한 스크롤에 적합**
- **✅ Redis를 활용하여 캐싱하면 조회 속도를 극대화할 수 있음**
