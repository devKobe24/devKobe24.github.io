---
title: 🍃[Spring] Hibernate와 JDBC는 어떤 관계인가요?
tags:
    - Spring
    - Framework
date: "2024-10-15"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Hibernate와 JDBC는 어떤 관계인가요?
- **Hibernate와 JDBC(Java Database Connectivity)는** 모두 자바에서 데이터베이스와 상호작용하는 방식이지만, 서로 다른 수준에서 작동하는 도구입니다.
- **Hibernate는 JDBC(Java Database Connectivity)를 내부적으로 사용하여 데이터베이스와의 연결을 관리하고 쿼리를 실행**하지만, 그 역할과 목적이 다릅니다.
    - 이 둘의 관계와 차이점을 이해하기 위해 각 도구를 살펴보겠습니다.

## 1️⃣ JDBC(Java Database Connectivity)
- **JDBC(Java Database Connectivity)는** 자바에서 데이터베이스에 직접 연결하고 SQL 쿼리를 실행할 수 있게 해주는 **저수준 API**입니다.
    - JDBC(Java Database Connectivity)는 개발자가 데이터베이스에 SQL(Structured Query Language) 문을 작성하고, 결과를 처리하며, 데이터베이스와 직접 상호작용할 수 있도록 해줍니다.
- JDBC(Java Database Connectivity)는 모든 SQL(Structured Query Language) 작업(삽입, 갱신, 삭제, 조회)을 수동으로 처리해야 하며, 데이터베이스 연결 관리, 쿼리 실행, 결과 집합(ResultSet) 처리, 예외 처리 등을 개발자가 직접 관리해야 합니다.

### 👉 JDBC 예시.
```java
Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "user", "password");
PreparedStatement pstmt = conn.prepareStatement("SELECT * FROM Users WHERE id = ?");
pstmt.setInt(1, 1);
ResultSet rs = pstmt.executeQuery();
while (rs.next()) {
    System.out.println(rs.getString("name"));
}
```

- 위의 코드에서 개발자는 **SQL을 직접 작성하고, Connection 및 PreparedStatement와 같은** JDBC(Java Database Connectivity) 객체를 이용해 데이터베이스 작업을 처리해야 합니다.

## 2️⃣ Hibernate
- **Hibernate**는 자바에서 ORM(Object-Relational Mapping) 프레임워크로, 데이터베이스와의 상호작용을 더 추상화된 고수준 방식으로 처리합니다.
    - Hibernate는 데이터베이스 테이블과 자바 객체 간의 매핑을 통해, 직접적인 SQL(Structured Query Language) 쿼리 작성 없이도 데이터베이스 작업을 수행할 수 있습니다.
- **Hibernate**는 내부적으로 **JDBC(Java Database Connectivity)를** 사용하여 데이터베이스와 연결하지만, 개발자는 이를 직접 다룰 필요가 없습니다.
    - 대신, 객체 중심적인 API를 사용하여 데이터베이스 작업을 처리할 수 있습니다.
- **Hibernate**는 데이터베이스 연결 관리, 캐싱, 트랜 잭션 관리 쿼리 생성을 모두 자동화하거나 쉽게 처리할 수 있게 해줍니다.

### 👉 Hibernate 예시.
```java
Session session = sessionFactory.openSession();
User user = session.get(User.class, 1); // SQL 작성 없이 객체로 데이터 조회
System.out.println(user.getName());
```

- 위의 예시에서는 SQL(Structured Query Language)을 작성할 필요 없이 **Hibernate**가 SQL(Structured Query Language)를 자동으로 생성하고 실행하여 객체를 반환합니다.

## 3️⃣ Hibernate와 JDBC의 관계 및 차이점.

### 1️⃣ 추상화(Abstraction) 수준.
- **JDBC(Java Database Connectivity)는** 데이터베이스와의 상호작용을 처리하는 **저수준 API**입니다.
    - 개발자는 SQL(Structure Query Language)을 직접 작성하고 실행해야 하며, 연결, 트랜잭션, 예외 처리 등도 관리해야 합니다.
- **Hibernate**는 **고수준 ORM(Object-Relational Mapping) 프레임워크로, JDBC(Java Database Connectivity) 내부적으로 사용하여 SQL(Structured Query Language) 쿼리를 실행하고 데이터베이스와 상호작용하지만,** 개발자에게는 객체 지향적인 API를 제공합니다.
    - 따라서 SQL(Structured Query Language) 대신 자바 객체를 통해 데이터베이스와 상호작용할 수 있습니다.

### 2️⃣ SQL 작성.
- **JDBC(Java Database Connectivity)는** SQL(Structured Query Language)을 직접 작성해야 하며, 데이터베이스의 테이블 구조를 이해하고 그에 맞는 쿼리를 작성해야 합니다.
- **Hibernate**는 SQL(Structured Query Language)을 자동으로 생성하거나, HQL(Hibernate Query Language)과 같은 객체 지향적인 쿼리 언어를 사용할 수 있어 SQL(Structured Query Language)을 직접 작성하지 않고도 데이터를 처리할 수 있습니다.

### 3️⃣ 데이터베이스 독립성.
- **JDBC(Java Database Connectivity)는** 특정 데이터베이스에 맞춰 SQL을 작성해야 하므로 데이터베이스 종속적인 코드가 될 수 있습니다.
- **Hibernate는** 특정 데이터베이스에 종속되지 않으며, 여러 데이터베이스 간의 전환이 쉽습니다.
    - SQL(Structured Query Language)을 자동으로 생성할 때 데이터베이스 종속 적인 차이를 처리해줍니다.

### 4️⃣ 트랜잭션 및 연결 관리.
- **JDBC(Java Database Connectivity)에서는** 개발자가 직접 트랜잭션을 시작하고 종료해야 하며, 데이터베이스 연결도 수동으로 관리해야 합니다.
- **Hibernate**는 트랜잭션과 연결 관리를 자동화하여, 개발자가 이러한 세부 사항을 신경 쓸 필요가 없습니다.
    - 트랜잭션은 세션 단위로 처리되며, 데이터베이스 연결되 자동으로 처리됩니다.

### 5️⃣ 캐싱 및 성능 최적화.
- **JDBC(Java Database Connectivity)는** 캐싱 기능이 없으므로, 데이터베이스 성능 최적화를 개발자가 수동으로 처리해야 합니다.
- **Hibernate는** 1차 캐시와 2차 캐시를 제공하여, 반복적인 데이터베이스 접근을 최소화하고 성능을 최적화할 수 있습니다.

> 📝 1차 캐시와 2차 캐시.
> 
> 1차 캐시와 2차 캐시는 Hibernate에서 성능을 최적화하기 위해 사용되는 캐싱 메커니즘입니다.
> 캐시는 데이터를 메모리에 저장하여 데이터베이스 접근을 줄이고, 애플리케이션의 성능을 향상시키는 데 중요한 역할을 합니다.
> 
> 1️⃣ 1차 캐시(First-Level Cache)
> 
> 세션 범위의 캐시로, Hibernate에서 기본적으로 제공되는 캐시입니다.
> 세션(Session) 동안만 유지되며, 각 세션마다 독립적으로 존재합니다.
> 즉, 동일한 세션에서 반복적으로 동일한 데이터를 조회할 때 데이터베이스에 다시 접근하지 않고 캐시된 데이터를 반환합니다.
> 1차 캐시는 자동으로 활성화되어 있으며, 개발자가 직접 설정할 필요가 없습니다.
> 1차 캐시 덕분에, 같은 세션 내에서 동일한 엔티티를 여러 번 조회해도 데이터베이스에 불필요한 접근을 줄일 수 있습니다.
> 
> 2️⃣ 2차 캐시(Second-Level Cache)
> 
> 세션 팩토리(SessionFactory) 범위의 캐시로, 여러 세션에 걸쳐 데이터를 공유할 수 있습니다.
> 선택적으로 활성화해야 하며, 기본적으로 활성화 되어 있지 않습니다.
> 개발자가 직접 설정을 통해 활성화할 수 있습니다.
> 2차 캐시는 여러 세션 간에 데이터를 재사용하여, 자주 조회되는 데이터를 메모리에 저장하고 데이터베이스 접근을 줄입니다.
> 즉, 동일한 데이터에 대해 세션을 종료한 후에도 2차 캐시에 저장된 데이터를 여러 세션에서 재사용할 수 있습니다.
> 2차 캐시는 다양한 캐시 제공자(예: EHCache, Infinispan)를 사용하여 구현할 수 있으며, Hibernate가 제공하는 설정을 통해 제어됩니다.

## 4️⃣ 결론.
- **Hibernate**는 **JDBC(Java Database Connectivity)를** 내부적으로 사용하여 데이터베이스와 상호작용하지만, JDBC(Java Database Connectivity)보다 더 높은 추상화(Abstraction) 수준에서 ORM(Object-Relational Mapping) 기능을 제공하여, 개발자가 객체 지향적으로 데이터를 처리할 수 있게 해줍니다.
- **JDBC(Java Database Connectivity)는** SQL(Structured Query Language)을 직접 작성하고 데이터베이스와의 저수준 작업을 다루는 반면, **Hibernate**는 이러한 세부 사항을 추상화(Abstraction)하여 더 쉽게 데이터베이스와 상호작용할 수 있도록 도와줍니다.
    - Hibernate는 실질적으로 JDBC(Java Database Connectivity)의 기능을 기반으로 동작하지만, 더 높은 수준의 기능을 제공합니다.
