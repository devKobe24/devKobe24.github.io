---
title: "💉[SQL] REPLACE, SUBSTRING, CONCAT"
tags:
    - SQL
date: "2024-04-04"
thumbnail: "/assets/img/thumbnail/sql.jpeg"
---

# REPLACE
**'REPLACE'** 함수는 SQL에서 문자열 내의 특정 부분을 다른 문자열로 바꾸고자 할 때 사용됩니다.
- 이 함수는 데이터 정제나 수정 작업에서 특히 유용하며, 기존 문자열 내의 특정 패턴이나 문자를 찾아 이를 새로운 문자열로 대체하는 기능을 제공합니다.
- **'REPLACE'** 는 로그 데이터 정리, 사용자 입력 데이터의 표준화, 데이터 마이그레이션 작업 등 다양한 상황에서 활용될 수 있습니다.

## 'REPLACE' 사용 예
- **특정 문자열 대체:** 고객 데이터에서 전화번호 형식을 변경하고 싶을 때
```sql
SELECT REPLACE(phone_number, '-', '') FROM customers;
```
이 쿼리는 **'customers'** 테이블의 **'phone_number'** 열에서 모든 **'-'** 를 제거합니다.
예를 들어, **'123-456-7890'** 이라는 전화번호가 있을 경우, **'1234567890'** 으로 변경됩니다.

- **데이터 정제:** 사용자의 이메일 주소에서 도메인을 변경하고 싶을 때
```sql
UPDATE users SET email = REPLACE(email, '@old_domain.com', '@new_domain.com');
```
이 쿼리는 **'users'** 테이블의 **'email'** 열에서 **'@old_domail.com'** 을 **'@new_domain.com'** 으로 변경합니다.

- **텍스트 내용 수정:** 상품 설명에서 특정 단어를 새로운 단어로 바꾸고 싶을 때
```sql
UPDATE products SET description = REPLACE(description, 'oldword', 'newword');
```
이 쿼리는 **'products'** 테이블의 **'description'** 열에서 **'oldword'** 를 **'newword'** 로 변경합니다.

## 'REPLACE' 함수의 특징.
- **'REPLACE'** 는 대소문자를 구분하여 작동합니다.
    - 대소문자 구분 없이 대체를 하고자 할 경우, 추가적인 함수나 조건을 사용해야 할 수 있습니다.
- 문자열 내에서 지정된 패턴이나 문자열을 찾아 모두 대체합니다.
    - 찾고자 하는 문자열이 존재하지 않으면, 원본 문자열이 변경 없이 그대로 반환됩니다.
- **'REPLACE'** 함수는 **'SELECT'**, **'UPDATE'** 등의 쿼리 내에서 사용할 수 있으며, 데이터 조회 또는 수정 작업에 모두 적용할 수 있습니다.

## 사용 시 고려사항
- 대량의 데이터를 처리할 때는 **'REPLACE'** 함수를 사용하는 쿼리의 성능에 주의해야 합니다.
    - 특히 **'UPDATE'** 작업에서는 대체 작업으로 인해 대량의 데이터가 변경될 수 있으므로, 사전에 작업 범위를 잘 파악하고 필요한 백업을 수행하는 것이 좋습니다.
- 문자열 대체 작업을 수행할 때는 원치 않는 데이터 변경을 방지하기 위해, 대체할 문자열이 정확히 일치하는지 사전에 확인하는 것이 중요합니다.

**'REPLACE'** 함수는 문자열 데이터를 쉽게 수정하고 정제할 수 있는 강력한 도구로, 데이터베이스 내의 데이터 관리 및 유지보수 작업에 널리 사용됩니다.

# SUBSTRING
**'SUBSTRING'** 함수는 SQL에서 문자열의 특정 부분을 추출할 때 사용됩니다.
- 이 함수는 문자열 데이터 내에서 특정 위치를 기준으로 한 부분 문자열(substring)을 반환하며, 데이터 정제, 특정 형식의 데이터 추출, 또는 문자열 처리 작업에서 매우 유용합니다.
- **'SUBSTRING'** 은 로그 분석, 데이터 마이그레이션, 사용자 입력의 특정 부분 처리 등 다양한 상황에서 활용될 수 있습니다.

## 'SUBSTRING' 사용 예
- **특정 위치의 문자열 추출:** 사용자 이메일에서 도메인 부분만을 추출하고 싶을 때
```sql
SELECT SUBSTRING(email FROM POSITION ('@' IN email) + 1) FROM users;
```
이 쿼리는 **'users'** 테이블의 **'email'** 열에서 **'@'** 기호 뒤의 도메인 부분을 추출합니다.

- **고정된 형식의 문자열 처리:** 전화번호에서 지역 코드를 추출하고 싶을 때
```sql
SELECT SUBSTRING(phone_number, 1, 3) FROM customers;
```
이 쿼리는 **'customers'** 테이블의 **'phone_number'** 열에서 처음 3자리(지역 코드)를 추출합니다.

- **문자열의 특정 부분 수정 작업에 사용:** 주소에서 특정 부분을 다른 형식으로 변경하고 싶을 때
```sql
UPDATE addresses SET street = SUBSTRING(street, 1, 10) || '...' WHERE LENGTH(street) > 10;
```
이 쿼리는 **'addresses'** 테이블의 **'street'** 열에서 문자열의 길이가 10자를 초과하는 경우, 처음 10자만을 남기고 그 뒤를 **'...'** 으로 대체합니다.

## 'SUBSTRING' 함수의 특징
- **'SUBSTRING'** 은 문자열의 특정 섹션을 반환하는 데 사용되며, 시작 위치와 길이(선택적)를 지정하여 원하는 부분 문자열을 추출할 수 있습니다.
- 다양한 문자열 처리 작업에 활용될 수 있으며, 데이터의 형식을 변경하거나, 특정 패턴에 기반한 정보를 추출하는 등의 목적으로 사용됩니다.
- 함수의 정확한 구문은 사용하는 SQL 데이터베이스 시스템에 따라 약간씩 다를 수 있으므로, 해당 시스템의 문서를 참조하는 것이 좋습니다.

## 사용 시 고려사항
- **'SUBSTRING'** 함수를 사용할 때는 문자열의 인덱스가 1부터 시작한다는 점을 주의해야 합니다.(대부분의 SQL 시스템에서).
- 대량의 데이터를 처리할 때는 **'SUBSTRING'** 함수를 사용하는 쿼리의 성능에 주의해야 합니다. 필요한 경우, 적절한 인덱스 사용과 데이터 필터링을 통해 성능을 최적화할 수 있습니다.

**'SUBSTRING'** 함수는 문자열 데이터를 효과적으로 처리하고 분석하는 데 있어 필수적인 도구로, 데이터베이스 내에서 다양한 문자열 조작 작업을 수행하는 데 널리 사용됩니다.

# CONCAT
**'CONCAT'** 함수는 SQL에서 두 개 이상의 문자열을 하나로 결합할 때 사용됩니다.
- 이 함수는 데이터베이스 내에서 다양한 문자열 정보를 합쳐 새로운 문자열 값을 생성하고자 할 때 유용하며, 보고서 작성, 데이터 형식의 표준화, 사용자 이름이나 주소와 같은 데이터의 결합 등 다양한 상황에서 활용될 수 있습니다.

## 'CONCAT' 사용 예
- **단순한 문자열 결합:** 사용자의 이름과 성을 하나의 문자열로 결합하고 싶을 때
```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM users;
```
이 쿼리는 **'users'** 테이블의 **'first_name'** 과 **'last_name'** 을 공백으로 구분하여 결합한 후, **'full_name'** 이라는 새로운 열로 결과를 반환합니다.

- **복수의 열 결합:** 고객의 주소 정보를 하나의 문자열로 결합하고 싶을 때
```sql
SELECT CONCAT(street_address, ', ', city, state, ' ', postal_code) AS full_address FROM customers;
```
이 쿼리는 **'customers'** 테이블에서 여러 주소 관련 열을 콤마와 공백을 사용하여 결합하고, 이를 **'full_address'** 라는 새로운 열로 결과를 반환합니다.

- **데이터 형식 표준화:** 상품 코드와 상품 이름을 결합하여 표준 형식의 상품 정보를 생성하고 싶을 떄
```sql
SELECT CONCAT(product_code, ': ', product_name) AS product_info FROM products;
```
이 쿼리는 **'products'** 테이블의 **'product_code'** 와 **'product_name'** 을 콜론과 공백으로 구분하여 결합한 후,
**'product_info'** 라는 새로운 열로 결과를 반환합니다.

## 'CONCAT' 함수의 특징
- **'CONCAT'** 함수는 두 개 이상의 문자열을 매개변수로 받아 이들을 순서대로 결합한 새로운 문자열을 생성합니다.
- 거의 모든 SQL 데이터베이스 시스템에서 지원되며, 문자열 처리와 데이터 형식의 변환에 널리 사용됩니다.
- 일부 데이터베이스 시스템에서는 **'CONCAT'** 대신 연산자(**'||'** 등)를 사용하여 문자열을 결합할 수도 있습니다.

## 사용 시 고려사항
- 결합하려는 문자열 중 하나라도 **'NULL'** 값을 포함하는 경우, **'CONCAT'** 의 동작은 데이터베이스 시스템에 따라 다를 수 있습니다.
    - 예를 들어, 일부 시스템은 **'NULL'** 을 빈 문자열로 취급할 수 있으나, 다른 시스템에서는 전체 결과가 **'NULL'** 이 될 수 있습니다.
- 복잡한 문자열 결합을 수행할 때는 성능에 주의해야 하며, 특히 대량의 데이터를 처리할 때는 쿼리 성능을 테스트하고 최적화하는 것이 중요합니다.

**'CONCAT'** 함수는 문자열 데이터를 결합하고 조작하는 과정에서 필수적인 도구로, 데이터베이스 내에서 다양한 문자열 관련 작업을 구행하는 데 활용됩니다.
