---
title: 🍃[Spring] `@Transactional` 애노테이션
tags:
    - Spring
    - Framework
date: "2024-09-04"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] `@Transactional` 애노테이션.
- `@Transactional` 애노테이션은 Spring Framework에서 제공하는 애노테이션으로, 메서드나 클래스에 적용하여 해당 범위 내의 데이터베이스 작업을 하나의 트랜잭션으로 관리할 수 있도록 해줍니다.
    - 즉, `@Transactional`을 사용하면 지정된 메서드 또는 클래스 내의 데이터베이스 작업이 모두 성공해야만 커밋(commit)되고, 그렇지 않으면 롤백(rollback)됩니다.

## 1️⃣ 주요 기능.
- **1. 트랜잭션 관리.**
    - `@Transactional` 애노테이션이 적용된 메서드 내에서 수행되는 모든 데이터베이스 작업(예: INSERT, UPDATE, DELETE)은 하나의 트랜잭션으로 관리됩니다.
        - 만약 메서드 실행 중 예외가 발생하면, 해당 트랜잭션 내의 모든 변경 사항이 롤백됩니다.
- **2. 적용 범위.**
    - `@Transactional`은 클래스나 메서드에 적용할 수 있습니다.
    - 클래스에 적용하면 해당 클래스의 모든 메서드가 트랜잭션 내에서 실행됩니다.
    - 메스트에 적용되면 해당 메서드만 트랜잭션으로 관리됩니다.
- **3. 트랜잭션 전파(Propagation)**
    - `@Transactional`은 여러 전파(Propagation) 옵션을 제공하여 트랜잭션이 다른 트랜잭션과 어떻게 상호작용할지를 정의할 수 있습니다.
        - `REQUIRED` : 기본값으로, 현재 트랜잭션이 존재하면 이를 사용하고, 없으면 새로운 트랜잭션을 생성합니다.
        - `REQUIRES_NEW` : 항상 새로운 트랜잭션을 생성하고, 기존 트랜잭션을 일시 정지합니다.
        - `MANDATORY` : 현재 트랜잭션이 반드시 존재해야 하며, 없으면 예외가 발생합니다.
        - `SUPPORT` : 현재 트랜잭션이 있으면 이를 사용하고, 없으면 트랜잭션 없이 실행합니다.
        - 기타 : `NOT_SUPPORT`, `NEVER`, `NESTED` 등.
- **4. 트랜잭션 격리 수준(Isolation Level)**
    - 데이터베이스 트랜잭션의 격리 수준을 설정할 수 있습니다.
        - 이는 동시에 실행되는 여러 트랜잭션 간의 상호작용 방식을 정의합니다.
            - `READ_UNCOMMITTED` : 다른 트랜잭션의 미완료 변경 사항을 읽을 수 있습니다.
            - `READ_COMMITED` : 다른 트랜잭션의 커밋된 변경 사항만 읽을 수 있습니다.
            - `REPEATABLE_READ` : 트랜잭션 동안 동일한 데이터를 반복적으로 읽어도 동일한 결과를 보장합니다.
            - `SERIALIZABLE` : 가장 엄격한 격리 수준으로, 트랜잭션이 완전히 순차적으로 실행되도록 보장합니다.
- **5. 롤백 규칙(Rollback Rules)**
    - 기본적으로 `@Transactional` 은 `RuntimeException` 또는 `Error` 가 발생하면 트랜잭션을 롤백합니다.
        - 특정 예외에 대해 롤백을 강제하거나, 롤백을 방지하도록 설정할 수 있습니다.
    - `rollbackFor` 또는 `noRollbackFor` 속성을 사용하여 이 동작을 커스터마이징할 수 있습니다.
- **6. 읽기 전용(Read-Only)**
    - `@Transactional(readOnly = true)`로 설정하면 트랜잭션이 데이터 읽기 전용으로 동작하며, 이 경우 데이터 수정 작업이 최적화될 수 있습니다.
        - 주로 SELECT 쿼리에서 사용됩니다.
## 2️⃣ 예시 코드

```java
@Service
public class MyService {
    
    @Autowired
    private MyRepository myRepository;
    
    @Transactional
    public void performTransaction() {
        // 데이터베이스에 새로운 엔티티 추가
        myRepository.save(new MyEntity("Data1"));
        
        // 다른 데이터베이스 작업 수행
        myRepository.updateEntity(1L, "UpdatedData");
        
        // 예외 발생 시, 위의 모든 작업이 롤백됨
        if (someConditionFails()) {
            throw new RuntimeException("Transaction failed, rolling back...");
        }
    }
    
    @Transactional(readOnly = true)
    public List<MyEntity> getEntities() {
        return myRepository.findAll();
    }
}
```

## 3️⃣ 요약.
- **`@Transactional`** 은 데이터베이스 트랜잭션을 쉽게 관리할 수 있도록 해주는 Spring의 핵심 애노테이션입니다.
    - 이 애노테이션을 사용하면 메서드나 클래스의 데이터베이스 작업을 하나의 트랜잭션으로 처리하며, 실패 시 자동으로 롤백됩니다.
        - 또한, 트랜잭션 전파, 격리 수준, 롤백 규칙 등을 통해 세부적으로 트랜잭션의 동작을 제어할 수 있습니다.

이러한 기능 덕분에 `@Transactional`은 Spring 애플리케이션에서 데이터 일관성과 무결성을 유지하는 데 중요한 역할을 합니다.
