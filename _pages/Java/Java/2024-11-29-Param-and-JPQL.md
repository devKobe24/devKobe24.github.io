---
title: "☕️[Java] `@Param`과 JPQL의 관계."
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-11-29"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] `@Param`과 JPQL의 관계.
- 우선 아래의 코드를 예시로 들어 이 포스트를 작성하도록 하겠습니다.

```java
public interface PersonRepository extends JpaRepository<Person, Long> {
    
    @Query("SELECT p FROM Person p WHERE p.firstName LIKE %:name% OR p.lastName LIKE %:name%")
    List<Person> findByNameAsEnglish(@Param("name") String name);
}
```

## 1️⃣ `@Param("name")`
### 1️⃣ 역할 및 기능.
#### 1️⃣ 파라미터 이름을 지정.
- `@Param("name")`은 JPQL(혹은 네이티브 쿼리)에서 사용하는 **파라미터 이름**을 지정하는 역할을 합니다.
- JPQL 쿼리 내에서 `:name`으로 표시된 **바인딩 변수**와 메서드 매개변수 `name`을 연결합니다.

#### 2️⃣ 쿼리에서의 사용.
- 쿼리 내의 `:name`은 런타임 시 `findByNameAsEnglish` 메서드에 전달된 `name` 값으로 바뀌어 실행됩니다.
    - 예를 들어, `findByNameAsEnglish("Jhon")`을 호출하면 , `:name`은 `"Jhon"`으로 대체됩니다.

#### 3️⃣ 바인딩된 매개변수 전달.
- JPQL이나 네이티브 쿼리에서 명시적으로 바인딩된 변수를 사용하려면 `@Param`을 통해 매핑을 명확히 해야 합니다.

### 2️⃣ 동작 과정.
#### 1️⃣ JPQL 정의.
```java
@Query("SELECT p FROM Person p WHERE p.firstName LIKE %:name% OR p.lastName LIKE %:name%")
```
- JPQL 쿼리에서 `:name`은 파리미터 **플레이스 홀더**로 사용됩니다.

#### 2️⃣ 메서드 호출.
```java
personRepository.findByNameAsEnglish("Jhon");
```
- 메서드를 호출하면서 "Jhon"을 name에 전달합니다.

#### 3️⃣ 매핑 및 실행.
- `@Param("name")`이 쿼리의 `:name`과 메서드 파라미터 `name`을 매핑하여 `SQL`로 변환됩니다.
```sql
SELECT p FROM Person p WHERE p.firstName LIKE '%John%' OR p.lastName LIKE '%John%'
```

## 2️⃣ 왜 `@Param`을 사용해야 할 때가 있을까요?
### 1️⃣ 명시적 매핑.
- 메서드 파라미터 이름이 쿼리에서 사용되는 이름과 다를 경우 명시적으로 연결할 필요가 있습니다.

### 2️⃣ 혼동 방지.
- 쿼리에 파라미터가 여러 개 있을 때, 각각을 명확히 매핑하기 위해 사용합니다.

### 3️⃣ 예제.
- 다음과 같이 매개변수 이름이 다를 때, `@Param`이 반드시 필요합니다.
```java
@Query("SELECT p FROM Person p WHERE p.firstName LIKE %:searchName%")
List<Person> findByNameCustom(@Param("searchName") String name);
```
- 위 코드에서 `name`이라는 메서드 파라미터를 `searchName`이라는 쿼리 파라미터로 매핑합니다.

## 3️⃣ 결론.
- `@Param`은 JPQL(혹은 네이티브 쿼리)에서 사용되는 **파라미터 이름**과 **메서드 매개변수를** 매핑해주는 역할을 합니다.
    - **매개변수 이름과 쿼리 파라미터가 동일하면 생략이 가능하지만, 명시적으로 작성하면 코드의 가독성과 유지보수성이 좋아집니다.**
