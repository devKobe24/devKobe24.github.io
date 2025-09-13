---
title: "📚[Backend Development] Covering Index"
tags:
    - Backend Ddevelopment
date: "2025-03-20"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] Covering Index."
## 📝 Intro
**Covering Index(커버링 인덱스)는 쿼리 실행 시, 인덱스만으로 필요한 데이터를 모두 충족하는 경우를** 의미합니다.
즉, **테이블(원본 데이터)에 접근하지 않고 인덱스만으로 결과를 가져올 수 있는 인덱스**입니다.

## ✅1️⃣ Covering Index의 핵심 개념.
### 1️⃣ 인덱스 스캔(Index Scan)만으로 필요한 모든 데이터를 조회.
- 일반적으로 인덱스를 사용해도 일부 컬럼만 검색한 후, 추가적으로 테이블에서 데이터를 가져와야 하는 경우가 있음.
- 하지만 **Covering Index는 필요한 모든 데이터가 인덱스에 포함**되어 있어, 테이블 조회를 생략할 수 있음.

### 2️⃣ 쿼리 성능 최적화.
- **디스크 I/O 감소 :** 테이블을 읽지 않으므로 I/O 비용 절감.
- **쿼리 속도 향상 :** 인덱스만 조회하면 되므로 실행 속도가 빨라짐.
- **Random I/O 감소 :** 테이블 데이터 접근이 필요 없으므로 불필요한 I/O를 최소화함.

## ✅2️⃣ Covering Index 동작 방식.
### 💎 예제 테이블 (MySQL 기준):
```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    age INT,
    city VARCHAR(100)
);
```

### 💎 일반적인 인덱스 사용 (테이블 조회 필요)
```sql
SELECT name FROM users WHERE age = 30;
```

- age 컬럼에 인덱스가 있어도, name 컬럼을 가져오기 위해 **테이블 조회(데이터 페이지 접근)가 필요함.**

### 💎 Covering Index 사용
```sql
CREATE INDEX idx_users_age_name ON users (age, name);
```

- **인덱스(idx_users_age_name)가 age와 name 컬럼을 모두 포함하므로, 테이블을 조회할 필요 없음.**
- 인덱스에서 직접 데이터를 가져오므로 **쿼리 속도가 크게 향상됨.**

## ✅3️⃣ Covering Index 확인 방법 (MySQL)
쿼리가 Covering Index를 사용하는지 확인하려면 **EXPLAIN 명령어를 사용하면 됩니다.**

```sql
EXPLAIN SELECT name FROM users WHERE age = 30;
```

- 📌 Extra 컬럼에 "Using index"가 표시되면 Covering Index가 적용된 것 입니다.

## 🚀 결론
- **✅ Covering Index는 테이블을 조회하지 않고, 인덱스만으로 데이터를 가져올 수 있는 최적화 기법**
- **✅ 디스크 I/O와 Random I/O를 줄여 성능을 크게 향상시킬 수 있음**
- **✅ 인덱스 크기가 커질 수 있으므로, 적절한 컬럼만 포함하여 생성하는 것이 중요.**
