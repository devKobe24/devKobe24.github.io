---
title: 🍃[Spring] 유일한 식별자(Primary Key)
tags:
    - Spring
    - Framework
date: "2024-09-26"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 유일한 식별자(Primary Key).

Java 백엔드 애플리케이션에서 user의 id 정보는 보통 **유일한 식별저(Primary Key)** 로 사용되며, **각 유저별로 겹치지 않는 고유한 값**입니다.

이는 데이터베이스 설계에서 매우 중요한 개념으로, 사용자와 같은 [엔티티(Entity)](https://www.devkobe24.com/CS/2024/2024-09-26-APIdesign-LayeredArchitecture-Transaction-Entity-BusinessLogicAndRules.html)를 고유하게 식별하기 위해 **Primary Key**를 사용합니다.

## 1️⃣ Primary Key란?

- **Primary Key**는 데이터베이스 테이블에서 각 행을 고유하게 식별하는 열(Column)입니다.
- Primary Key는 테이블 내에서 중복될 수 없으며, `null` 값을 가질 수 없습니다.
- 즉, 각 레코드는 Primary Key를 통해 식별되므로, `user` 테이블에서 각 사용자는 유일한 `id`를 가져야 하며, 이를 통해 사용자를 구분할 수 있습니다.

## 2️⃣ 일반적인 예: User 엔티티

`user` 엔티티에서 `id` 필드는 주로 Primary Key로 설정되며, 데이터베이스에서 자동으로 생성되거나 애플리케이션에서 생성할 수도 있습니다.

### User 엔티티 예시 (JPA를 사용한 경우)

```java
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id; // Primary Key, 유일한 식별자.
    
    private String name;
    private String email;
    
    // Getter, Setter, 기본 생성자
    public User() {}
    
    public Long getId() {
        return id;
    }
    
    public void setId(Long id) {
        this.id = id;
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public String getEmail() {
        return email;
    }
    
    public void setEmail(String email) {
        this.email = email;
    }
}
```

- `@Id`
    - `id` 필드는 Primary Key임을 나타냅니다.
- `@GeneratedValue`
    - `id` 값이 자동으로 생성됨을 의미합니다.
    - `GenerationType.IDENTITY`는 데이터베이스에서 자동으로 Primary Key 값을 생성하도록 지정하는 방식입니다.

## 3️⃣ Primary Key의 역할.
- **1. 고유성 보장**
    - Primary Key는 각 레코드를 고유하게 식별하므로, 사용자 정보에서 `id`는 각 유저별로 유일하게 존재하며 절대 중복되지 않습니다.

- **2. 빠른 조회**
    - Primary Key는 인덱스가 자동으로 생성되기 때문에 데이터베이스에서 해당 레코드를 빠르게 조회할 수 있습니다.

- **3. 관계 설정**
    - Primary Key는 다른 테이블에서 **Foreign Key**로 사용되어 테이블 간의 관계를 설정할 수 있습니다.
    - 예를 들어, `Order` 테이블에서 `user_id`는 `User` 테이블의 Primary Key를 참조하여 주문과 사용자를 연결할 수 있습니다.

## 4️⃣ ID 생성 방식.
ID는 여러 방식으로 생성될 수 있으며, 가장 많이 사용하는 두 가지 방식은 다음과 같습니다.

- **1. 자동 증가(Auto Increment)**
    - 데이터베이스에서 `AUTO_INCREMENT` 속성을 설정하여 ID가 자동으로 증가합니다.
    - 주로 MySQL, PostgreSQL 같은 데이터베이스에서 사용됩니다.

- **2. UUID(Universally Unique Identifier)**
    - UUID는 전 세계적으로 고유한 식별자를 생성하는 방식으로, 중복될 가능성이 거의 없습니다.
    - JPA에서는 `UUID`를 Primary Key로 사용할 수도 있습니다.
        - 예시
        ```java
        @Id
        @GeneratedValue(generator = "UUID")
        @GenericGenerator(name = "UUID", strategy = "org.hibernate.id.UUIDGenerator")
        private String id;
        ```
        
## 5️⃣ 데이터베이스에서 User 테이블 예시
```sql
CREATE TABLE users (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255),
    email VARCHAR(255)
);
```

- 여기서 `id`는 Primary Key로, 데이터베이스에서 자동으로 증가하며 각 사용자는 고유한 `id` 값을 가집니다.

## 6️⃣ 예시: Primary Key를 통한 사용자 조회
```java
@RestController
@RequestMapping("/users")
public class UserController {
    
    @Autowired
    private UserRepository userRepository;
    
    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        User user = userRepository.findById(id)
            .orElseThrow(() -> new UserNotFoundException("User not found with id " + id));
        return ResponseEntity.ok(user);
    }
}
```

- 위 코드는 `id`가 Primary Key인 `User` 엔티티를 데이터베이스에서 조회하는 예시입니다.
- `id`는 고유하기 때문에 이 값을 통해 특정 사용자를 정확하게 조회할 수 있습니다.

## 7️⃣ 요약.
- Java 백엔드 애플리케이션에서 `user`의 `id`는 보통 Primary Key로 사용되며, 각 사용자는 고유한 `id` 값을 가집니다.
- Primary Key는 데이터베이스에서 각 레코드를 고유하게 식별하는 필드로, 절대 중복되지 않으며 `null` 값을 가질 수 없습니다.
- Primary Key를 통해 사용자를 빠르고 정확하게 조회할 수 있으며, 테이블 간의 관계를 설정하는 데 중요한 역할을 합니다.
- `id`는 자동 증가 방식이나 `UUID` 방식으로 생성될 수 있으며, 사용자의 고유성을 보장합니다.
