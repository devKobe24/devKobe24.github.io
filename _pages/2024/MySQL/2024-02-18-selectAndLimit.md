---
title: "🐋[MySQL] SELECT 조회 결과 LIMIT 1000 ROW 해제하기."
tags:
    - MySQL
date: "2024-02-18"
thumbnail: "/assets/img/thumbnail/mysql.jpeg"
---

# MySQL에서 SELECT의 조회 결과가 1000개만 출력이 되는 이유.

처음에는 MySQL로 for 문을 만들 수 있는지 호기심에서 시작했습니다.(for문에 대한 포스팅은 따로 올리겠습니다.)

그래서 다음과 같이 코드를 만들고 실행했습니다.

```sql
delimiter $$ 
create procedure loopInsert()
begin
	declare i int;
	set i = 1;

	while (i <= 2000) do
		insert into test_table (id) values(i);
    
		set i = i + 1;
	end while;
end $$

call loopInsert(); --2000개의 id가 만들어짐.
```

저의 예상은 2000개의 row(필드)가 만들어져 id가 1~2000이 되어 Result 값에 보여질 줄 알았으나 id의 갯수를 세어주는 SQL 문을 만들어 실행 시켜보니 1000 이라는 결과값이 나왔습니다.

```sql
SELECT COUNT(id) FROM test_table; -- 결과값 count(id) 1000
```

그래서 확인을 해보려 다음과 같은 쿼리문을 만들어 실행해봤습니다.

```sql
SELECT * FROM test_table; 
-- Action select * from test_table LIMIT 0, 1000
-- 결과값 1~999, Response 1000 row(s) returned
```
* 여기서 주목할 점은 **"Action select * from test_table LIMIT 0, 1000"** 입니다.
    * 저는 LIMIT 키워드를 발견했고 이 LIMIT을 해제하는 방법을 찾아보았습니다.

## LIMIT 해제 방법1 : 직접 LIMIT 제한 범위 걸어주기

직접적인 해결 방법은 아니지만,
쿼리 자체가 'LIMIT 1000'으로 고정되기 때문에 쿼리에 LIMIT를 넣어 LIMIT 제한을 늘려주는 방법입니다.
* 이 방식은 권장하는 방식은 아니라고 합니다. 이런 방식으로 조회할 수 있다는 방법 중 하나를 제시한 것 뿐이라는 블로그의 글을 보았습니다.
    * 아래와 같이 LIMIT 키워드를 쿼리 내에 직접 명시하여 범위를 제공해주는 방법입니다.
```sql
select * from table_test LIMIT 99999999;
```

## LIMIT 해제 방법2 : Workbench 설정 Limit Rows 옵션 해제하기

MySQL에서의 Workbench에는 SELECT 쿼리 조회 결과를 자체적으로 행을 제한해서 보여주는 옵션이 있습니다 이게 기본 값 1000행으로 되있는데, 이 옵션을 변경 또는 해제하면 됩니다.

* settings > SQL Execution > SELECT Query Results > Limit Rows 해제
    * 혹은 Limit Rows Count 행 수 변경.

## Limit Rows 옵션의 장.단점.

* 장점
    * 테이블에 속해 있는 데이터의 수를 정확하게 확인할 수 있습니다.
* 단점
    * 무분별한 전체 조회는 데이터베이스의 과부하에 영향을 미칠 수 있습니다.

결론적으로 쿼리를 작성하고 실행하는 개발자가 상황에 맞게 잘 사용해야 합니다.

즉, 최적화와 DB 성능에 영향에 대해 잘 고민해보고 쿼리를 사용해야 합니다.

* 예시 1) 테이블 구조 및 데이터 확인 용도.
```sql
SELECT * FROM 테이블이름 LIMIT 5;
```

* 예시 2) 조건에 맞는 특정 데이터 확인 용도.
```sql
SELECT 컬럼명 FROM 테이블명 WHERE 조건입력 LIMIT 5;
```
