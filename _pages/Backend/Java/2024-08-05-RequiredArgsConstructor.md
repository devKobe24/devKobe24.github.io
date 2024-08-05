---
title: "☕️[Java] @RequiredArgsConstructor의 역할."
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-08-05"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] @RequiredArgsConstructor 역할.

- **`RequiredArgsConstructor`** 어노테이션은 **`Lombok`** 라이브러리에서 제공하는 기능 중 하나로, 클래스에 필수적인 생성자를 자동으로 생성하는 역할을 합니다.
- 이 어노테이션을 클래스에 적용하면, **`Lombok`** 이 그 클래스의 **`final`** 필드 또는 **`@NonNull`** 어노테이션이 붙은 필드를 인자로 받는 생성자를 자동으로 생성합니다.

## 1️⃣ `@RequiredArgsConstructor`의 주요 기능.
- **1. 자동 생성자 생성** 
    - 클래스 내의 모든 **`final`** 필드와 **`@NonNull`** 어노테이션이 붙은 필드에 대한 생성자를 자동으로 생성합니다.
    - 이 생성자는 이 필드들을 초기화하는 데 필요한 파라미터를 요구합니다.

- **2. 코드 간결화**
    - 수동으로 생성자를 작성하는 번거로움을 줄여줍니다.
    - 특히 많은 필드를 가진 클래스에서 유용하게 사용될 수 있습니다.

- **3. 불변성 강화**
    - **`final`** 필드를 사용함으로써 클래스의 불변성을 강화할 수 있습니다.
    - 생성자를 통해 한 번 설정되면, 이 필드들의 값은 변경될 수 없습니다.

- **4. 의존성 주입 용이**
    - Spring과 같은 프레임워크에서 생성자를 통한 의존성 주입을 사용할 때 유용합니다.
    - 필요한 의존성을 생성자를 통해 주입받기 때문에, 컴포넌트 간의 결합도를 낮출 수 있습니다.

## 2️⃣ 사용 예시.

- 다음은 **`@RequiredArgsConstructor`** 어노테이션을 사용한 간단한 클래스 예제입니다.

```java
import lombok.RequiredArgsConstructor;
import lombok.NonNull;

@RequiredArgsConstructor
public class UserData {
    private final String username; // final 필드에 대한 생성자 파라미터 자동 포함.
    @NonNull private String email; // @NonNull 필드에 대한 생성자 파라미터 자동 포함.
    
    // 추가 메소드 등
}
```

- 위 코드에서 **`UserData`** 클래스에는 **`username`** 과 **`email`** 두 필드가 있으며, **`username`** 은 **`final`** 로 선언되어 수정할 수 없고, **`email`** 은 **`@NonNull`** 어노테이션이 붙어 null 값을 허용하지 않습니다.
- Lombok은 이 두 필드를 초기화하는 생성자를 자동으로 생성합니다.

## 3️⃣ 주의 사항.
- **`@RequiredArgsConstructor`** 는 필드가 많고, 특히 final 또는 @NonNull 필드가 있는 경우 유용합니다.
    - 그러나 생성자를 통한 초기화가 필요하지 않은 필드에는 적용되지 않습니다.

- Lombok을 사용하면 코드가 간결해지고 가독성이 향상되지만, 코드의 명시성이 다소 떨어질 수 있습니다.
    - 따라서 Lombok 사용 시, 팀 내에서 Lombok에 대한 이해도가 충분한지 확인하는 것이 좋습니다.

---

- **Lombok** 의 **`@RequiredArgsConstructor`** 는 반복적인 코드 작성을 줄여주고, 오류 가능성을 감소시키며, 더 깔끔하고 관리하기 쉬운 코드베이스를 유지하는 데 도움을 줄 수 있습니다.
