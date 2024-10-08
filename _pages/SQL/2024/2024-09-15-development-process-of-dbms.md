---
title: "💉[SQL] DBMS의 발전 과정."
tags:
    - SQL
date: "2024-09-15"
thumbnail: "/assets/img/thumbnail/sql.jpeg"
---

# 💉[SQL] DBMS의 발전 과정.
- 컴퓨터가 존재하기 전부터 사람들은 데이터(정보)를 관리해 왔습니다.
- 종이에 정보를 기록하고 관리하던 때부터 시작해 지금의 DBMS까지 어떤 과정으로 발전했는지 차례대로 살펴봅시다.

## 1️⃣ 종이에 펜으로 기록.
- 아주 오래 전부터 정보는 관리되어 왔습니다.
- 컴퓨터가 없던 시기에도 구멍가게(요즘의 편의점과 비슷)를 운영하면서 판매와 구매가 발생했을 것이고, 그것을 종이에 펜으로 기록했을 것입니다.

## 2️⃣ 컴퓨터에 파일로 저장.
- 컴퓨터가 등장하고 일반 사람들도 컴퓨터를 사용하게 되면서 종이에 기록하던 내용을 컴퓨터 파일에 기록, 저장하게 되었습니다.
- 컴퓨터에 판매/구매 이력을 저장하는 방법은 단순하게 메모장을 사용할 수도 있지만, 컴퓨터를 어느 정도 활용하게 되면서 엑셀과 같은 스프레드시트 프로그램을 사용해 표 형태로 내용을 기록하고 자동으로 계산하는 등 한층 더 효율적으로 정보를 관리하게 되었습니다.
- 기록된 내용은 **파일(file)** 이라는 형태로 저장해 필요할 때마다 열어서 사용할 수 있습니다.
- 엑셀을 사용하면 아주 편리하지만, 저장한 파일은 한 번에 한 명의 사용자만 열어서 작업할 수 있습니다.
- 규모가 작은 구멍가게에서는 한 명의 사용자가 하나의 파일에 작업하는 것이 문제가 되지 않을 수도 있습니다.
    - 하지만 규모가 큰 슈퍼마켓이나 마트 등에서는 데이터의 양이 많아 한 명의 사용자가 모두 처리할 수 없기 때문에 여러 명이 각자의 파일을 만들어서 작업할 수밖에 없습니다.
### 예시.
- 예를 들어, 3명의 직원이 엑셀로 판매 내용을 기록한다고 합시다.
    - A 직원은 오전, B 직원은 오후, C 직원은 야간에 판매된 내용을 기록한다고 가정하겠습니다.
    - 3명이 정확히 자신의 시간에 판매된 것만 기록하면 좋겠으나, 실수로 A 직원이 판매한 내역을 B 직원 파일에 작성할 수도 있을 것입니다.
        - 또, 오전에 판매한 물건을 오후에 반품할 경우에는 오전에 판매한 사람이 기록해야 할지, 오후에 반품받은 사람이 기록해야 할지 그 주체도 모호하기 때문에 기록이 누락되거나 모두 기록하여 중복되는 문제가 발생할 소지도 있습니다.
            - 하루, 한 달 더 나아가서는 연간 판매 기록을 합계할 때 금액이 맞지 않는 경우처럼 심각한 일이 발생할 수도 있습니다.
                - 이러한 불일치가 파일의 큰 문제점 중 하나입니다.
                    - 하지만 이런 문제점에도 불구하고 파일은 한 명이 처리하거나 소량의 데이터를 처리할 때는 속도가 빠르고, 사용법이 쉽기 때문에 지금도 많이 사용하고 있습니다.

## 3️⃣ DBMS의 대두와 보급.
- 앞에서 언급한 파일의 단점을 보완하면서 대량의 데이터를 효율적으로 관리하고 운영하기 위해서 등장한 것이 **DBMS** 입니다.
- MySQL과 같은 DBMS의 개념은 1973년에 최초로 에드거 프랭크 커드(E.F. Codd)라는 학자가 이론을 정립했습니다.
    - 그 이후로 많은 DBMS 제품이 만들어졌고, 지금과 같이 안정적인 소프트웨어로 자리 잡게 되었습니다.
- DBMS는 데이터의 집합인 **데이터베이스** 를 잘 관리하고 운영하기 위한 시스템 또는 소프트웨어를 말합니다.
- DBMS에 데이터를 구축, 관리하고 활용하기 위해서 사용되는 언어가 **SQL(Structured Query Language)** 입니다.
    - 이 SQL을 사용하면 DBMS를 통해 중요한 정보들을 입력, 관리하고 추출할 수 있습니다.
        - 즉, SQL 문을 잘 이해하고 사용해야만 DBMS를 원활하게 활용할 수 있습니다.
            - 비유하자면 미국 문화(DBMS)를 완전히 이해하고 싶다면 그 나라의 언어인 영어(SQL)를 먼저 배워야 하는 것과 비슷한 개념입니다.

<img src = "https://github.com/devKobe24/images2/blob/main/sql-img/sql-dbms.png?raw=true">
