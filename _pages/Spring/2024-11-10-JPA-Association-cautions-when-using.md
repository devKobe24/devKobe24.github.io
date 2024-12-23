---
title: 🍃[Spring] JPA 연관관계에 대한 추가적인 기능들에는 무엇이 있을까요? - 연관관계 사용시 주의점.
tags:
    - Spring
    - Framework
date: "2024-11-10"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] JPA 연관관계에 대한 추가적인 기능들에는 무엇이 있을까요? - 연관관계 사용시 주의점.
- JPA에서 연관관계를 사용할 때는 여러가지 주의해야 할 사항이 있습니다.

## 1️⃣ 연관관계의 주인 설정.
- 연관관계에서 **주인과 비주인을 올바르게 설정해야 합니다.**
    - 주인이 아닌 쪽에서 연관관계를 수정해도 데이터베이스에는 반영되지 않기 때문에, 관계 주인을 명확히 지정하고 주인을 통해 관계를 관리해야 합니다.
        - 예를 들어, 양방향 연관관계에서 mappedBy 속성을 제대호 설정하지 않으면 불필요한 쿼리가 발생할 수 있습니다.

## 2️⃣ 지연 로딩(Lazy Loading)과 즉시 로딩(Eager Loading).
- 연관된 엔티티를 **언제 로드할지에 따라 성능이 크게 영향을 받을 수 있습니다.**
    - FetchType.LAZY는 필요한 시점에 데이터를 로드하지만, FatchType.EAGER는 엔티티를 조회할 때 즉시 연관된 모든 데이터를 로드하므로, 필요 이상의 데이터를 가져와 성능이 저하될 수 있습니다.
        - 예를 들어, `@OneToMany` 관계에서 EAGER 로딩을 사용하면 예상하지 못한 대량의 쿼리가 발생할 수 있으므로 주의해야 합니다.

## 3️⃣ 무한 루프 문제.
- 양방향 연관관계를 JSON으로 직렬화할 때, **서로 참조하는 객체들이 계속 호출되면서 무한 루프가 발생할 수 있습니다.**
    - 이를 방지하기 위해 `@JsonIgnore` 또는 `@JsonManagedReference` / `@JsonBackReference` 같은 어노테이션을 사용해 직렬화 대상에서 제외할 수 있습니다.
- 스프링과 같은 웹 애플리케이션에서 이를 처리하지 않으면 무한 루프가 발생하여 서버가 중단될 수 있습니다.

## 4️⃣ 연관관계 매핑 시 데이터 무결성 주의.
- 엔티티의 상태가 일관되도록 **연관관계 양쪽을 모두 설정하는 것이 중요합니다.**
    - 예를 들어, 양방향 관계에서 한쪽만 관계를 설정하면 다른 쪽에서는 관계가 없는 것으로 익식될 수 있습니다.
- 두 엔티티에 대해 양방향 관계를 설정할 때는, 양쪽 필드를 모두 설정하는 핼퍼 메서드를 만드는 것이 좋습니다.
```java
public void setUserProfile(UserProfile profile) {
    this.userProfile = profile;
    profile.setUser(this);
}
```

## 5️⃣ Cascade 옵션 사용 주의.
- CascadeType 옵션(CascadeType.PERSIST, CascadeType.REMOVE 등)을 사용할 때는, 자칫 불필요하게 연관된 모든 엔티티가 영속화되거나 삭제 될 수 있습니다.
    - 연관된 모든 엔티티를 한 번에 영속화하거나 삭제할 때만 신중히 사용해야 합니다.
        - 예를 들어 CascadeType.REMOVE를 잘못 설정하면 부모 엔티티 삭제 시 연관된 자식 엔티티도 모두 삭제될 수 있습니다.

## 6️⃣ N + 1 문제.
- 즉시 로딩(FetchType.EAGER)을 사용할 때 N+1 문제를 유발할 수 있습니다.
    - 예를 들어, 부모 엔티티를 조회할 때 자식 엔티티를 매번 조회하면, 추가적인 쿼리가 발생해 성능에 큰 영향을 미칩니다.
        - 이를 해결하기 위해 **JPQL의 fetch join**을 사용하여 필요한 데이터를 한 번에 조회하거나, `@BatchSize` 같은 옵션을 고려할 수 있습니다.

## 7️⃣ 데이터 변경 시 주의.
- JPA는 엔티티를 **추적하는 1차 캐시를** 가지고 있어 엔티티를 변경하면 자동으로 데이터베이스에 반영됩니다.
    - 이를 이용한 자동 반영 기능은 편리하지만, 한 트랜잭션 내에서 같은 엔티티를 여러 번 조회하거나 변경할 경우, 데이터 정합성이 깨지지 않도록 주의해야 합니다.

## 8️⃣ 복잡한 연관관계 설계 시 신중함.
- **많은 연관관계가 복잡하게 얽히면 성능에 문제가 생길 수 있고, 코드의 유지 보수성이 떨어집니다.**
    - **상황에 따라 불필요한 연관관계는 제거하고 "단방향 관계"를 사용하는 것이 좋습니다.**
- 복잡한 연관관계를 단순화하고, 연관된 엔티티 조회가 필요할 때만 쿼리를 수행하도록 설계하면 성능과 유지보수성이 향상됩니다.

## 9️⃣ 요약.
- 연관관계 주인을 정확히 설정하고, 무한 루프의 데이터 무결성을 주의합니다.
- 지연 로딩을 기본으로 하고, 필요에 따라 즉시 로딩과 페치 조인을 사용합니다.
- N+1 문제를 방지하며, Cascade 옵션은 신중히 사용합니다.
- 복잡한 연관관계는 피하고 단순화할 수 있는 구조를 선택하는 것이 좋습니다.
