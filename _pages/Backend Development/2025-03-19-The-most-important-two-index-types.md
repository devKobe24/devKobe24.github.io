---
title: "📚[Backend Development] 가장 중요한 두 가지 인덱스 유형."
tags:
    - Backend Ddevelopment
date: "2025-03-19"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 가장 중요한 두 가지 인덱스 유형."
## 📝 Intro
데이터베이스에서 **Index**는 검색 성능을 최적화하는 핵심 요소입니다.
그 중에서 **Clustered Index(클러스터형 인덱스)와 Secondary Index(보조 인덱스, Non-Clustered Index)는** 가장 중요한 두 가지 인덱스 유형입니다.

## ✅1️⃣ Clustered Index (클러스터형 인덱스)
### ✅ 개념
- **테이블의 데이터를 물리적으로 정렬하는 인덱스.**
- **한 테이블에 하나만 존재할 수 있음.**
- **기본 키(Primary Key)** 에 자동으로 생성됨.
- **데이터 자체가 인덱스 트리(B+ Tree) 구조로 저장됨.**

### ✅ 특징
- **✅ 데이터 자체가 정렬된 형태로 저장됨.**
- **✅ Primary Key(기본 키)에 자동 생성됨.**
- **✅ 범위 검색(BETWEEN, ORDER BY)이 빠름.**
- **✅ 테이블당 하나만 존재.**

### ✅ 예제
```sql
CREATE TABLE users (
    id INT PRIMARY KEY, -- 자동으로 Clusterd Index 생성
    name VARCHAR(100),
    age INT
);
```

- 위 테이블에서 id는 **기본 키(Primary Key)** 이므로 자동으로 **Clustered Index**가 생성됩니다.
- 즉, 테이블의 데이터가 **id 값을 기준으로 정렬된 상태**로 저장됩니다.

## ✅2️⃣ Secondary Index (보조 인덱스, Non-Clustered Index)
### ✅ 개념
- **Clustered Index와 별개로 추가적인 검색 속도를 높이기 위해 사용하는 인덱스.**
- **데이터와 별도로 저장되며, Clustered Index의 값(Primary Key)을 참조.**
- **한 테이블에 여러 개 생성 가능.**

### ✅ 특징
- **✅ 데이터 정렬에는 영향을 주지 않음**
- **✅ 테이블당 여러 개 생성 가능**
- **✅ Clustered Index를 기반으로 추가적인 검색 속도 향상**
- **✅ 범위 검색보다는 특정 값 검색(WHERE 조건)에 적합**

### ✅ 예제
```sql
CREATE INDEX idx_users_name ON users(name);
```

- name 컬럼에 **보조 인덱스(Secondary Index)를** 생성하여 **이름 검색 속도를 최적화.**

## ✅3️⃣ Clustered Index vs Secondary Index 비교

|구분|Clustered Index|Secondary Index (Non-Clustered Index)|
| -------- | -------- | -------- |
|정렬 방식|데이터 자체가 인덱스 트리에 정렬됨|데이터 정렬에 영향을 주지 않음|
|저장 방식|데이터 자체가 인덱스 노드에 저장됨|인덱스에 Primary Key를 저장 후 데이터 참조|
|속도|범위 검색(BETWEEN, ORDER BY) 최적|특정 값 검색(WHERE) 최적|
|개수|한 테이블에 하나만 존재|여러 개 존재 가능|
|예제|PRIMARY KEY (id)|CREATE INDEX idx_name ON users(name)|

## ✅4️⃣ Clustered Index & Secondary Index 검색 과정.
### 📌 Clustered Index 검색 과정
```sql
SELECT * FROM users WHERE id = 100;
```

- **Clustered Index는 데이터 자체가 정렬된 상태이므로, id = 100을 바로 찾을 수 있음.**
- B+ Tree에서 **한 번의 검색으로 데이터까지 도달 → 빠른 조회 속도.**

### 📌 Secondary Index 검색 과정
```sql
SELECT * FROM users WHERE name = 'Alice';
```

- name 컬럼은 Secondary Index이므로, **먼저 보조 인덱스를 검색한 후, Primary Key 값을 찾아 Clustered Index에서 데이터 검색.**
- **"Secondary Index ➞ Clustered Index" 두 단계 검색 과정이 필요**하여 Clustered Index보다 속도가 약간 느림.

## ✅5️⃣ 언제 Clustered Index & Secondary Index를 사용해야 할까?
### ✅ Clustered Index 추천
- PRIMARY KEY와 같이 **데이터 정렬이 중요한 경우.**
- ORDER BY, BETWEEN, RANGE QUER를 자주 사용해야 하는 경우.

### ✅ Secondary Index 추천
- 특정 컬럼을 WHERE 조건으로 자주 검색해야 할 때.
- JOIN 또는 GROUP BY 연산이 많은 경우.
- 보조적인 검색 속도를 높이고 싶을 때.

## 📌 결론.
- **✅ Clustered Index는 데이터 자체를 정렬하여 저장하며, Primary Key에 자동으로 생성됨**
- **✅ Secondary Index는 추가적인 검색 최적화를 위해 사용되며, 테이블당 여러 개 생성 가능**
- **✅ 범위 검색(ORDER BY, BETWEEN)은 Clustered Index가 유리**
- **✅ 특정 컬럼 검색(WHERE 조건)은 Secondary Index가 유리.**
