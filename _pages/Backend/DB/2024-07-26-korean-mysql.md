---
title: "💾[Database] MySQL DB에 한글 삽입."
tags:
    - Database
    - MySQL
date: "2024-07-26"
thumbnail: "/assets/img/thumbnail/database.jpeg"
---

# 💾[Database] MySQL DB에 한글 삽입.

- 한글을 MySQL 데이터베이스에 삽입하려고 할 때 발생하는 오류는 주로 데이터베이스, 테이블 또는 열의 문자 세트와 관련 있습니다.
    - 이 문제를 해결하기 위해서는 데이터베이스와 테이블의 문자 세트를 UTF-8로 설정해야 합니다.

## 🙋‍♂️ 데이터베이스와 테이블의 문자 세트를 UTF-8로 설정하는 방법.

### 1️⃣ 데이터베이스 생성 시 문자 세트 설정.

```sql
CREATE DATABASE {데이터베이스 이름} CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### 2️⃣ 기존 데이터베이스의 문자 세트 변경.

```sql
ALTER DATABASE {데이터베이스 이름} CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### 3️⃣ 테이블 생성 시 문자 세트 설정.

```sql
CREATE TABLE test (
    id INT AUTO_INCREMENT PRIMARY KEY,
    content TEXT
) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### 4️⃣ 기존 테이블의 문자 세트 변경.

```sql
ALTER TABLE test CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### 5️⃣ 각 열의 문자 세트 확인 및 변경.

```sql
ALTER TABLE test MODIFY content TEXT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### 6️⃣ MySQL 서버의 기본 문자 세트를 변경.
- my.cnf(또는 my.ini) 파일을 수정하여 기본 문자 세트를 utf8mb4로 설정합니다.
    - 보통 이 파일은 **`/etc/my.cnf`** 또는 **`/etc/mysql/my.cnf`** 에 위치해 있습니다.
- my.cnf 파일에 다음 내용을 추가합니다.
```ini
[client]
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4

[mysqld]
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
```

### 7️⃣ MySQL 서버 재시작.
```shell
sudo systemctl restart mysqld
```

- 이제 한글을 포함한 데이터를 데이터베이스에 삽입할 수 있을 것입니다.
    - 예를 들어, 한글 데이터를 삽입하려면:
```sql
INSERT INTO test (content) VALUES ('테스트 데이터');
```
- 이 방법으로 UTF-8 설정을 적용하면 한글 데이터를 MySQL 데이터베이스에 문제 없이 저장할 수 있습니다.
