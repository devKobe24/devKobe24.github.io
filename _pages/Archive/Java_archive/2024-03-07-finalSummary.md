---
title: "☕️[Java] final 정리"
tags:
    - Java
    - Programming Language
date: "2024-03-07"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# final 정리.

`final`은 매우 유용한 제약입니다.
만약 특정 변수의 값을 할당한 이후에 변경하지 않아야 한다면 `final`을 사용합시다.
* 예를 들어서 고객의 `id`를 변경하면 큰 문제가 발생한다면 `final`로 선언하고 생성자로 값을 할당합시다.
    * 만약 어디선가 실수로 `id` 값을 변경한다면 컴파일러가 문제를 찾아줄 것입니다.

```java
package final1.ex;

public class Member {

  private final String id; // final 키워드 사용
  private String name;

  public Member(String id, String name) {
    this.id = id;
    this.name = name;
  }

  public void changeData(String id, String name) {
//    this.id = id; // 컴파일 오류 발생
    this.name = name;
  }

  public void print() {
    System.out.println("id:" + id + ", name:" + name);
  }
}
```
* `changeData()` 메서드에서 `final`인 `id` 값 변경을 시도하면 컴파일 오류가 발생합니다.

```java
package final1.ex;

public class MemberMain {

  public static void main(String[] args) {
    Member member = new Member("dev", "kobe");
    member.print();
    member.changeData("dev.skyachieve", "kang");
    member.print();
  }
}
```

**실행 결과**
```
id:dev, name:kobe
id:dev, name:kang
```
