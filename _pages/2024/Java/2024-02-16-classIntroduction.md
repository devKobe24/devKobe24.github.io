---
title: "☕️[Java] 클래스 도입"
tags:
    - Java
    - Programming Language
date: "2024-02-16"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 클래스 도입

클래스를 사용해서 "학생"이라는 개념을 만들고, 각 학생 별로 본인의 이름, 나이, 성적을 관리해봅시다.

우선 코드를 봐봅시다.

```java
package class1;

public class Student {
  String name;
  int age;
  int grade;
}
```

* `class` 키워드를 사용해서 학생 클래스(`Student`)를 정의합니다.
    * 학생 클래스는 내부에 이름(`name`), 나이(`age`), 성적(`grade`) 변수를 가집니다.

이렇게 **"클래스에 정의한 변수들을 멤버 변수, 또는 필드"** 라 합니다.
* **멤버 변수(Member Variable) :** 이 변수들은 특정 클래스에 소속된 멤버이기 때문에 이렇게 부릅니다.
* **필드(Field) :** 데이터 항목을 가르키는 전통적인 용어입니다. 데이터베이스, 엑셀 등에서 데이터 각각의 항목을 필드라고 합니다.
* 자바에서 멤버 변수, 필드는 같은 뜻입니다. 
    * 클래스에 소속된 변수를 뜻합니다.

**클래스는 관례상 대문자로 시작하고 낙타 표기법을 사용합니다.**
* 예) `Student`, `User`, `MemberService`

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

**실행 결과**
```
이름:학생1 나이:15 성적:90
이름:학생2 나이:16 성적:80
```

**클래스와 사용자 정의 타입**
* **"타입"** 은 데이터의 종류나 형태를 나타냅니다.
    * `int`라고 하면 정수 타입, `String`이라고 하면 문자 타입입니다.
    * 그러면 학생(`Student`)이라는 타입을 만들면 되지 않을까요?
* 클래스를 사용하면 `int`, `String`과 같은 타입을 직접 만들 수 있습니다.
    * 사용자가 직접 정의하는 **"사용자 정의 타입"** 을 만들려면 설계도가 필요합니다.
        * 이 **"이 설계도가 바로 클래스"** 입니다.
* "설계도"인 "클래스"를 사용해서 **"실제 메모리에 만들어진 실체를 객체 또는 인스턴스"** 라 합니다.
* "클래스"를 통해서 사용자가 원하는 종류의 데이터 타입을 마음껏 정의할 수 있습니다.

**용어: 클래스, 객체, 인스턴스**
* 클래스는 설계도이고, 이 설계도를 기반으로 실제 메모리에 만들어진 실체를 **"객체 또는 인스턴스"** 라 합니다.
    * 둘다 같은 의미로 사용됩니다.
        * 여기서는 학생(`Student`) 클래스를 기반으로 학생1(`student1`), 학생2(`student2`) 객체 또는 인스턴스를 만들었습니다.

## 코드 분석

**1. 변수 선언.**
```java
Student student1 // Student 변수 선언
```
* `Student student1`
    * `Student` **"타입"** 을 받을 수 있는 변수를 선언합니다.
    * `int`는 정수를, `String`은 문자를 담을 수 있듯이 <ins>"`Student`는 `Student` 타입의 객체(인스턴스)를 받을 수 있습니다."</ins>

**2. 객체 생셩.**
```java
student1 = new Student() // Student 인스턴스 생성
```

`student1 = new Student()` 코드를 나누어 분석해봅시다.
* 객체를 사용하려면 먼저 설계도인 클래스를 기반으로 객체(인스턴스)를 생성해야 합니다.
* `new Student()`에서의 `new`는 새로 생성한다는 뜻 입니다.
    * `new Student()`는 `Student` 클래스 정보를 기반으로 새로운 객체를 생성하라는 뜻입니다.
    * 이렇게 하면 메모리에 실제 `Student` 객체(인스턴스)를 생성합니다.
* 객체를 생성할 때는 `new 클래스명()`을 사용하면 됩니다. 마지막에 `()`도 추가해야합니다.
* `Student` 클래스는 `String name`, `int age`, `int grade` 멤버 변수를 가지고 있습니다.
    * 이 변수를 사용하는데 필요한 메모리 공간도 함께 확보합니다.

**3. 참조값 보관**
```java
student1 = x001l // Student 인스턴스 참조값 보관
```
* 객체를 생성시 자바는 메모리 어딘가에 있는 이 객체에 접근할 수 있는 참조값(주소)(`x001`)을 반환합니다.
    * 여기셔 `x001`이라고 표현한 것이 참조값입니다.(실제로 `x001`처럼 표현되는 것은 아니고 이해를 돕기 위한 예시입니다.)
* `new` 키워드를 통해 객체가 생성되고 나면 참조값을 반환합니다.
    * 앞서 선언한 변수인 `Student student1`에 생성된 객체의 참조값(`x001`)을 보관합니다.
* `Student student1` 변수는 이제 메모리에 존재하는 실제 `Student` 객체(인스턴스)의 참조값을 가지고 있습니다.
    * `student1` 변수는 방금 만든 객체에 접근할 수 있는 참조값을 가지고 있습니다.
        * 따라서 이 변수를 통해서 객체를 접근(참조)할 수 있습니다. 쉽게 이야기해서 `student1` 변수를 통해 메모리에 있는 실제 객체를 접근하고 사용할 수 있습니다.

**참조값을 변수에 보관해야 하는 이유**
* 객체를 생성하는 `new Student()` 코드 자체에는 아무런 이름이 없습니다. 
    * **"이 코드는 단순히 `Student` 클래스를 기반으로 메모리에 실제 객체를 만드는 것 입니다."**
        * 따라서 생성한 객체에 접근할 수 있는 방법이 필요합니다.
            * 이런 이유로 객체를 생성할 때 반환되는 참조값을 어딘가에 보관해두어야 합니다.
* 앞서 `Student student1` 변수에 참조값(`x001`)을 저장해두었으므로 저장한 참조값(`x001`)을 통해서 실제 메모리에 존재하는 객체에 접근할 수 있습니다.

지금까지 설명한 내용을 간단히 풀어보면 다음과 같습니다.
```java
Student student1 = new Student(); // 1. Student 객체 생성
Student student1 = x001; // 2. new Student()의 결과로 x001 참조값 변환
student1 = x001; // 3. 최종 결과
```

이후에 학생2(`student2`)까지 생성하면 다음과 같이 <ins>`Student` 객체(인스턴스)가 메모리에 2개 생성됩니다.</ins>
* **"각각의 참조값이 다르므로 서로 구분할 수 있습니다."**

참조값을 확인하고 싶다면 다음과 같이 객체를 담고 있는 변수를 출력해보면 됩니다.
```java
System.out.println(student1);
System.out.println(student2);
```

**출력 결과**
```
class1.Student@30f39991
class1.Student@452b3a41
```
* `@` 앞은 패키지 + 클래스 정보를 뜻합니다. `@` 뒤에 <ins>16진수는 참조값을 뜻합니다.</ins>
