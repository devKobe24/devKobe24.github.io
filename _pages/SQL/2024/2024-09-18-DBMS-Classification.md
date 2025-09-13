---
title: "💉[SQL] DBMS의 분류."
tags:
    - SQL
date: "2024-09-18"
thumbnail: "/assets/img/thumbnail/sql.jpeg"
---

# 💉[SQL] DBMS의 분류.
- DBMS의 유형은 계층형(Hierarchical), 망형(Network), 관계형(Relational), 객체지향형(Object-Oriented), 객체관계형(Object-Relational) 등으로 분류됩니다.
- 현재 사용되는 DBMS 중에는 **관계형 DBMS(Relational DBMS)** 가 가장 많은 부분을 차지하며, **MySQL** 도 관계형 DBMS(Relational DBMS)에 포함됩니다.


## 1️⃣ 계층형 DBMS.
- **계층형 DBMS(Hierarchical DBMS)** 는 처음으로 등장한 DBMS 개념으로 1960년대에 시작되었습니다.
- 다음 그림과 같이 각 계층은 **트리(tree)** 형태를 갖습니다.

<img src = "https://github.com/devKobe24/images2/blob/main/sql-img/sql-tree-hierarchical-dbms.png?raw=true">

- 계층형 DBMS의 문제는 처음 구성을 완료한 후에 이를 변경하기가 상당히 까다롭다는 것입니다.
- 또한, 다른 구성원을 찾아가는 것이 비효율적입니다.
    - 예를 들어 재무 2팀에서 회계팀으로 연결하려면 재무이사 -> 사장 -> 회계이사 -> 회계팀과 같이 여러 단계를 거쳐야 합니다.
- 지금은 사용하지 않는 형태입니다.

## 2️⃣ 망형 DBMS.
- **망형 DBMS(Network DBMS)** 는 계층형 DBMS의 문제점을 개선하기 위해 1970년대에 등장했습니다.
- 다음 그림을 보면 하위에 있는 구성원끼리도 연결된 유연한 구조입니다.

<img src = "https://github.com/devKobe24/images2/blob/main/sql-img/sql-tree-network-dbms.png?raw=true">

- 예를 들어 재무 2팀에서 바로 회계팀으로 연결이 가능합니다.
- 하지만 망형 DBMS를 잘 활용하려면 프로그래머가 모든 구조를 이해해야만 프로그램 작성이 가능하다는 단점이 존재합니다.
- 지금은 거의 사용하지 않는 형태입니다.

## 3️⃣ 관계형 DBMS
- **관계형 DBMS(Relational DBMS)** 는 줄여서 **RDBMS** 라고 부릅니다.
- MySQL 뿐만 아니라, 대부분의 DBMS가 RDBMS 형태로 사용됩니다.
- RDBMS의 데이터베이스는 **테이블(table)** 이라는 최소 단위로 구성되며, 이 테이블은 하나 이상의 **열(Column)** 과 **행(Row)** 으로 이루어져있습니다.
- 아래의 표 모양이 바로 테이블입니다.
    - 친구의 카카오톡 아이디, 이름, 연락처 등 3가지 정보를 표, 즉 테이블로 만들면 다음과 같습니다.

<img src = "https://github.com/devKobe24/images2/blob/main/sql-img/sql-RDBMS.png?raw=true">

- **RDBMS** 에서는 모든 데이터가 테이블에 저장됩니다.
- 이 구조가 가장 기본적이고 중요한 구성이기 때문에 테이블만 제대로 파악하면 RDBMS를 어느 정도 이해했다고 알 수 있습니다.
- 테이블은 열과 행으로 이루어진 2차원 구조를 갖습니다.
    - 세로는 열(Column)이라 하고, 가로는 행(Row)이라고 합니다.
        - 열은 아이디, 이름, 연락처로 이름을 가지고 있고, 행은 각각의 정보로 이루어져 있습니다.
