---
title: "☕️[Java] 배열 도입"
tags:
    - Java
    - Programming Language
date: "2024-02-17"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 배열 도입

* **"배열"** 을 사용하면 <ins>특정 타입을 연속한 데이터 구조로 묶어서 편리하게 관리</ins>할 수 있습니다.
* `Student` 클래스를 사용한 변수들도 `Student` 타입이기 때문에 학생도 **"배열"** 을 사용해서 **"하나의 데이터 구조로 묶어서 관리"** 할 수 있습니다.
    * [클래스의 도입](https://www.devkobe24.com/2024/Java/2024-02-16-classIntroduction.html) 포스팅 예시 코드 참고 -> 학생 클래스.

`Student` 타입을 사용하는 **"배열"** 을 도입해봅시다.

```java
package class1;

public class ClassStart4 {

  public static void main(String[] args) {

    Student student1 = new Student();
    student1.name = "학생1";
    student1.age = 15;
    student1.grade = 90;

    Student student2 = new Student();
    student2.name = "학생2";
    student2.age = 16;
    student2.grade = 80;

    Student[] students = new Student[2];
    students[0] = student1;
    students[1] = student2;

    for (int i = 0; i < students.length; i++) {
      System.out.println("이름:" + students[i].name + " 나이:" + students[i].age + " 성적:" + students[i].grade);
    }
  }
}
```

**코드를 분석해 봅시다.**
```java
Student student1 = new Student();
student1.name = "학생1";
student1.age = 15;
student1.grade = 90;

Student student2 = new Student();
student2.name = "학생2";
student2.age = 16;
student2.grade = 80;
```

* `Student` 클래스를 기반으로 `student1`, `student2` 인스턴스를 생성합니다. 그리고 필요한 값을 채워둡니다.

## 배열에 참조값 대입

**이번에는 `Student`를 담을 수 있는 배열을 생성하고, 해당 배열에 `student1`, `student2` 인스턴스를 보관해봅시다.**

```java
Student[] students = new Student[2];
```

* `Student` 변수를 2개 보관할 수 있는 사이즈 2의 배열을 만듭니다.
* `Student` 타입의 변수는 `Student` 인스턴스의 **"참조값을 보관"** 합니다.
    * `Student` 배열의 각각의 항목도 **"`Student` 타입의 변수일 뿐"** 입니다. 따라서 **"`Student` 타입의 참조값을 보관"** 합니다.
        * `student1`, `student2` 변수를 생각해보면 `Student` 타입의 참조값을 보관합니다.
* 배열에는 아직 참조값을 대입하지 않았기 때문에 참조값이 없다는 의미의 `null` 값으로 초기화 됩니다.

**이제 배열에 객체를 보관해보겠습니다.**

```java
students[0] = student1;
students[1] = student2;

// 자바에서 대입은 항상 변수에 들어 있는 값을 복사합니다.
students[0] = x001;
students[1] = x002;
```

> 잊지 말자 자바의 대원칙: **"자바에서 대입은 항상 변수에 들어 있는 값을 복사한다."**

* `student1`, `student2`에는 참조값이 보관되어 있습니다.
    * 따라서 이 참조값이 배열에 저정됩니다.
    * 또는 `student1`, `student2`에 보관된 참조값을 읽고 복사해서 배열에 대입한다고 표현합니다.

이제 배열은 `x001`, `x002`의 참조값을 가집니다.
참조값을 가지고 있기 때문에 `x001(학생1)`, `x002(학생)`, `Student` 인스턴스에 모두 접근할 수 있습니다.

> 너무 중요해서 한 번더 강조합니다 잊지 말자 자바의 대원칙: **"자바에서 대입은 항상 변수에 들어 있는 값을 복사한다."**

```java
students[0] = student1;
students[1] = student2;

// 자바에서 대입은 항상 변수에 들어 있는 값을 복사합니다.
students[0] = x001;
students[1] = x002;
```

* 자바에서 변수의 대입(`=`)은 모두 변수에 들어있는 값을 복사해서 전달하는 것입니다.
    * 이 경우 오른쪽 변수인 `student1`, `student2`에는 참조값이 들어있습니다.
        * 그래서 이 값을 복사해서 왼쪽에 있는 배열에 전달합니다.
            * <ins>따라서 기존 `student1`, `student2`에 들어있던 참조값은 당연히 그대로 유지됩니다.</ins>

**주의!!**
* 변수에는 인스턴스 자체가 들어있는 것이 아닙니다!
    * **"인스턴스의 위치를 가리키는 참조값이 들어있을 뿐입니다!!"**
        * <ins>따라서 대입(`=`)시에 인스턴스가 복사되는 것이 아니라 참조값만 복사됩니다.</ins>

## 배열에 들어있는 객체 사용
배열에 들어있는 객체를 사용하려면 먼저 배열에 접근하고, 그 다음에 객체에 접근하면 됩니다.
이전에 설명한 그림과 코드를 함께 보면 쉽게 이해가 될 것입니다.

**학생1 예제**
```java
System.out.println(students[0].name); // 배열 접근 시작
System.out.println(x005[0].name); // [0]를 사용해서 x005 배열의 0번 요소에 접근
System.out.println(x001.name); // .(dot)을 사용해서 참조값으로 객체에 접근
System.out.println("학생1");
```

**학생2 예제**
```java
System.out.println(students[1].name); // 배열 접근 시작
System.out.println(x005[1].name); // [0]를 사용해서 x005 배열의 1번 요소에 접근
System.out.println(x002.name); // .(dot)을 사용해서 참조값으로 객체에 접근
System.out.println("학생2");
```
