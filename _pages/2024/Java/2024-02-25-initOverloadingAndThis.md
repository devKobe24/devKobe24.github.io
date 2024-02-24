---
title: "☕️[Java] 생성자 - 오버로딩 this()"
tags:
    - Java
    - Programming Language
date: "2024-02-25"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 생성자 - 오버로딩 this()
생성자도 메서드 오버로딩처럼 매개변수만 다르게 해서 여러 생성자를 제공할 수 있습니다.

**MemberConstruct - 생성자 추가**
```java
package construct;

public class MemberConstruct {
  String name;
  int age;
  int grade;

  // 추가
  MemberConstruct(String name, int age) {
    this.name = name;
    this.age = age;
    this.grade = 50;
  }

  MemberConstruct(String name, int age, int grade) {
    System.out.println("생성자 호출 name=" + name + ",age=" + age + ",grade=" + grade);
    this.name = name;
    this.age = age;
    this.grade = grade;
  }
}
```

기존 `MemberConstruct`에 생성자를 하나 추가해서 생성자가 2개가 되었습니다.
```java
MemberConstruct(String name, int age)
MemberConstruct(String name, int age, int grade)
```

새로 추가한 생성자는 `grade`를 받지 않습니다. 대신에 `grade`는 `50`점이 됩니다.

```java
package construct;

public class ConstructMain2 {

  public static void main(String[] args) {
    MemberConstruct member1 = new MemberConstruct("user1", 15, 90);
    MemberConstruct member2 = new MemberConstruct("user2", 16);

    MemberConstruct[] members = { member1, member2 };

    for (MemberConstruct member : members) {
      System.out.println("이름:" + member.name + " 나이:" + member.age + "성적:" + member.grade);
    }
  }
}
```

**실행 결과**
```
생성자 호출 name=user1,age=15,grade=90
이름:user1 나이:15성적:90
이름:user2 나이:16성적:50
```

* 생성자를 오버로딩 한 덕분에 성적 입력이 꼭 필요한 경우에는 `grade`가 있는 생성자를 호출하면 되고, 그렇지 않은 경우에는 `grade`가 없는 생성자를 호출하면 됩니다.
    * `grade`가 없는 생성자를 호출하면 성적은 `50`점이 됩니다.

## this()
두 생성자를 비교해 보면 코드가 중복되는 부분이 있습니다.
```java
public MemberConstruct(String name, int age) {
    this.name = name;
    this.age = age;
    this.grade = 50;
}

public MemberConstruct(String name, int age, int grade) {
    this.name = name; 
    this.age = age;
    this.grade = grade;
}
```

바로 다음 부분입니다.
```java
this.name = name;
this.age = age;
```

이때 `this()`라는 기능을 사용하면 생성자 내부에서 자신의 생성자를 호출할 수 있습니다.

> 참고로 `this`는 인스턴스 자신의 참조값을 가리킵니다. 그래서 자신의 생성자를 호출한다고 생각하면됩니다.

**MemberConstruct - this() 사용**
```java
package construct;

public class MemberConstruct {
  String name;
  int age;
  int grade;

  // 추가
  MemberConstruct(String name, int age) {
    this(name, age, 50);
  }

  MemberConstruct(String name, int age, int grade) {
    System.out.println("생성자 호출 name=" + name + ",age=" + age + ",grade=" + grade);
    this.name = name;
    this.age = age;
    this.grade = grade;
  }
}
```

* 이 코드는 첫번째 생성자 내부에서 두번째 생성자를 호출합니다.
```java
MemberConstruct(String name, int age) -> MemberConstruct(String name, int age, int grade)
```

* `this()`를 사용하면 생성자 내부에서 다른 생성자를 호출할 수 있습니다.
    * 이 부분을 잘 활용하면 지금과 같이 중복을 제거할 수 있습니다.
        * 물론 실행 결과는 기존과 같습니다.

**this() 규칙**
* `this()`는 생성자 코드의 첫줄에만 작성할 수 있습니다.

다음은 규칙 위반입니다.
이 경우 컴파일 오류가 발생합니다.
```java
public MemberConstruct(String name, int age) {
    Sytem.out.println("go");
    this(name, age, 50);
}
```
* `this()`가 생성자 코드의 첫줄에 사용되지 않았습니다.

