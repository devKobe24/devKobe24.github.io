---
title: "☕️[Java] @Transactional의 역할과 의미."
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-08-02"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] @Transactional의 역할과 의미.

- **`@Transaction`** 어노테이션은 스프링 프레임워크에서 제공하는 선언적 트랜젝션 관리 기능을 활용하기 위해 사용됩니다.
- 이 어노테이션을 사용함으로써, 특정 메서드 또는 클래스 전체에 걸쳐 데이터베이스 트랜잭션의 경계를 설정할 수 있습니다.
- 트랜잭션은 일련의 연산들이 전부 성공적으로 완료되거나, 하나라도 실패할 경우 전체를 취소(롤백)하여 데이터의 일관성과 정합성을 보장하는 것을 목적으로 합니다.

## 1️⃣ `@Transactional`의 주요 기능과 특징.

- **1. 자동 롤백**
    - **`@Transactional`** 이 적용된 메서드에서 런타임 예외(RuntimeException)가 발생하면, 그 트랜잭션에서 수행된 모든 변경이 자동으로 롤백됩니다.
    - 이는 데이터의 일관성을 유지하는 데 필수적입니다.
- **2. 프로파게이션(Propagation)**
    - 트랜잭션의 전파 행위를 제어합니다.
        - 예를 들어, 이미 진행 중인 트랜잭션이 있을 때 새로운 트랜잭션을 시작할 것인지, 아니면 기존 트랜잭션을 참여할 것인지 결정할 수 있습니다.
            - **`REQUIRED`(기본값) :** 이미 진행 중인 트랜잭션이 있다면 그 트랜잭션이 참여하고, 없다면 새로운 트랜잭션을 시작합니다.
            - **`REQUIRED_NEW` :** 항상 새로운 트랜잭션을 시작합니다. 이미 진행 중인 트랜잭션이 있다면 잠시 보류합니다.
- **3. 격리 수준(Isolation Level)**
    - 다른 트랜잭션이 데이터에 동시에 접근했을 때 발생할 수 있는 문제를 제어합니다.
        - 예를 들어, **`READ_COMMITTEED`, `REPEATED_READ`, `SERIALIZABLE`** 등 다양한 격리 수준을 지정할 수 있습니다.
- **4. 읽기 전용(Read-Only)**
    - 트랜잭션을 읽기 전용으로 설정할 수 있어, 데이터 수정이 이루어지지 않는다는 것을 데이터베이스 최적화 엔진에 알려 성능을 향상시킬 수 있습니다.
- **5. 롤백 규칙(Rollback Rules)**
    - 특정 예외가 발생했을 때 롤백을 수행할지 아니면 커밋을 수행할지를 세밀하게 제어할 수 있습니다.
    - 기본적으로 런타임 예외에서는 롤백을 수행하고, 체크 예외에서는 커밋을 수행합니다.

## 2️⃣ 사용 예제.
```java
import org.springframework.transaction.annotation.Transactional;
import org.springframework.stereotype.Service;

@Service
public class TransactionalService {
    
    @Transactional(readOnly = true)
    public User getUser(Long id) {
        return userRepository.findById(id);
    }
    
    @Transactional(rollbackFor = Exception.class)
    public User updateUser(User user) {
        return userRepository.save(user);
    }
}
```
- 위 예시처럼, **`getUser`** 메서드는 데이터를 변경하지 않고 조회만 수행하기 때문에 **`readOnly = true`** 로 설정했습니다.
- 반면, **`updateUser`** 메서드는 데이터를 변경할 가능성이 있으므로, 모든 예외(**`Exception`**)가 발생할 경우 롤백하도록 설정했습니다.

---

**`@Transactional`** 을 사용함으로써 개발자는 복잡한 트랜잭션 관리 코드를 직접 작성하지 않고도, 스프링 프레임워크가 제공하는 선언적 방식을 통해 간단하게 트랜잭션을 관리할 수 있게 됩니다.
이는 애플리케이션의 데이터 처리 로직을 더욱 안정적이고 효율적으로 만듭니다.
