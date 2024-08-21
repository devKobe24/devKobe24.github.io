---
title: "☕️[Java] Object 배열"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-08-22"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] Object 배열.

## 1️⃣ Object 배열.
- **`'Object'`** 는 모든 타입의 객체를 담을 수 있습니다.
    - 따라서 **`'Object[]'`** 을 만들면 세상의 모든 객체를 담을 수 있는 배열을 만들 수 있습니다.

```java
package langReview.object.poly;

public class ObjectPolyExample2 {

    public static void main(String[] args) {
        Dog dog = new Dog();
        Car car = new Car();
        Object object = new Object(); // Object 인스턴스도 만들 수 있습니다.

        Object[] objects = { dog, car, object };

        size(objects);
    }

    private static void size(Object[] objects) {
        System.out.println("전달된 객체의 수는 : " + objects.length);
    }
}
```

**실행 결과**

```bash
전달된 객체의 수는 : 3
```

```java
Object[] objects = { dog, car, object };
// 쉽게 풀어서 설명하면 다음과 같이 코드를 작성할 수 있습니다.
Object objects[0] = new Dog();
Object objects[1] = new Car();
Object objects[2] = new Object();
```

- **`'Object'`** 타입을 사용한 덕분에 세상의 모든 객체를 담을 수 있는 배열을 만들 수 있게 되었습니다.

<img src = "https://github.com/devKobe24/images2/blob/main/Inflearn-Java-Mid/Object-Arr.png?raw=true">

**size() 메서드**
- **`'size(Object[] objects)'`** 메서드는 배열에 담긴 객체의 수를 세는 역할을 담당합니다.
- 이 메서드는 **`'Object'`** 타입만 사용합니다.
    - **`'Object'`** 타입의 배열은 세상의 모든 객체를 담을 수 있기 때문에, 새로운 클래스가 추가되거나 변경되어도 이 메서드를 수정하지 않아도 됩니다.
        - 지금 만든 **`'size()'`** 메서드는 자바를 사용하는 곳이라면 어디서든지 사용될 수 있습니다.

## 2️⃣ Object가 없다면 어떻게 될까?
- **`'void actioon(Object obj)'`** 과 같이 모든 객체를 받을 수 있는 메서드를 만들 수 없게됩니다.
- **`'Objectp[] objects'`** 처럼 모든 객체를 저장할 수 있는 배열을 만들 수 없게됩니다.
- 🤔 직접 구현하면 되지 않을까?
    - **`'Object'`** 가 없어도 직접 **`'MyObject'`** 와 같은 클래스를 만들고 모든 클래스에서 직접 정의한 **`'MyObject'`** 를 상속 받으면 됩니다.
        - **🙋‍♂️ 하지만 하나의 프로젝트를 넘어 전세계의 모든 개발자가 비슷한 클래스를 만들 것이고, 서로 호환되지 않는 수많은 `'XxxObject'` 들이 넘쳐나는 현상이 생길 것입니다.**
