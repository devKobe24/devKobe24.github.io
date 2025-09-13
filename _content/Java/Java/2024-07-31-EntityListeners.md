---
title: "☕️[Java] @EntityListeners 어노테이션."
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-07-31"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] @EntityListeners 어노테이션.

## 1️⃣ 사용 방법.

- **`@EntityListeners`** 어노테이션은 엔티티 클래스 또는 매핑된 슈퍼클래스에서 사용할 수 있으며, 하나 이상의 리스너 클래스를 지정할 수 있습니다.
    - 이 리스너 클래스들은 앞서 언급한 이벤트를 처리할 메소드들을 포함하고 있어야 합니다.

```java
import javax.persistence.EntityListeners;
import javax.persistence.PostPersist;

@EntityListeners(MyEntityListner.class)
public class MyEntity {
    // 엔티티 필드와 메서드
}

public class MyEntityListner {
    @PostPersist
    public void afterPersist(Object entity) {
        System.out.println("Entity has been persisted: " + entity);
    }
}
```

## 2️⃣ 사용 사례.
- 엔티티 리스너는 로깅, 유효성 검사, 보안 검사, 비즈니스 로직 실행 등 다양한 목적으로 사용할 수 있습니다.
    - 예를 들어, 사용자 계정의 엔티티가 데이터베이스에 저장될 때 비밀번호 강도를 자동으로 검증하거나, 엔티티가 업데이트 될 때 특정 필드의 변경을 추적할 수 있습니다.
---

**이처럼 `@EntityListeners`는 JPA 엔티티의 생명주기에 자동으로 반응하는 메소드를 구현함으로써, 엔티티와 관련된 비즈니스 로직을 분리하고 관리하는 데 큰 도움을 줍니다.**
