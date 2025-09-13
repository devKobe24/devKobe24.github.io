---
title: "📚[Backend Development] SQL에서 INDEX"
tags:
    - Backend Ddevelopment
date: "2025-03-18"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] SQL에서 INDEX"
## 📝 Intro.
- **인덱스(INDEX)는** 데이터베이스에서 **검색 성능을 최적화하기 위해 사용하는 자료구조입니다.**
- 마치 책의 **목차(Table of Contents)처럼,** 테이블에서 데이터를 **더 빠르게 찾을 수 있도록 돕는 기능**입니다.

## ✅1️⃣ 인덱스의 역할
- **✅ 검색 속도 향상 ➞** WHERE, ORDER BY, JOIN 시 빠르게 데이터 조회 가능
- **✅ 데이터 정렬 최적화 ➞** ORDER BY 연산 시 정렬 속도 향상
- **✅ 중복 방지 ➞** UNIQUE INDEX를 사용하여 중복 데이터 삽입 방지

### 📝 예제 (인덱스가 없는 경우)
```sql
SELECT * FROM users WHERE email = 'example@email.com';
```

- 데이터베이스는 **모든 행을 하나씩 검사(Full Table Scan)** 해야 함
- 데이터가 많을수록 검색 속도가 느려짐

### 📝 예제 (인덱스가 있는 경우)
```sql
CREATE INDEX idx_email ON users(email);
SELECT * FROM users WHERE email = 'example@email.com';
```

- **이진 탐색(Binary Search)을** 통해 빠르게 검색 가능
- **Full Table Scan을 방지하고 인덱스를 활용하여 검색 속도 향상**

## ✅2️⃣ 인덱스의 종류
### 1️⃣ 기본(B-Tree) 인덱스
- 가장 일반적인 인덱스, **B-Tree(Balanced Tree) 구조 사용**
- 검색, 정렬, 범위 조회에 최적화

```sql
CREATE INDEX idx_name ON users(name);
```

### 2️⃣ UNIQUE 인덱스
- **중복방지를 위한 인덱스** (중복된 값 삽입 불가)

```sql
CREATE UNIQUE INDEX idx_unique_email ON users(email);
```

```sql
INSERT INTO users (id, email) VALUES (1, 'test@email.com');
INSERT INTO users (id, email) VALUES (2, 'test@email.com'); -- ❌ 오류 발생 (중복)
```

### 3️⃣ 복합(Composite) 인덱스
- **두 개 이상의 컬럼을 결합하여 인덱스를 생성**
- 검색 조건이 여러 개일 때 유용

```sql
CREATE INDEX idx_name_email ON users(name, email);
```

```sql
SELECT * FROM users WHERE name = 'Alice' AND email = 'alice@email.com';
```

### 4️⃣ FULLTEXT 인덱스 (MySQL 전용)
- 긴 텍스트 데이터(TEXT, VARCHAR)에서 **단어 검색**이 필요할 때 사용
- `LIKE '%keyword%'`보다 훨씬 빠른 검색 가능

```sql
CREATE FULLTEXT INDEX idx_content ON posts(content);
SELECT * FROM posts WHERE MATCH(content) AGAINST ('database');
```

### 5️⃣ HASH 인덱스
- **정확한 일치검색(Equality Search)에 최적화됨**
- **범위 검색에는 적합하지 않음**

```sql
CREATE INDEX idx_hash_email USING HASH ON users(email);
```

```sql
SELECT * FROM users WHERE email = 'test@email.com'; -- ✅ 빠름
SELECT * FROM users WHERE email = LIKE 'test%'; -- ❌ 느림 (HASH 인덱스는 범위 검색 지원 안 함)
```

## ✅3️⃣ 인덱스의 성능 고려 사항
- **✅ 인덱스는 빠른 검색을 제공하지만, 무조건 많이 만든다고 좋은 것은 아닙니다.**
- **✅ 인덱스가 많을수록 삽입(INSERT), 수정(UPDATE), 삭제(DELETE) 연산 속도가 느려집니다.**
- **✅ 자주 사용하는 조회 쿼리에 맞춰 필요한 인덱스만 생성하는 것이 중요합니다**

## ✅4️⃣ 인덱스 최적화 및 활용
### 📌 실행 계획(EXPLAIN) 확인

인덱스를 잘 활용하는지 확인하려면 **EXPLAIN 명령어를** 사용하면 됩니다.

```sql
EXPLAIN SELECT * FROM users WHERE email = 'test@email.com';
```

- **Using index ➞** 인덱스를 사용하여 최적화된 쿼리
- **Using full table scan ➞** 테이블 전체 검색 (느림)

## ✅5️⃣ 결론
- **✅ 인덱스는 데이터 검색 속도를 최적화하는 핵심 도구**
- **✅ 너무 많은 인덱스는 오히려 성능을 저하시킬 수 있음**
- **✅ 조회 성능을 높이려면 EXPLAIN을 사용하여 인덱스 최적화**
