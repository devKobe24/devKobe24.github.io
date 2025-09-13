---
title: "💾 [CS] 노출 모듈 패턴(revealing module pattern)"
tags:
    - CS
date: "2024-09-19"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 노출 모듈 패턴(revealing module pattern).

## 1️⃣ 노출 모듈 패턴(revealing module pattern).
- 자바에서 노출 모듈 패턴(revealing module patter)은 객체지향 프로그래밍의 모듈화 개념을 사용하여 내부 구현 세부사항을 숨기고, 외부에 필요한 부분만 노출하는 디자인 패턴입니다.
    - 이 패턴은 주로 자바스크립트에서 많이 사용되지만, 자바에서도 유사한 방식으로 구현할 수 있습니다.

## 2️⃣ 핵심 개념.
- **정보 은닉.**
    - 내부 데이터나 메서드를 외부에 노출하지 않고, 꼭 필요한 부분만 노출하여 객체의 캡슐화를 유지합니다.
- **외부 노출 제어.**
    - 외부에서 접근할 수 있는 메서드와 내부적으로만 사용되는 메서드를 구분하여, 내부 구현의 변경이 외부 코드에 영향을 미치지 않도록 합니다.

## 3️⃣ 자바에서의 구현 예시.
```java
public RevealingModule {
    
    // private 필드
    private String hiddenData = "This is hidden";
    
    // 외부에서 접근할 수 없는 메서드
    private void hiddenMethod() {
        System.out.println("This method is hidden");
    }
    
    // 외부에 노출할 메서드
    public void showData() {
        System.out.println("Exposed Data: " + hiddenData);
    }
    
    // 외부에서 사용할 수 있도록 숨겨진 메서드를 호출하는 메서드
    public void callHiddenMethod() {
        hiddenMethod(); // 외부에서 접근 불가능한 메서드를 내부적으로 호출
    }
}
```

### 설명.
- 1. **`hiddenData`** 와 **`hiddenMethod`** 는 외부에 노출되지 않는 필드와 메서드입니다.
    - 이는 클래스 내부에서만 사용되며 외부 객체는 접근할 수 없습니다.

- 2. **`showData`** 와 **`callHiddenMethod`** 는 외부에 노출되는 메서드로, 이 메서드를 통해 외부에서 내부의 일부 데이터나 메서드에 접근할 수 있습니다.

### 장점.
- **캡슐화 강화.**
    - 외부에서 불필요한 내부 구조에 접근할 수 없도록 하여 보안성은 높입니다.

- **코드 유지보수성.**
    - 내부 구현을 변경하더라도 외부 인터페이스가 일정하면, 외부 코드에 영향을 미치지 않고 유연하게 수정 가능합니다.

---

- 이 패턴을 통해 자바에서도 모듈간의 명확한 경계를 설정하고, 유지보수성을 높이는 코드를 작성할 수 있습니다.
