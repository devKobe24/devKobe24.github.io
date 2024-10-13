---
title: "💾[Database] 관계형 데이터베이스(Relational Database, RDB)란 무엇일까요?"
tags:
    - Database
date: "2024-10-13"
thumbnail: "/assets/img/thumbnail/database.jpeg"
---

# 💾[Database] 관계형 데이터베이스(Relational Database, RDB)란 무엇일까요?

- **관계형 데이터베이스(Relational Database, RDB)는** 테이블 형식으로 데이터를 저장하고, 테이블 간의 관계를 정의하여 데이터를 관리하는 **데이터 모델**입니다.
- 이를 관리하기 위해 **데이터베이스 관리 시스템(Database Management System, DBMS)이** 사용되며, RDB(Relational Database, 관계형 데이터베이스)를 관리하는 DBMS(Database Management System, 데이터베이스 관리 시스템)는 **관계형 데이터베이스 관리시스템(RDBMS, Relational Database Management System)이라고** 합니다.
- 관계형 데이터베이스(Relational Database, RDB)는 데이터를 **행(Row)과 열(Column)로** 구성된 **테이블(Table)에** 저장되며, 각 테이블은 서로 **관계를 맺어** 구조화된 데이터를 관리하고 검색하기 쉽게 합니다.

## 1️⃣ 관계형 데이터베이스(RDB, Relational Database)의 주요 특징.

### 1️⃣ 테이블 구조.
- 관계형 데이터베이스(RDB, Relational Database)는 데이터를 **테이블 형태**로 저장합니다.
    - 각 테이블은 **행(Row)과 열(Column)로** 구성되며, 행(Row)은 **레코드(Record)라고** 하고, 열(Column)은 **속성(Attribute) 또는 필드(Field)라고** 부릅니다.
- 예를 들어, "사용자"라는 테이블이 있다면, 각 행(Row)은 **사용자 정보(이름, 이메일, 나이 등)를** 나타내고, 각 열(Column)은 **사용자 정보를 설명하는 속성(Attribute)을** 나타냅니다.

### 2️⃣ 관계.
- 관계형 데이터베이스(Relational Database, RDB)는 **여러 테이블 간의 관계를 정의할 수 있습니다.**
    - **키(Key)를** 사용해 테이블 간의 관계를 맺고, 데이터를 참조하고 연결할 수 있습니다.
- 예를 들어, "사용자" 테이블과 "주문" 테이블이 있다고 하면, "사용자" 테이블의 기본 키(Primary Key)가 "주문" 테이블에 **외래 키(Foreign Key)로** 사용되며, 사용자와 주문 간의 관계를 나타낼 수 있습니다.

### 3️⃣ 고유한 키(Primary Key).
- 각 테이블에는 **고유한 키(Primary Key)가** 있습니다.
    - 이 키는 테이블의 각 레코드(Record)를 **유일하게 식별하는 역할을 합니다.**
        - 기본 키(Primary Key)는 중복될 수 없으며, 이를 통해 데이터를 **정확하게 검색하고 관리할 수 있습니다.**

### 4️⃣ 데이터 무결성 및 일관성.
- 관계형 데이터베이스(Relational Database, RDB)는 **데이터 무결성(Integrity)과 일관성(Consistency)을** 유지하도록 설계되었습니다.
    - 이를 통해 데이터의 정확성과 신뢰성을 보장할 수 있습니다.
- **제약 조건(Constraints)을** 통해 데이터의 무결성(Integrity)을 유지합니다.
    - 예를 들어, **NOT NULL** 제약 조건을 사용하여 특정 열이 항상 값을 가져야 한다는 것을 보장하거나, **FOREIGN KEY** 제약 조건을 사용해 테이블 간의 관계를 유지합니다.

### 5️⃣ SQL(Structured Query Language)
- 관계형 데이터베이스(Relational Database, RDB)는 데이터를 관리하기 위해 **SQL(Structured Query Language)이라는** 언어를 사용합니다.
    - SQL(Structured Query Language)은 데이터를 **조회, 삽입, 수정, 삭제**하는 데 사용되는 **표준 언어**입니다.
        - SQL(Structured Query Language)은 관계형 데이터베이스(Relational Database, RDB)에서 데이터 검색과 조작을 위한 강력한 도구입니다.
- 예: 데이터를 삽입하기 위한 `INSERT`문, 데이터를 조회하기 위한 `SELECT`문, 데이터를 수정하기 위한 `UPDATE`문, 데이터를 삭제하기 위한 `DELETE`문 등이 있습니다.

## 2️⃣ 관계형 데이터베이스(Relational Database, RDB)의 구성 요소.

### 1️⃣ 테이블(Table)
- 관계형 데이터베이스(Relational Database, RDB)의 기본 단위입니다.
    - 데이터는 **행(Row)과 열(Column)로** 구성된 테이블에 저장됩니다.
        - 예: `사용자(Users)` 테이블, `주문(Order)` 테이블

### 2️⃣ 기본 키(Primary Key)
- 각 테이블에서 **고유하게 레코드를 식별**하는 열(Column)입니다.
    - 중복될 수 없으며, 각 행(Row)을 유일하게 구분할 수 있습니다.
- 예: 사용자 테이블의 `user_id` 열(Column)은 기본 키(Primary Key)로 사용될 수 있습니다.

### 3️⃣ 외래 키(Foreign Key)
- **다른 테이블의 기본 키(Primary Key)를 참조**하는 열(Column)로, 테이블 간의 **관계**를 정의합니다.
    - 외래 키(Foreign Key)는 두 테이블을 연결하는 역할을 하며, 데이터 간의 **일관성**을 유지하는 데 도움을 줍니다.
- 예: `주문(Orders)` 테이블에서 `user_id` 열이 `사용자(Users)` 테이블의 `user_id`를 참조하는 경우, 이를 외래키(Foreign Key)라고 합니다.

### 4️⃣ 속성(Attribute)
- 테이블의 **열(Column)을** 의미하며, 각 속성(Attribute)은 특정 유형의 데이터를 저장합니다.
    - 예를 들어, 사용자 테이블에서 `name`, `email`, `age`와 같은 속성이 있을 수 있습니다.

### 5️⃣ 레코드(Record)
- 테이블의 **행(Row)을** 의미하며, 하나의 레코드는 테이블에 저장된 **데이터의 한 항목**을 나타냅니다.
    - 예를 들어, 사용자 테이블의 한 행(Row)은 한 사용자의 정보를 나타냅니다.

## 3️⃣ 관계형 데이터베이스(Relational Database, RDB)의 예시.
- 예를 들어, **전자상거래 웹사이트**를 위한 데이터베이스를 설계한다고 가정해봅시다.
    - 여기에는 사용자와 주문 정보를 저장하기 위해 두 개의 테이블이 있을 수 있습니다.

### 1️⃣ 사용자(Users) 테이블.
- `user_id`(기본 키, Primary Key)
- `name``
- `email`
- `address`

### 2️⃣ 주문(Orders) 테이블.
- `order_id`(기본 키, Primary Key)
- `user_id`(외래 키, Foreign Key, 사용자와의 관계를 나타냄)
- `product`
- `quantity`

### 3️⃣ 설명.
- 이 예에서, **사용자 테이블과 주문 테이블**은 `user_id`를 통해 관계를 맺고 있습니다.
    - 이를 통해 특정 사용자가 어떤 주문을 했는지 쉽게 조회할 수 있습니다.

## 4️⃣ 관계형 데이터베이스 관리시스템(RDBMS, Relational Database Management System)의 예.
- **MySQL**
    - 오픈 소스 관계형 데이터베이스 관리시스템(RDBMS, Relational Database Management System)으로, 웹 애플리케이션에서 많이 사용됩니다.
- **PostgreSQL**
    - 오픈 소스 관계형 데이터베이스 관리시스템(RDBMS, Relational Database Management System)로, 확장성과 표준 준수에 중점을 둔 관계형 데이터베이스 관리시스템(RDBMS, Relational Database Management System)입니다.
- **Oracle**
    - 대규모 상업용 데이터베이스로, 높은 성능과 보안성을 자랑하는 관계형 데이터베이스 관리시스템(RDBMS, Relational Database Management System)입니다.
- **Microsoft SQL Server**
    - 마이크로소프트에서 개발한 관계형 데이터베이스 관리시스템(RDBMS, Relational Database Management System)으로, 기업 환경에서 많이 사용됩니다.

## 5️⃣ 관계형 데이터베이스(Relational Database, RDB)의 장점.

### 1️⃣ 데이터 무결성 보장.
- 관계형 데이터베이스(Relational Database, RDB)는 **제약 조건(Constraints)을** 통해 **데이터의 무결성을 보장합니다.**
    - 예를 들어, 외래 키(Foreign key)를 통해 테이블 간의 참조 무결성을 유지하고, 데이터의 일관성을 확보합니다.

### 2️⃣ SQL을 통한 데이터 관리.
- 관계형 데이터베이스(Relational Database)는 **SQL(Structured Query Language)이라는** 표준 언어를 사용하여 데이터를 **검색, 삽입, 수정, 삭제할** 수 있습니다.
    - SQL은 관계형 데이터베이스에서 데이터를 쉽게 관리할 수 있도록 해줍니다.

### 3️⃣ 데이터 중복 최소화.
- 관계형 데이터베이스(Relational Database, RDB)는 데이터를 여러 테이블로 나누고, **중복을 최소화**하여 저장합니다.
    - 이로 인해 데이터 저장소의 효율성이 증가하고, 데이터 일관성을 유지할 수 있습니다.

### 4️⃣ 데이터 보안.
- 관계형 데이터베이스(Relational Database, RDB)는 사용자 권한을 관리하여, 데이터에 대한 접근을 **제어**하고 **보안**을 강화할 수 있습니다.

## 6️⃣ 관계형 데이터베이스의 단점.

### 1️⃣ 복잡한 구조.
- 데이터가 여러 테이블에 분산되어 저장되기 때문에, 데이터의 **구조가 복잡**해질 수 있습니다.
    - 특히, 데이터간의 관계가 많아질수록 관리와 설계가 어려워질 수 있습니다.

### 2️⃣ 확장성의 한계.
- 관계형 데이터베이스(Relational Database, RDB)는 데이터의 **수평적 확장**(데이터를 여러 서버로 나누어 저장)이 어려운 경우가 많습니다.
    - 데이터가 크고 관계가 복잡할수록 확장성과 성능에 제약이 있을 수 있습니다.

### 3️⃣ 고정된 스키마.
- 관계형 데이터베이스(Relational Database, RDB)는 **고정된 스키마**를 사용합니다.
    - 즉, 테이블 구조(열(Row)의 수와 이름 등)가 정해진 후, 이를 변경하기 어렵습니다.
        - 데이터 구조가 자주 변경되는 경우 유연성이 떨어질 수 있습니다.

## 7️⃣ 관계형 데이터베이스(Relational Database, RDB)와 NoSQL 데이터베이스의 차이.

### 1️⃣ 관계형 데이터베이스(Relational Database, RDB)
- 데이터를 **테이블** 형태로 저장하며, 테이블 간의 **관계를** 정의합니다.
- **SQL(Structured Query Language)을** 사용하여 데이터를 관리합니다.
- **일관성**과 **무결성**을 중요하게 다루며, 고정된 스키마 구조를 가집니다.

### 2️⃣ NoSQL 데이터베이스
- 데이터를 **유연한 구조**로 저장하며, 문서(Document), 키-값(Key-Value), 그래프(Graph) 등 다양한 방식으로 데이터를 저장할 수 있습니다.
- **스키마가 유연**하여 데이터 구조가 자주 변경될 때 유리합니다.
- 관계형 데이터베이스(Relational Database, RDB)에 비해 **수평적 확장**이 용이하며, 대규모 분산 시스템에 적합합니다.

## 8️⃣ 결론.
- **관계형 데이터베이스(Relational Database, RDB)는** 데이터를 **테이블 형태**로 저장하고, 여러 테이블 간의 **관계**를 정의하여 데이터를 효율적으로 관리하는 **데이터베이스 관리 시스템**입니다.
- **SQL**을 사용해 데이터를 관리하며, **데이터 무결성과 일관성을 유지하는 데 강점을 가집니다.**
- 대표적인 관계형 데이터베이스로는 MySQL, PostgreSQL, Oracle 등이 있으며, 복잡한 관계를 가지는 정형 데이터 관리에 적합합니다.
