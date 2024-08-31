---
title: "💾 [CS] 팩토리 패턴(factory pattern)"
tags:
    - CS
date: "2024-08-31"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 팩토리 패턴(factory pattern).

## 1️⃣ 팩토리 패턴(factory pattern).
- 팩토리 패턴(factory pattern)은 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 [추상화](https://www.devkobe24.com/Backend/CS/2024-08-31-Abstraction.html)한 패턴이자 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴입니다.

- 상위 클래스와 하위 클래스가 분리되기 때문에 느슨한 결합을 가지며 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없기 때문에 더 많은 유연성을 갖게 됩니다.
    - 그리고 객체 생성 로직이 따로 떼어져 있기 때문에 코드를 리팩터링하더라도 한 곳만 고칠 수 있게 되니 유지 보수성이 증가됩니다.
        - 예를 들어 라떼 레시피와 아메리카노 레시피, 우유 레시피라는 구체적인 내용이 들어 있는 하위 클래스가 컨베이어 벨트를 통해 전달되고, 상위 클래스인 바리스타 공장에서 이 레시피들을 토대로 우유 등을 생산하는 생산 공정을 생각하면 됩니다.

<img src = "https://github.com/devKobe24/images2/blob/main/CS_IMG/cs-factory-example.png?raw=true">

## 2️⃣ 자바의 팩토리 패턴

```java
enum CoffeeType {
    LATTE,
    ESPRESSO
}

abstract class Coffee {
    protected String name;
    
    public String getName() {
        return name;
    }
}

class Latte extends Coffee {
    public Latte() {
        name = "latte";
    }
}

class Espresso extends Coffee {
    public Espresso() {
        name = "Espresso";
    }
}

class CoffeeFactory {
    public static Coffee createCoffee(CoffeeType type) {
        switch (type) {
            case LATTE:
                return new Latte();
            case ESPRESSO:
                return new Espresso();
            default:
                throw new IllegalArgumentException("Invalid coffee type: " + type);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Coffee coffee = CoffeeFactory.createCoffee(CoffeeType.LATTE);
        System.out.println(coffee.getName()); // latte
    }
}
```

## 3️⃣ 코드 설명.
- **팩토리 패턴(Factory Pattern)** 은 객체 생성의 로직을 별도의 클래스나 메서드로 분리하여 관리하는 디자인 패턴입니다.
    - 이는 객체 생성에 관련된 코드를 클라이언트 코드에서 분리하여, 객체 생성의 변화에 대한 유연성을 높이고 코드의 유지보수성을 개선하는 데 도움이 됩니다.
- **팩토리 패턴** 은 크게 **팩토리 메서드 패턴** 과 **추상 팩토리 패턴** 으로 구분되며, 위 코드 예시는 **팩토리 메스드 패턴** 의 전형적인 예입니다.

### 1. `CoffeeType` 열거형(Enum)

```java
enum CoffeeType {
    LATTE,
    ESPRESSO
}
```

- **설명 :** **`CoffeeType`** 은 커피의 종류를 나타내는 열거형(Enum)입니다.
    - 이 열거형은 **`LATTE`** 와 **`ESPRESSO`** 두 가지 타입의 커피를 정의하고 있습니다.
- **역할 :** 커피의 종류를 코드 내에서 명확하게 구분하고, **`CoffeeFactory`** 에서 커피 객체를 생성할 때 사용됩니다.

### 2. `Coffee` 추상 클래스.

```java
abstract class Coffee {
    protected String name;
    
    public String getName() {
        return name;
    }
}
```

- **설명 :** **`Coffee`** 는 커피 객체의 공통된 속성과 메서드를 정의한 추상 클래스입니다.
    - **`name`** 필드는 커피의 이름을 저장하며, **`getName()`** 메서드는 커피의 이름을 반환합니다.
- **역할 :** 구체적인 커피 클래스들이 상속받아야 하는 공통적인 기능을 정의합니다.

### 3. `Latte` 와 `Espresso` 클래스.
```java
class Latte extends Coffee {
    public Latte() {
        name = "latte";
    }
}

class Espresso extends Coffee {
    public Espresso() {
        name = "Espresso";
    }
}
```

- **설명 :** **`Latte`** 와 **`Espresso`** 는 **`Coffee`** 클래스를 상속받아 구체적인 커피 타입을 구현한 클래스들입니다.
    - 각 클래스는 생성자에서 **`name`** 필드를 특정 커피 이름으로 초기화합니다.
- **역할 :** 특정 커피 타입의 객체를 생성하는 역할을 합니다.

### 4. `CoffeeFactory` 클래스.
```java
class CoffeeFactory {
    public static Coffee createCoffee(CoffeeType type) {
        switch (type) {
            case LATTE:
                return new Latte();
            case ESPRESSO:
                return new Espresso();
            default:
                throw new IllegalArgumentException("Invalid coffee tyep: " + type);
        }
    }
}
```

- **설명 :** **CoffeeFactory** 클래스는 팩토리 패턴의 핵심으로, **`createCoffee()`** 메서드를 통해 특정 타입의 커피 객체를 생성하여 반환합니다.
    - **`CoffeeType`** 열거형에 따라 적절한 커피 객체를 생성합니다.
- **역할 :** 객체 생성의 로직을 중앙 집중화하여 클라이언트 코드에서 객체 생성의 책임을 분리합니다.
    - 클라이언트는 **`CoffeeFactory`** 의 **`createCoffee()`** 메서드를 호출하여 원하는 커피 객체를 생성할 수 있습니다.

### 5. `Main` 클래스.
```java
public class Main {
    public static void main(String[] args) {
        Coffee coffee = CoffeeFactory.createCoffee(CoffeeType.LATTE);
        System.out.println(coffee.getName()); // latte
    }
}
```

- **설명 :** **`Main`** 클래스는 클라이언트 코드로, **`CoffeeFactory`** 를 사용하여 **`LATTE`** 타입의 커피 객체를 생성하고, 그 이름을 출력합니다.
- **역할 :** 팩토리 패턴을 사용하는 클라이언트 코드로, 직접적으로 객체를 생성하지 않고 팩토리를 통해 객체를 생성합니다.

## 4️⃣ 팩토리 패턴의 장점.
- **1. 코드의 유연성 증가.**
    - 객체 생성 로직이 중앙화되어 있으므로, 새로운 커피 타입을 추가할 때 클라이언트 코드를 수정할 필요 없이 팩토리 클래스만 수정하면 됩니다.
- **2. 유지보수성 향상.**
    - 객체 생성 코드가 한 곳에 모여 있어 코드의 유지보수가 쉬워집니다.
        - 객체 생성 과정에서의 변경이 필요한 경우에도 팩토리 클래스만 수정하면 됩니다.
- **3. 코드의 결합도 감소.**
    - 클라이언트 코드는 구체적인 클래스에 의존하지 않고, 인터페이스나 추상 클래스를 통해 객체를 다루기 때문에 결합도가 낮아집니다.

## 5️⃣ 팩토리 패턴의 단점.
- **1. 클래스의 복잡성 증가.**
    - 객체 생성을 위한 팩토리 클래스가 추가됨으로써 클래스의 수가 증가하고, 코드 구조가 다소 복잡해질 수 있습니다.
- **2. 확장 시 주의 필요.**
    - 새로운 커피 타입을 추가할 때마다 팩토리 클래스의 **`switch`** 문이나 **`if-else`** 문이 증가할 수 있어, 확장성이 제한될 수 있습니다.
        - 이 문제를 해결하기 위해서는 추상 팩토리 패턴이나 다른 디자인 패턴과 결합하는 방법을 고려할 수 있습니다.

## 6️⃣ 결론.
- 팩토리 패턴은 객체 생성의 책임을 분리하여 코드의 유연성과 유지보수성을 높이는 강력한 디자인 패턴입니다.
    - 위 코드 예시에서는 커피 객체를 생성하는 로직을 **`CoffeeFactory`** 클래스에 모아두어, 클라이언트 코드가 특정 커피 클래스에 직접적으로 의존하지 않도록 하였습니다.
        - 이를 통해 클라이언트 코드는 커피 객체의 생성 방식에 대해 신경 쓰지 않고도 다양한 타입의 커피를 생성하고 사용할 수 있게 됩니다.
