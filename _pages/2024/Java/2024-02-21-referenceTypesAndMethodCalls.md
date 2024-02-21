---
title: "☕️[Java] 참조형과 메서드 호출 - 활용"
tags:
    - Java
    - Programming Language
date: "2024-02-21"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 참조형과 메서드 호출 - 활용

아래의 코드 **class1.ClassStart3** 코드에는 중복되는 부분이 2가지가 있습니다.
* `name`, `age`, `grade`에 값을 할당하는 부분.
* 학생 정보를 출력하는 부분.

```java
package class1;

public class ClassStart3 {

  public static void main(String[] args) {
    Student student1;
    student1 = new Student();
    student1.name = "학생1";
    student1.age = 15;
    student1.grade = 90;

    Student student2 = new Student();
    student2.name = "학생2";
    student2.age = 16;
    student2.grade = 80;

    System.out.println("이름:" + student1.name + " 나이:" + student1.age + " 성적:" + student1.grade);
    System.out.println("이름:" + student2.name + " 나이:" + student2.age + " 성적:" + student2.grade);
  }
}
```

**"이러한 중복은 메서드를 통해 손쉽게 제거할 수 있습니다."**

## 메서드에 객체 전달
다음과 같이 코드를 작성해봅시다.

**Student**
```java
package ref;

public class Student {
  String name;
  int age;
  int grade;
}
```
* `ref` 패키지에도 `Student` 클래스를 만듭니다.

**Method1**
```java
package ref;

public class Method1 {

  public static void main(String[] args) {
    Student student1 = new Student();
    initStudent(student1, "학생1", 15, 90);

    Student student2 = new Student();
    initStudent(student2, "학생2", 16, 80);

    printStudent(student1);
    printStudent(student2);

  }

  static void initStudent(Student student, String name, int age, int grade) {
    student.name = name;
    student.age = age;
    student.grade = grade;
  }

  static void printStudent(Student student) {
    System.out.println("이름:" + student.name + " 나이:" + student.age + " 성적:" + student.grade);
  }
}
```

참조형은 메서드를 호출할 때 참조값을 전달합니다.
따라서 메서드 내부에서 전달된 참조값을 통해 객체의 값을 변경하거나, 값을 읽어서 사용할 수 있습니다.
* `initStudent(Student student, ...)` : 전달한 학생 객체의 필드에 값을 설정합니다.
* `printStudent(Student student, ...)` : 전달한 학생 객체의 필드 값을 읽어서 출력합니다.

**initStudent() 메서드 호출 분석**

```java
initStudent(Student student, String name, int age, int grade) {
    student.name = name;
    student.age = age;
    student.grade = grade;
}
```

* `student1`이 참조하는 `Student` 인스턴스에 값을 편리하게 할당하고 싶어서 `initStudent()` 메서드를 만들었습니다.
    * 이 메서드를 호출하면서 `student1`을 전달합니다.
        * 그러면 `student1`의 참조값(30f39991)이 매개변수 `student`에 전달됩니다.
            * 이 참조값을 통해 `initStudent()` 메서드 안에서 `student1`이 참조하는 것과 동일한 `30f39991` `Student` 인스턴스에 접근하고 값을 변경할 수 있습니다.

> 참고: `30f39991`은 실제 `student1`의 참조값입니다.
> `System.out.println("student1 참조값 : " + student1);` 의 결과가 `student1 참조값 : ref.Student@30f39991` 이였습니다.

**주의!**

```java
package ref;

import class1 Student;

public class Method1 {
    ...
}
```
* `import class1.Student;`이 선언되어 있으면 안됩니다.
    * 이렇게 되면 `class1` 패키지에서 선언한 `Student`를 사용하게 됩니다.
        * 이 경우에는 접근 제어자 때문에 컴파일 오류가 발생합니다.
            * 만약 선언되어 있다면 삭제해야 합니다. 삭제하면 같은 패키지에 있는 `ref.Student`를 사용합니다.

## 메서드에서 객체 반환
**조금 더 코드를 리팩토링 시켜봅시다. 다음 코드에도 중복이 있습니다.**

```java
Student student1 = new Student();
initStudent(student1, "학생1", 15, 90);
    
Student student2 = new Student();
initStudent(student2, "학생2", 16, 80);
```

바로 객체를 생성하고, 초기값을 설정하는 부분입니다.
* 이렇게 2번 반복되는 부분을 하나로 합져봅시다.

**"다음과 같이 기존 코드를 변경해봅시다."**

**Method2**
```java
package ref;

public class Method2 {

  public static void main(String[] args) {
    Student student1 = createStudent("학생1", 15, 90);
    Student student2 = createStudent("학생2", 16, 80);

    printStudent(student1);
    printStudent(student2);
  }

  static Student createStudent(String name, int age, int grade) {
    Student student = new Student();
    student.name = name;
    student.age = age;
    student.grade = grade;
    return student;
  }

  static void printStudent(Student student) {
    System.out.println("이름:" + student.name + " 나이:" + student.age + " 성적:" + student.grade);
  }
}
```

`createStudent()` 라는 메서드를 만들고 객체를 생성하는 부분도 이 메서드안에 함께 포함했습니다.
"이제 이 메서드 하나로 객체의 생성과 초기값 설정을 모두 처리합니다."
* 그런데 메서드 안에서 객체를 생성했기 때문에 만들어진 객체를 메서드 밖에서 사용할 수 있게 돌려주어야 합니다.
    * 그래야 메서드 밖에서 이 객체를 사용할 수 있습니다.
        * 메서드는 호출 결과를 반환(`return`)을 할 수 있습니다.
            * 메서드의 반환 기능을 사용해서 만들어진 객체의 참조값을 메서드 밖으로 반환하면 됩니다.

**createStudent() 메서드 호출 분석**
```java
createStudent(String name, int age, int grade) {
    Student student = new Student();
    student.name = name;
    student.age = age;
    student.grade = grade;
    return student;
}
```
* 메서드 내부에서 인스턴스를 생성한 후에 참조값을 메서드 외부로 반환했습니다.
    * 이 참조값만 있으면 해당 인스턴스에 접근할 수 있습니다.
        * 여기서는 `student1`에 참조값을 보관하고 사용합니다.

**진행 과정**
```java
Student student1 = createStudent("학생1", 15, 90); // 메서드 호출후 결과 반환
Student student1 = student(30f39991); // 참조형인 student를 반환
Student student1 = 30f39991; // student의 참조값 대입
student1 = 30f39991;
```

* `createStudent()` 는 생성한 `Student` 인스턴스의 참조값을 반환합니다.
    * 이렇게 반환된 참조값을 `student1` 변수에 저장합니다.
        * 앞으로는 `student1`을 통해 `Student` 인스턴스를 사용할 수 있습니다.
