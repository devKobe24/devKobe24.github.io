---
title: "📚[Backend Development] 대표적인 인덱스 자료구조"
tags:
    - Backend Ddevelopment
date: "2025-03-19"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] SQL에서 INDEX"
## 📝 Intro.
인덱스는 데이터베이스에서 검색 성능을 최적화하기 위해 사용되며, **데이터의 특성과 쿼리 유형에 따라 다양한 자료구조가 활용됩니다.**

## ✅1️⃣ B+ Tree (Balanced Plus Tree)
### ✅ 개념
- 대부분의 **관계형 데이터베이스(RDBMS)에서** 기본적으로 사용하는 인덱스 구조.
- **B-Tree의 확장형**으로, 리프 노트(Leaf Node)에만 데이터를 저장하며 **범위 검색이 빠름.**
- **균형 트리(Balanced Tree)** 구조로, 검색,삽입,삭제 연산이 $O(log N)$.
- **순차적 검색(ORDER BY, RANGE QUERY)에 최적화됨.**

### ✅ 특징
- **📌 검색, 삽입, 삭제 모두 $O(log N)$**
- **📌 ORDER BY, BETWEEN 검색이 빠름**
- **📌 데이터가 정렬된 상태로 유지됨**

### ✅ 사용 예시
```sql
CREATE INDEX idx_user_name ON users(name);
SELECT * FROM users WHERE name LIKE 'A%';
```

- LIKE 'A%' 같은 **접듀서 검색(범위 검색)** 시 최적화됨.

### ✅ 사용 DBMS
- 📌 MySQL (InnoDB)
- 📌 PostgreSQL
- 📌 Oracle
- 📌 SQL Server

## ✅2️⃣ Hash Index
### ✅ 개념
- **정확한 값 검색(Equality Search)에 최적화**된 해시 테이블 기반 인덱스.
- **WHERE column = 'value'** 같은 조건에 최적화.
- **순차적 검색이나 범위 검색을 지원하지 않음.**

### ✅ 특징.
- **📌 검색이 O(1)에 가까움 (해시 충돌이 없을 경우)**
- **📌 WHERE 절의 = 연산에 최적**
- **📌 ORDER BY, 범위 검색 불가능**

### ✅ 사용 예시
```sql
CREATE INDEX idx_email_hash USING HASH ON users(email);
SELECT * FROM users WHERE email = 'user@example.com';
```

- **이메일 주소 검색** 같은 정확한 값 조회에 적합.

### ✅ 사용 DBMS
- 📌 MySQL (MEMORY Engine)
- 📌 PostgreSQL
- 📌 Redis
- 📌 DynamoDB

## ✅3️⃣ LSM Tree (Log-Structured Merge Tree)
### ✅ 개념
- **쓰기 성능 최적화**를 위한 **NoSQL 및 시계열 데이터베이스(TSDB)에서** 많이 사용됨
- **데이터를 메모리에 먼저 저장 후, 일정 크기가 되면 디스크에 병합(Merge)하는 방식.**
- **읽기 성능이 상대적으로 낮지만, 쓰기(Writes) 성능이 뛰어남.**

### ✅ 특징
- **📌 쓰기 성능이 뛰어남 (배치 삽입/삭제 최적화)**
- **📌 NoSQL, 로그 데이터, 시계열 데이터에 최적**
- **📌 랜덤 읽기가 느리면, 병합(Compaction) 비용 발생**

### ✅ 사용 예시
- **Cassandra, RocksDB, LevelDB에서 기본 인덱스로 사용됨**

### ✅ 사용 DBMS
- 📌 Apache Cassandra
- 📌 RocksDB
- 📌 LevelDB
- 📌 Amazon DynamoDB

## ✅4️⃣ R-Tree (Rectangle Tree)
### ✅ 개념
- **공간(Spatial) 데이터 검색**에 최적화된 트리 구조
- **위도/경도, 좌표 정보(GIS 데이터)를** 검색할 때 주로 사용됨
- **2D, 3D 범위 검색(WHERE lat BETWEEN ... AND lon BETWEEN ...)에 유리.**

### ✅ 특징
- **📌 위치 기반 검색(GIS) 최적화**
- **📌 범위 검색(RANGE QUERY) 가능**
- **📌 이진 트리가 아닌 공간 분할 트리 구조**

### ✅ 사용 예시
```sql
CREATE INDEX idx_location ON locatioons USING GIST (geom);
SELECT * FROM locations WHERE ST_Within(geom, ST_GeomFromText('POLYGON((...))'));
```

- **PostGIS, Oracle Spatial**에서 사용됨

### ✅ 사용 DBMS
- 📌 PostgreSQL (PostGIS)
- 📌 Oracle Spatial
- 📌 MySQL (GIS 가능)

## ✅5️⃣ Bitmap Index
### ✅ 개념
- **중복 값이 많은 컬럼(성별, 상태값 등)에 최적화된 인덱스.**
- 각 값에 대해 **비트맵(Bit Map)으로 저장하여 검색 속도를 향상.**
- **데이터 웨어하우스, OLAP(Online Analytical Processing)에서 주로 사용.**

### ✅ 특징
- **📌 중복이 많은 데이터에 최적**
- **📌 저장 공간이 적게 들고, 빠른 검색 가능**
- **📌 쓰기(INSERT/UPDATE/DELETE) 성능이 낮음**

### ✅ 사용 예시
```sql
CREATE BITMAP INDEX idx_status ON orders(status);
SELECT * FROM orders WHERE status = 'completed';
```

- **상태(status) 컬럼처럼 중복이 많은 데이터 검색 시 빠름.**

### ✅ 사용 DBMS
- 📌 Oracle
- 📌 PostgreSQL
- 📌 ClickHouse

## ✅6️⃣ Skip List
### ✅ 개념
- **연결 리스트 기반 인덱스 구조로, 정렬된 데이터를 빠르게 검색.**
- **Redis의 Sorted Set(순위 랭킹)에서 사용됨.**

### ✅ 특징
- **📌 B+Tree보다 간단한 구조**
- **📌 읽기/쓰기 속도가 균형적**
- **📌 Redis에서 순위 기반 랭킹(Key-Value 저장소)에 사용**

### ✅ 사용 예시 (Redis)
```java
redisTemplate.opsForZSet().add("ranking", "user1", 100);
redisTemplate.opsForZSet().add("ranking", "user2", 150);
```

- **점수 기반 랭킹을 빠르게 검색 가능.**

### ✅ 사용 DBMS
- 📌 Redis
- 📌 Apache Ignite

## ✅7️⃣ 결론
- **✅ 관계형 데이터베이스(RDBMS)에서 가장 많이 사용하는 인덱스 ➞ B+ Tree**
- **✅ 빠른 키-값 조회(Hash 검색)에 적합한 인덱스 ➞ Hash Index**
- **✅ 쓰기 성능 최적화 (NoSQL, 로그 데이터) ➞ LSM Tree**
- **✅ 공간 데이터(GIS, 위치 정보) 검색 ➞ R-Tree**
- **✅ 중복 데이터(성별, 상태값) 검색 최적화 ➞ Bitmap Index**
- **✅ 순위 랭킹, Redis Sorted Set ➞ Skip List**
