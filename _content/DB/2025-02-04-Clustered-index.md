---
title: "💾[Database] Clustered Index는 무엇일까요?"
tags:
    - Database
    - Native SQL Query
    - MySQL
date: "2025-02-04"
thumbnail: "/assets/img/thumbnail/database.jpeg"
---

# "💾[Database] Clustered Index는 무엇일까요?"
## 🍎 Intro.
- **Clusterd Index**는 데이터베이스 테이블에서 **데이터가 물리적으로 정렬된 방식으로 저장되는 인덱스입니다.**
    - 이 인덱스를 사용하면 **인덱스가 곧 데이터이며, 인덱스를 조회할 때 별도로 데이터를 찾아가는 과정이 필요하지 않습니다.**

## ✅1️⃣ Clustered Index의 특징.
- **1. 인덱스와 데이터가 동일**
    - 클러스터형 인덱스는 테이블의 **행(ROW)** 자체를 정렬된 상태로 저장합니다.
        - 즉, 인덱스의 키 값이 데이터의 저장 순서를 결정합니다.
    - 하나의 테이블에는 **하나의 클러스터형 인덱스**만 존재할 수 있습니다.

- **2. 데이터가 정렬되어 저장**
    - 데이터는 클러스터형 인덱스의 **키 값 순서대로 물리적으로 정렬됩니다.**
        - 예: PRIMARY KEY를 기준으로 정렬된 테이블.

- **3. 빠른 검색**
    - 클러스터형 인덱스는 데이터가 이미 정렬되어 있으므로 **범위 검색**이나 **정렬 쿼리**에서 성늠이 매우 뛰어납니다.

- **4. 삽입/삭제 성능 저하 가능성**
    - 데이터가 정렬된 상태를 유지해야 하므로, **삽입/삭제 시 성능이 떨어질 수 있습니다**(특히 중간에 삽입할 경우).

## ✅2️⃣ Clustered Index의 실제 예제
### 1️⃣ 예제: PRIMARY KEY로 생성된 클러스터형 인덱스.
- MySQL에서 기본적으로 **기본 키(Primary Key)는** 클러스터형 인덱스로 생성됩니다.

```sql
CREATE TABLE article (
    article_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255),
    created_at DATETIME
);
```

- **결과 :**
    - article_id를 기준으로 데이터가 정렬된 상태로 저장됩니다.
    - PRIMARY KEY가 클러스터형 인덱스 역할을 합니다.

### 2️⃣ 예제: UNIQUE를 기준으로 클러스터형 인덱스 생성.
- MySQL에서는 특정 열을 기준으로 클러스터형 인덱스를 생성할 수도 있습니다.

```sql
CREATE TABLE user (
    user_id INT AUTO_INCREMENT,
    username VARCHAR(255) UNIQUE,
    email VARCHAR(255),
    PRAIMARY KEY (user_id),
    UNIQUE KEY idx_username (username)
);
```

- **결과:**
    - PRIMARY KEY(user_id)가 클러스터형 인덱스로 사용됩니다.
    - username은 정렬되지 않고, 별도의 비클러스터형(Non-Clustered) 인덱스로 저장됩니다.

## ✅3️⃣ Clustered Index의 장점.
- **1. 빠른 검색:**
    - 데이터가 인덱스 키 값으로 정렬되어 있으므로, **범위 검색**(예: BETWEEN, ORDER BY)에서 빠른 성능 제공.
- **2. 데이터 접근 최소화:**
    - 인덱스가 곧 데이터이므로, 별도의 포인터를 따라가지 않아도 됨.
- **3. 효율적인 저장 공간:**
    - 데이터와 인덱스가 결합되어 별도의 인덱스 저장 공간이 필요 없음.

## ✅4️⃣ Clustered Index의 단점.
- **1. 삽입/삭제 성능 저하:**
    - 데이터가 항상 정렬된 상태를 유지해야 하므로, 중간에 삽입하거나 삭제할 경우 추가적인 비용 발생.
- **2. 업데이트 성능 문제:**
    - 클러스터형 인덱스 키 값이 변경되면 데이터의 물리적 재배열이 필요할 수 있음.
- **3. 하나만 생성 가능:**
    - 한 테이블에 하나의 클러스터형 인덱스만 허용되므로, 여러 정렬 기준이 필요하면 비클러스터형 인덱스를 사용해야 함.

## ✅5️⃣ Clustered Index가 적합한 경우.
- **1. 범위 검색이 빈번한 경우:**
    - 예시:
    ```sql
    SELECT *
    FROM orders
    WHERE order_date
    BETWEEN '2025-01-01' AND '2025-01-31';
    ```

- **2. 정렬된 결과가 필요한 경우:**
    - 예시:
    ```sql
    SELECT *
    FROM article
    ORDER BY created_at DESC;
    ```

- **3. 데이터 읽기가 많은 경우:**
    - 데이터 읽기 성능이 중요할 때 클러스터형 인덱스를 활용하면 효율적.

## 🚀 정리.
- **Clustered Index**는 데이터가 인덱스 키 값 순서대로 정렬된 상태로 저장되는 인덱스입니다.
- **장점 :** 범위 검색과 정렬 쿼리에서 매우 효율적.
- **단점 :** 삽입/삭제 성능 저하 가능, 한 테이블에서 하나만 생성 가능.
- MySQL에서는 **PRIMARY KEY**가 기본적으로 클러스터형 인덱스로 사용됩니다.
