---
title: "☕️[Java] 객체 사용"
tags:
    - Java
    - Programming Language
date: "2024-02-16"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 객체 사용

**"클래스를 통해 생성한 객체를 사용하려면 먼저 메모리에 존재하는 객체에 접근해야 합니다."**
* 객체에 접근하려면 `.`(점, dot)을 사용하면 됩니다.

```java
// 객체 값 대입
student1.name = "학생1";
student1.age = 15;
student1.grade = 90;

// 객체 값 사용
System.out.println("이름:" + student1.name + " 나이:" + student1.age + " 성적:" + student1.grade);
```

## 객체에 값 대입
* 객체가 가지고 있는 멤버 변수(`name`, `age`, `grade`)에 값을 대입하려면 먼저 객체에 접근해야 합니다.
    * 객체에 접근하려면 `.`(점, dot) 키워드를 사용하면 됩니다.
        * 이 키워드는 변수(`student1`)에 들어있는 참조값(`x001`)을 읽어서 메모리에 존재하는 객체에 접근합니다.

**순서를 간단히 풀어보겠습니다.**
```java
student1.name = "학생1" // 1. student1 객체의 name 멤버 변수에 값 대입
x001.name = "학생1" // 2. 변수에 있는 참조값을 통해 실제 객체에 접근, 해당 객체의 name 멤버 변수에 값 대입
```

* `student1.`(dot)이라고 하면 `student1` 변수가 가지고 있는 참조값을 통해 실제 객체에 접근합니다.
* `student1`은 `x001`이라는 참조값을 가지고 있으므로 `x001` 위치에 있는 `Student` 객체에 접근합니다.

## 객체 값 읽기
객체의 값을 읽는 것도 앞서 설명한 내용과 같습니다.
* `.`(점, dot) 키워드를 통해 참조값을 사용해서 객체에 접근한 다음에 원하는 작업을 하면 됩니다.

아래 예시 코드를 봅시다.
```java
// 1. 객체 값 읽기.
System.out.println("이름:" + student1.name);
// 2. 변수에 있는 참조값을 통해 실제 객체에 접근하고, name 멤버 변수에 접근합니다.
System.out.println("이름:" + x001.name);
// 3. 객체의 멤버 변수의 값을 읽어옵니다.
System.out.println("이름:" + "학생1");
```
