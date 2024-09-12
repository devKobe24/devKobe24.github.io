---
title: "🐋[MySQL] 테이블에 데이터 입력 INSERT INTO"
tags:
    - MySQL
date: "2024-02-18"
thumbnail: "/assets/img/thumbnail/mysql.jpeg"
---

# INSERT INTO

`memberTBL`이라는 테이블이 있습니다.

그 테이블에는 아무런 데이터가 입력되지 않은 상태입니다.

열(Column, 필드)은 총 3개입니다.
* memberID
    * char(8)
* memberName
    * char(5)
* memberAddress
    * char(20)

데이터를 한 행(row)을 삽입하려고 할 경우에는 다음과 같이하면 됩니다.
```
INSERT INTO 테이블이름 VALUES (데이터1, 데이터2, 데이터3...);
```

만약 `memberTBL`에 한 행(row)을 삽입하려 할 경우는 다음과 같이하면 됩니다.
```
INSERT INTO memberTBL VALUES ('Thomas', '토마스', '경기 부천시 중동');
```

만약 특정 필드에만 값을 입력하고 싶을 경우에는 `VALUES` 앞에 "입력할 필드를 선언해 줍니다."
```
INSERT INTO memberTBL (memberID, memberAddress) VALUES ('Thomas', '경기 부천시 중동');
```

만약 여러 개의 행(row)을 동시에 입력하고 싶을 경우에는 아래와 같이 여러 개의 행을 동시에 입력이 가능합니다.
```
INSERT INTO memberTBL (memberID, memberName, memberAddress) VALUES  ('Thomas', '토마스', '경기 부천시 중동'), ('Edward', '에드워드', '서울 은평구 증산동'), ('Henrv', '헨리', '인천 남구 주안동'), ('Gorden', '고든', '경기 성남시 분당구');
```
