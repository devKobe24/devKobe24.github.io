---
title: "🐋[MySQL] null 컬럼 변경하기."
tags:
    - MySQL
date: "2024-02-18"
thumbnail: "/assets/img/thumbnail/mysql.jpeg"
---

# 테이블 null 컬럼 -> not null 으로 변경하기.

```mysql
alter table 테이블이름 mofify column 컬럼이름 컬럼타입 not null;
```

* 컬럼에 null 값 허용

# 테이블 null 컬럼 -> null 으로 변경하기.

```mysql
alter table 테이블이름 mofify column 컬럼이름 컬럼타입 null;
```

* 컬럼에 null 값 비허용
