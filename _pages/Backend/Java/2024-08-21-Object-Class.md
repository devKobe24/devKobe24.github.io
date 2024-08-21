---
title: "☕️[Java] Object 클래스"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-08-21"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] Object 클래스.
**자바에서 모든 클래스의 "최상위 부모 클래스는 항상 `'Object'` 클래스"**  입니다.

<img src = "https://github.com/devKobe24/images2/blob/main/Inflearn-Java-Mid/kyh-objcet.png?raw=true">

## 1️⃣ Object 클래스.

```java
package lang.object;

// 부모가 없으면 묵시적으로 Object 클래스를 상속 받습니다.
public class Parent {
    public void parentMethod() {
        System.out.println("Parent.parentMethod");
    }
}
```

- 위 코드는 묵시적으로 **`'Object'`** 클래스를 상속 받기 때문에 **`'extends Object'`** 코드가 없습니다.
    - 위 코드를 명시적으로 구현하면 아래와 같습니다.
        - 즉, 위 코드와 아래 코드는 같은 코드입니다.

> ✏️ **묵시적(Implicit) vs 명시적(Explicit)**
> **묵시적(Implicit) :** 개발자가 코드에 직접 기술하지 않아도 시스템 또는 컴파일러에 의해 자동으로 수행되는 것을 의미.
> **명시적(Explicit) :** 개발자가 코드에 직접 기술해서 작동하는 것을 의미.

```java
package lang.object;


public class Parent extends Object {
    public void parentMethod() {
        System.out.println("Parent.parentMethod");
    }
}
```
- 클래스에 상속 받을 부모 클래스가 없으면 **"묵시적으로 `'Object'` 클래스"** 를 상속 받습니다.
    - 즉, 자바가 **`'extends Object'`** 코드를 넣어줍니다.
        - 따라서 **`'extends Object'`**는 생략하는 것을 권장합니다.

```java
package lang.object;

public class Child extends Parent {
    public void childMethod() {
        System.out.println("Child.childMethod");
    }
}
```
- 클래스에 상속 받을 부모 클래스를 명시적으로 지정할 경우에는 **`'Object'`** 상속 받지 않습니다.
    - 즉, 이미 명시적으로 상속했기 때문에 Java가 **`'extends Object'`** 코드를 넣지 않는 것입니다.

```java
package langReview.object;

public class ObjectMain {
    public static void main(String[] args) {
        Child child = new Child();
        child.childMethod();
        child.parentMethod();

        // toString()은 Object 클래스의 메서드
        String string = child.toString();
        System.out.println(string);
    }
}
```

**실행 결과**
```bash
Child.childMethod
Parent.parentMethod
langReview.object.Child@452b3a41
```

**실행 결과 그림**
- **`'Parent'`** 는 **`'Object'`** 를 묵시적으로 상속 받았기 때문에 메모리에도 함께 생성됩니다.

<img src = "https://github.com/devKobe24/images2/blob/main/Inflearn-Java-Mid/kyh-objcet-2.png?raw=true">

- 1. **`'child.toString()'`** 을 호출합니다.
- 2. 먼저 본인의 타입인 **`'Child'`** 에서 **`'toString()'`** 을 찾습니다.
    - **`'toString()'`** 이 없으므로 부모 타입으로 올라가서 찾습니다.
- 3. 부모 타입인 **`'Parent'`** 에서 **`'toString()'`** 을 찾습니다.
    - 이곳에도 없으므로 부모 타입으로 올라가서 찾습니다.
- 4. 부모 타입인 **`'Object'`** 에서 **`'toString()'`** 을 찾습니다.
    - 부모 타입인 **`'Object'`** 에는 **`'toString()'`** 이 있으므로 이 메서드를 호출합니다.

**정리**
- 자바에서 모든 객체의 최종 부모는 **`'Object'`** 다.

## 2️⃣ 자바에서 Object 클래스가 최상위 부모 클래스인 이유.
모든 클래스가 **`'Object'`** 클래스를 상속 받는 이유는 다음과 같습니다.
- 공통 기능 제공.
- 다형성의 기본 구현.

### 1️⃣ 공통 기능 제공.
- 객체의 정보를 제공하고, 이 객체가 다른 객체와 같은지 비교하고, 객체가 어떤 클래스로 만들어졌는지 확인하는 기능은 모든 객체에게 필요한 기본 기능입니다.
    - 이런 기능을 객체를 만들 때 마다 항상 새로운 메서드를 정의해서 만들어야 한다면 상당히 번거로울 것입니다.
    - 그리고 막상 만든다고 해도 개발자마다 서로 다른 이름의 메서드를 만들어서 일관성이 없을 것입니다.
        - 예를 들어서 객체의 정보를 제공하는 기능을 만든다고 하면 어떤 개발자는 **`'toString()'`** 으로 또 어떤 개발자는 **`'objectInfo()'`** 와 같이 서로 다른 이름으로 만들 수 있습니다.
        - 객체를 비교하는 기능을 만들 때고 어떤 개발자는 **`'equals()'`** 로 어떤 개발자는 **`'same()'`** 으로 만들 수 있습니다.
- **`'Object'`** 는 모든 객체에 필요한 **`'공통 기능'`** 을 제공합니다.
- **`'Object'`** 는 최상위 부모 클래스이기 때문에 모든 객체는 **`'공통 기능'`** 을 **편리하게 제공(상속)** 받을 수 있습니다.
- **`'Object'`** 가 제공하는 기능은 다음과 같습니다.
    - 객체의 정보를 제공하는 **`'toString()'`**
    - 객체의 같음을 비교하는 **`'equals()'`**
    - 객체의 클래스 정보를 제공하는 **`'getClass()'`**
    - 기타 여러가지 기능
- 개발자는 모든 객체가 앞서 설명한 메서드를 지원한다는 것을 알고 있습니다.
    - 따라서 프로그래밍이 단순화되고, 일관성을 가집니다.

### 2️⃣ 다형성의 기본 구현.
- 부모는 자식을 담을 수 있습니다.
- **`'Object'`** 는 모든 클래스의 부모 클래스 입니다.
    - 따라서 모든 객체를 참조할 수 있습니다.
- **`'Object'`** 클래스는 다형성을 지원하는 기본적인 메커니즘을 제공합니다.
    - 모든 자바 객체는 **`'Object'`** 타입으로 처리될 수 있으며, 이는 다양한 타입의 객체를 통합적으로 처리할 수 있게 해줍니다.
        - 즉, **`'Object'`** 는 모든 객체를 다 담을 수 있습니다.
        - 타입이 다른 객체들을 어딘가에 보관해야 한다면 바로 **`'Object'`** 에 보관하면 됩니다.
