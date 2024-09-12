---
title: "☕️[Java] 변수와 초기화"
tags:
    - Java
    - Programming Language
date: "2024-02-21"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 변수와 초기화

## 변수의 종류
* 멤버 변수(필드): 클래스에 선언.
* 지역 변수: 메서드에 선언, 매개변수도 지역 변수의 한 종류입니다.

**멤버 변수, 필드 예시**
```java
public class Student {
    String name;
    int age;
    int grade;
}
```
* `name`, `age`, `grade` 는 멤버 변수입니다.

**지역 변수 예시**
```java
public class ClassStart3 {
    public static void main(String[] args) {
        Student student1;
        student1 = new Student();
        Student student2 = new Student();
    }
}
```
* `student1`, `student2`는 지역 변수입니다.

```java
public class MethodChange1 {

  public static void main(String[] args) {
    int a = 10;
    System.out.println("메서드 호출 전: a = " + a);
    changePrimitive(a);
    System.out.println("메서드 호출 후: a = " + a);
  }

  static void changePrimitive(int x) {
   x = 20;
  }
}
```

* `a`, `x`(매개변수, Parameter)는 지역변수입니다.
* 지역 변수는 이름 그대로 **"특정 지역에서만 사용되는 변수라는 뜻입니다."**
    * 예를 들어서 변수 `x`는 `changePrimitive()` 메서드의 블록에서만 사용됩니다.
        * `changePrimitive()` 메서드가 끝나면 제거됩니다.
        * `a` 변수도 마찬가지입니다. `main()` 메서드가 끝나면 제거됩니다.

## 변수의 값 초기화
* 멤버 변수: 자동 초기화.
    * 인스턴스의 멤버 변수는 인스턴스를 생성할 때 자동으로 초기화됩니다.
    * 숫자(`int`) = `0`, `boolean` = `false`, 참조형 = `null`(`null` 값은 참조할 대상이 없다는 뜻으로 사용됩니다.)
    * 개발자가 초기값을 직접 지정할 수 있습니다.
* 지역 변수: 수동 초기화.
    * 지역 변수는 항상 직접 초기화해야 합니다.

"멤버 변수의 초기화를 살펴봅시다"
**InitData**
```java
package ref;

public class InitData {
    int value1; // 초기화 하지 않음
    int value2 = 10; // 10으로 초기화
}
```

* `value1`은 초기값을 지정하지 않았고, `value2`는 초기값을 `10`으로 지정했습니다.

**InitMain**
```java
package ref;

public class initMain {

  public static void main(String[] args) {
    InitData data = new InitData();
    System.out.println("value1 = " + data.value1);
    System.out.println("value2 = " + data.value2);
  }
}
```

**실행 결과**
```
value1 = 0
value2 = 10
```

* `value1`은 초기값을 지정하지 않았지만 멤버 변수는 자동으로 초기화 됩니다.
    * 숫자는 `0`으로 초기화됩니다.
* `value2`는 `10`으로 초기값을 지정해두었기 때문에 객체를 생성할 때 `10`으로 초기화됩니다.
