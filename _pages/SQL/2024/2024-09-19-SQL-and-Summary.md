---
title: "💉[SQL] DBMS에서 사용되는 언어와 마무리"
tags:
    - SQL
date: "2024-09-19"
thumbnail: "/assets/img/thumbnail/sql.jpeg"
---

# 💉[SQL] DBMS에서 사용되는 언어: SQL.

## 1️⃣ SQL.
- **SQL(Structured Query Language)** 은 관계형 데이터베이스에서 사용되는 언어입니다.
- 관계형 DBMS(그중 MySQL)를 배우려면 SQL을 필수로 익혀야 합니다.
- SQL이 데이터베이스를 조작하는 '언어'이긴 하지만 일반적인 프로그래밍 언어(C, Java Python 등)와는 조금 다른 특성을 갖습니다.
---
- SQL은 특정 회사에서 만드는 것이 아니라 국제표준화기구에서 SQL에 대한 표준을 정해서 발표하고 있습니다.
    - 이를 **표준 SQL** 이라고 합니다.
- 그런데 문제는 SQL을 사용하는 DBMS를 만드는 회사가 여러 곳이기 때문에 표준 SQL이 각 회사 제품의 특성을 모두 포용하지 못한다는 점입니다.
- 그래서 DBMS를 만드는 회사에서는 되도록 표준 SQL을 준수하되, 각 제품의 특성을 반영한 SQL을 사용합니다.
---
- 다음 그림을 보면 3가지 DBMS 제품(오라클, SQL 서버, MySQL)이 모두 표준 SQL을 포함하고 있지만, 추가로 자신만의 기능도 가지고 있습니다.

<img src = "https://github.com/devKobe24/images2/blob/main/sql-img/sql-standard-sql.png?raw=true">

- 이렇게 변경된 SQL을 오라클은 PL/SQL, SQL 서버는 T-SQL, MySQL은 SQL로 부릅니다.
- 결론은 표준 SQL을 익히면 여러 DBMS의 공통적인 부분을 배우는 것입니다.

---

## 2️⃣ 마무리.

### 4가지 키워드로 끝내는 핵심 포인트.
- **데이터베이스** 는 데이터의 집합입니다..
- **DBMS** 는 데이터베이스를 운영/관리하는 프로그램입니다.
- **테이블** 은 데이터베이스의 최소 단위로, 하나 이상의 열(Column)과 행(Row)으로 구성되어 있습니다.
- **SQL** 은 데이터베이스를 구축, 관리하고 활용하기 위해서 사용되는 언어입니다.
