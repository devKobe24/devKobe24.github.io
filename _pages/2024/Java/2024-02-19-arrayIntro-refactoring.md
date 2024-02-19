---
title: "☕️[Java] 배열 도입 - 리팩토링"
tags:
    - Java
    - Programming Language
date: "2024-02-19"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 배열 도입 - 리팩토링

아래 코드를 보면 배열을 사용한 덕분에 출력에서 `for문`을 **"도입할 수 있게 되었습니다."**

**ClassStart5**
```java
package class1;

public class ClassStart5 {

  public static void main(String[] args) {

    Student student1 = new Student();
    student1.name = "학생1";
    student1.age = 15;
    student1.grade = 90;

    Student student2 = new Student();
    student2.name = "학생2";
    student2.age = 16;
    student2.grade = 80;
    
    // 배열 선언
    Student[] students = new Student[]{student1, student2};
      
    // for문 적용
    for (int i = 0; i < students.length; i++) {
      System.out.println("이름:" + students[i].name + " 나이:" + students[i].age + " 성적:" + students[i].grade);
    }
}
```

## 배열 선언 최적화.
* 개발자가 직접 정의한 **'`Student`'** 타입도 일반적인 변수와 동일하게 배열을 생성할 때 포함할 수 있습니다.
```java
Student[] students = new Student[]{student1, student2};
```

* 생성과 선언을 동시에 하는 경우 다음과 같이 더 최적화 할 수 있습니다.
```java
Student[] students = {student1, student2};
```

## for문 최적화.
* 배열을 사용한 덕분에 `for문`을 사용해서 반복 작업을 깔끔하게 처리할 수 있습니다.

**for문 도입**
```java
for (int i = 0; i < students.length; i++) {
      System.out.println("이름:" + students[i].name + " 나이:" + students[i].age + " 성적:" + students[i].grade);
    }
```

**for문 - 반복 요소를 변수에 담아서 처리하기**
```java
for (int i = 0; i < students.length; i++) {
      Student s = students[i];
      System.out.println("이름:" + s.name + " 나이:" + s.age + " 성적:" + s.grade);
}
```

* `students[i].name`, `students[i].age` 처럼 `students[i]`를 자주 접근하는 것이 번거롭다면 반복해서 사용하는 객체를 `Students s`와 같은 변수에 담아두고 사용해도 됩니다.

* 물론 이런 경우에는 다음과 같이 `향상된 for문`을 사용하는 것이 가장 깔끔합니다.
**향상된 for문(Enhanced For Loop)**
```java
for (int i = 0; i < students.length; i++) {
    Student s = students[i];
    System.out.println("이름:" + s.name + " 나이:" + s.age + " 성적:" + s.grade);
}
```
