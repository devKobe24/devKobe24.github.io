---
title: 🍃[Spring] Spring Data JPA를 이용해 다양한 쿼리를 작성해보자.
tags:
    - Spring
    - Framework
date: "2024-11-08"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Spring Data JPA를 이용해 다양한 쿼리를 작성해보자.

## 1️⃣ findByName

- 삭제 기능을 **Spring Data JPA**로 만들어 보기로 했다!

```java
public class UserService {
    
    private final UserRepository userRepository;
    
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    // 삭제 기능.
    public void deleteUser(String name) {
        // SELECT * FROM user WHRER name = ?
        userRepository.findByName(name) // <- 이 코드는 빨간 글씨로 표시됨.
    }
}
```

- `findByName()`이 빨간 글씨로 표시되는 이유는 메서드가 정의되지 않았기 때문입니다.
    - 그렇다면 어떻게 해야할까요?
        - `UserRepository` 인터페이스 내부에 "함수"를 정의해 주면 됩니다.

```java
public interface UserRepository extends JpaRepository<User, Long> {
    
    User findByName(String name);
}
```

- `User findByName(String name);`
    - `findByName` 함수는 `User`를 반환하며, `name`을 parameter(매개변수)로 받습니다.
        - 이렇게 정의하고 나서 다시 `UserService`에서 사용해주면 됩니다.

```java
public class UserService {
    
    private final UserRepository userRepository;
    
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    // 삭제 기능.
    public void deleteUser(String name) {
        // SELECT * FROM user WHRER name = ?
        User user = userRepository.findByName(name);
        if (user == null) {
            throw new IllegalArgumentException();
        }
        
        userRepository.delete(user);
    }
}
```

- `findByName(String name)`은 `User`를 반환합니다.
    - `findByName` 함수에서 `User`가 있다면 `User`를 반환하고 없다면 `Null`을 반환합니다.
- 원래라면 jdbcRepository를 사용했더라면 `DELETE FROM user WHERE name = ?`; 이 쿼리를 직접 날려줬어야 합니다.
    - 하지만 **Spring Data JPA**에서는 그럴 필요 없이 `delete()`를 사용할 수 있습니다.


## 2️⃣ Spring Data JPA에서 메서드 이름을 통해 쿼리를 자동으로 생성하기.
- **Spring Data JPA**에서는 **이름을 통해 쿼리를 자동으로 생성할 수 있습니다.**
    - 이 기능을 활용하기 위해서는 메서드 이름을 특정한 규칙에 따라 작성해야 합니다.
    - Spring Data JPA는 **메서드 이름을 파싱하여 쿼리로 변환하므로, 올바른 규칙을 따르는 것이 중요합니다.**

### 1️⃣ Spring Data JPA에서 메서드 이름을 작성할 때의 주요 규칙.

#### 1️⃣ 메서드 이름의 기본 구조.
- 메서드 이름은 보통 findBy, readBy, queryBy, countBy 등의 **접두사**로 시작합니다.
    - 그 뒤에 조건으로 사용할 필드 이름을 CamelCase 방식으로 추가합니다.

#### 👉 기본 구조.
```
[접두사][엔티티의 필드 이름][조건식]
```

### 👉 예시.
```java
List<User> findByEmail(String email);
List<User> findByNameAndAge(String name, int age);
```

#### 2️⃣ 접두사.
- 메서드 이름은 보통 다음과 같은 접두사로 시작합니다.
    - **findBy :** 특정 조건에 맞는 엔티티를 조회합니다.
    - **readBy :** findBy와 비슷한 역할로 데이터를 조회할 때 사용합니다.
    - **queryBy :** 데이터 조회 시 사용.
    - **countBy :** 특정 조건에 맞는 엔티티 개수를 조회합니다.
    - **existsBy :** 특정 조건에 맞는 엔티티가 존재하는지 여부를 확인합니다.
    - **deleteBy :** 특정 조건에 맞는 엔티티를 삭제합니다.

#### 3️⃣ 조건 연결 키워드.
- 여러 필드를 조건으로 사용할 때는 다음과 같은 키워드를 사용하여 조건을 연결합니다.
    - **And :** 두 조건을 모두 만족하는 경우를 조회합니다.
    - **Or :** 두 조건 중 하나라도 만족하는 경우를 조회합니다.

#### 👉 예시
```java
List<User> findByNameAndAge(String name, int age);
List<User> findByNameOrEmail(String name, String email);
```

#### 4️⃣ 연산자 키워드.
- Spring Data JPA는 다양한 연산지 키워드를 지원하여 필드 값에 조건을 부여할 수 있습니다.
    - **Comparison(비교)**
        - **IsNull, IsNotNull :** 값이 null인지 아닌지를 체크합니다.
        - **IsTrue, IsFalse :** Boolean 값이 true 또는 false인지 체크합니다.
        - **LessThan, LessThanEqual :** 값이 지정된 값보다 작거나, 작거나 같은 경우를 조회합니다.
        - **GreaterThan, GreaterThanEqual :** 값이 지정된 값보다 크거나, 크거나 같은 경우를 조회합니다.
        - **Between :** 두 값 사이에 있는 경우를 조회합니다.
    - **String Operations(문자열 연산)**
        - **Like :** SQL의 LIKE와 같이 특정 문자열 패턴이 포함된 경우를 조회합니다.
        - **StartingWith :** 특정 문자열로 시작하는 경우를 조회합니다.
        - **EndingWith :** 특정 문자열로 끝나는 경우를 조회합니다.
        - **Containing :** 특정 문자열을 포함하는 경우를 조회합니다.
    - **Collection Operations(컬렉션 연산)**
        - **In :** 주어진 컬렉션에 값이 포함되는 경우를 조회합니다.
        - **NotIn :** 주어진 컬렉션에 값이 포함되지 않는 경우를 조회합니다.

#### 👉 예시
```java
List<User> findByAgeGreaterThan(int age);
List<User> findByNameStartingWith(String prefix);
List<User> findByEmailContaining(String keyword);
List<User> findByIdIn(List<Long> ids);
```

#### 5️⃣ 정렬하기
- 정렬이 필요한 경우 **OrderBy** 키워드를 사용하여 필드 이름과 방향(Asc 또는 Desc)을 지정합니다.

#### 👉 예시
```java
List<User> findByNameOrderByAgeAsc(String name);
List<User> findByNameOrderByAgeDesc(String name);
```

#### 6️⃣ 페이징 및 정렬 파라미터
- Spring Data JPA는 Pageable 및 Sort 파라미터를 지원하여, 메서드 이름에 OrderBy를 추가하지 않고도 정렬과 페이징을 적용할 수 있습니다.

#### 👉 예시
```java
Page<User> findByAgeGreaterThan(int age, Pageable pageable);
List<User> findByName(String name, Sort sort);
```

#### 7️⃣ 복잡한 쿼리 작성 예시
- 규칙을 적용하여 다양한 조건을 포함한 메서드를 작성할 수 있습니다.
```java
List<User> findByNameAndAgeGreaterThan(String name, int age);
List<User> findByEmailContainingAndIsActiveTrue(String keyword);
List<User> findByCreatedAtBetween(Date startDate, Date endDate);
List<User> findByLastNamesStartingWithAndAgeLessThanOrderByAgeDesc(String prefix, int age);
```

## 3️⃣ 요약.
- 1. **접두사 :** findBy, countBy, existsBy, deleteBy 등을 사용.
- 2. **조건 연결 :** And, Or을 사용하여 여러 조건을 연결.
- 3. **연산자 :** IsNull, IsTrue, LessThan, GreaterThan, Containing, Like, Between 등을 사용하여 필드에 다양한 조건 적용.
- 4. **정렬 :** OrderBy와 Asc 또는 Desc를 사용하여 정렬 조건을 추가.
- 5. **페이징과 정렬 파라미터 :** Pageable과 Sort 파라미터를 사용하여 페이징 및 정렬 가능.
