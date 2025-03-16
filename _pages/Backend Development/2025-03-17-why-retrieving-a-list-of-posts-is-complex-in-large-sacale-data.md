---
title: "📚[Backend Development] 대규모 데이터에서 게시글 목록 조회가 복잡한 이유"
tags:
    - Backend Ddevelopment
date: "2025-03-17"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 대규모 데이터에서 게시글 목록 조회가 복잡한 이유
## 📝 Intro
- 대규모 데이터에서 게시글 목록 조회가 복잡한 이유는 여러 가지가 있습니다.
- 일반적으로 소규모 데이터베이스에서는 단순한 `SELECT * FROM posts ORDER BY created_at DESC LIMIT 10` 같은 쿼리로 쉽게 해결할 수 있지만, **수백만~수억 개의 데이터를** 다뤄야 하는 시스템에서는 여러 복잡한 문제가 발생합니다.

## ✅1️⃣ 대규모 데이터에서 게시글 목록 조회가 복잡한 이유
### 1️⃣ 데이터의 양이 방대함
- 게시글이 많아질수록 ORDER BY와 LIMIT 연산의 부담이 커짐
- 인덱스를 사용하더라도 **디스크의 I/O 비용이 증가**하고 **캐싱이 어렵게 됨**

### 📝 예제 쿼리
```sql
SELECT * FROM posts ORDER BY created_at DESC LIMIT 10;
```

### ❌ 문제점
- ORDER BY create_at DESC 실행 시 **모든 게시글을 정렬**해야 함 (데이터가 클수록 느려짐)
- 최신 게시글이 많으면 **새로운 데이터가 계속 추가되며 인덱스가 자주 변경됨**

### 2️⃣ 페이지네이션 (Pagination) 성능 저하
- 게시판은 보통 페이지네이션을 지원해야 합니다.
- 즉, LIMIT과 OFFSET을 사용해 특정 페이지의 게시글을 조회해야 하는데, 대규모 데이터에서는 **OFFSET이 클수록 성능이 저하됩니다.**
```sql
SELECT * FROM posts ORDER BY created_at DESC LIMIT 10 OFFSET 1000000;
```

### ❌ 문제점
- OFFSET 1000000을 사용하면 앞의 **100만 개 데이터를 스캔하고 버린 후** 그 다음 10개를 반환합니다.
- 데이터가 많을수록 **불필요한 연산이 많아지고 성능이 급격히 저하됩니다.**

### ✅ 해결 방법
- **Keyset pagination (Seek 방식) 활용 ➞** OFFSET 없이 WHERE 조건을 활용
- LIMIT을 활용해 **이전 조회된 마지막 ID를 기준으로 다음 데이터를 가져오기**
```sql
SELECT * FROM posts WHERE created_at < '2024-03-11 10:00:00' ORDER BY created_at DESC LIMIT 10;
```

### 3️⃣ 인덱스 사용 최적화 문제
#### 📌 왜 인덱스만으로 해결되지 않을까?
- **인덱스를 사용하면 정렬이 빨라지지만,** 데이터가 많을 경우에도 여전히 **디스크 I/O 비용이 증가**
- 새로운 게시글이 추가될 때마다 **B-Tree 인덱스가 갱신**되며 성능에 부담을 줌

#### ✅ 해결 방법
- **커버링 인덱스 (Covering Index) 활용 ➞** SELECT에 포함된 모든 컬럼을 인덱스에 미리 포함
- **파티셔닝 (Partitioning) ➞** 날짜별 테이블 분리 (posts_202403 같은 테이블)
```sql
CREATE INDEX idx_posts_created ON posts(created_at, title, content);
```

### 4️⃣ 트래픽 분산 문제
- 게시글 목록은 대부분 서비스에서 **조회 트래픽이 많고, 쓰기 트래픽도 꾸준히 발생하는 구조입니다.**
- 즉, **읽기(READ)가 많지만, 동시에 새로운 게시글이 추가되며 정렬 순서가 계속 바뀝니다.**

#### ❌ 문제점
- 캐시 사용이 어렵다 ➞ 새로운 게시글이 추가되면 정렬 순서가 바뀌어 기존 캐시 무효화됨
- 동시성이 증가하면 DB 부하가 커짐 ➞ 많은 유저가 같은 목록을 요청하면 DB 부하 증가

#### ✅ 해결 방법
**1️⃣ 캐싱 (Redis, Memcached) 활용**
- 최신 게시글 목록을 Redis에 저장하고, 데이터가 변경될 때만 업데이트.
```java
redisTemplate.opsForValue().set("latest_posts", posts, Duration.ofSeconds(60));
```

**2️⃣ 읽기 전용 데이터베이스(Read Replica) 활용**
- Master-Slave Replication을 사용하여 **읽기 트래픽을 Slave DB로 분산**
```sql
SELECT * FROM posts ORDER BY created_at DESC LIMIT 10;
-- 이 쿼리를 Slave DB에서 실행하여 Master DB 부하 감소
```

**3️⃣ CQRS 패턴 적용**
- 게시글 조회와 생성/수정을 다른 DB로 분리 (읽기 전용 DB, 쓰기 전용 DB)

### 5️⃣ JOIN 연산 성능 문제
- 게시글 목록을 조회할 때 보통 **작성자 정보, 좋아요 수, 댓글 수** 등을 함께 조회해야 합니다.
- 즉, JOIN을 수행해야 하는데, 데이터가 많을수록 성능이 저하됩니다.
```sql
SELECT p.id, p.title, p.content, u.name, COUNT(c.id) as comment_count
FROM posts p
LEFT JOIN users u ON p.user_id = u.id
LEFT JOIN comments c ON p.id = c.post_id
GROUP BY p.id, u.name
ORDER BY p.created_at DESC LIMIT 10;
```

#### ❌ 문제점
- JOIN을 수행할 때 **데이터가 많으면 메모리와 CPU가 필요**
- GROUP BY를 수행하면 **정렬 및 집계 연산이 필요**하여 성능이 더 느려짐

#### ✅ 해결 방법
- **NoSQL(예: Redis, Elasticsearch) 캐시 활용 ➞** 좋아요 수, 댓글 수는 미리 저장해둠
- **CQRS 패턴 적용 ➞** 별도 테이블에 **미리 계산된 카운트** 값을 저장
```sql
SELECT p.id, p.title, p.title, u.name, p.comment_count
FROM posts p
LEFT JOIN users u ON p.user_id = u.id
ORDER BY p.created_at DESC LIMIT 10;
```

- p.comment_count는 미리 계산된 값이므로 JOIN을 줄여 성능을 개선할 수 있습니다.

## ✅2️⃣ 대규모 게시글 조회 시 최적화 방법
### 1️⃣ Keyset Pagination 사용
```sql
SELECT * FROM posts WHERE created_at < '2024-03-11 10:00:00' ORDER BY created_at DESC LIMIT 10;
```

- OFFSET 없이 **이전 조회된 마지막 ID를 기준으로 다음 데이터를 가져오는 방식**

### 2️⃣ 캐싱 활용 (Redis, Elasticsearch)
```java
redisTemplate.opsForValue().set(CACHE_KEY, posts, Duration.ofSeconds(60))
```

- 최신 게시글을 캐시에 저장하여 **DB 부하를 줄임**

### 3️⃣ 읽기 전용 DB(Read Replica) 활용
```sql
SELECT * FROM posts ORDER BY created_at DESC LIMIT 10;
```

- 읽기 트래픽을 **Replica DB로 분산**하여 성능 최적화

### 4️⃣ 미리 계산된 값 사용
```sql
SELECT p.id, p.title, p.content, u.name, p.comment_count
FROM posts p
LEFT JOIN users u ON p.user_id = u.id
ORDER BY p.created_at DESC LIMIT 10;
```

- 댓글 개수, 좋아요 수 등을 **미리 계산된 값으로 저장**하여 JOIN 연산 줄이기

## ✅3️⃣ 결론
✅ 대규모 데이터에서 게시글 목록 조회가 복잡한 이유는?
- **데이터 양이 많아 ORDER BY LIMIT가 비효율적**
- **OFFSET이 클수록 성능 저하 (페이징 문제)**
- **JOIN 연산이 많을수록 성능 저하**
- **쓰기 트래픽이 많아지면 인덱스 관리 부담 증가**
- **캐시가 자주 무효와되어 DB 부하가 커짐**

**✅ 최적화 방법**
**1️⃣ Keyset Pagination ➞** OFFSET 대신 마지막 ID 기반 조회
**2️⃣ Redis, Elasticsearch 캐싱 ➞** 최신 데이터 미리 저장
**3️⃣ 읽기 전용 DB(Read Replica) 활용 ➞** 조회 트래픽 분산
**4️⃣ 미리 계산된 데이터 활용 ➞** JOIN 최소화
