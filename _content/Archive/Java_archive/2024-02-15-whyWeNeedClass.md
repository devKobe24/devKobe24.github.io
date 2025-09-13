---
title: "☕️[Java] 클래스가 필요한 이유."
tags:
    - Java
    - Programming Language
date: "2024-02-15"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---
# 클래스가 필요한 이유.

먼저 아래의 **학생 정보 출력 프로그램**을 만들어 봅시다.

이 프로그램은 두 명의 학생 정보를 출력하는 프로그램입니다.
각 학생은 이름, 나이, 성적을 가지고 있습니다.

**요구사항**
1. 첫 번째 학생의 이름은 "학생1", 나이는 15, 성적은 90 입니다.
2. 두 번째 학생의 이름은 "학생2", 나이는 16, 성적은 80 입니다.
3. 각 학생의 정보를 다음과 같은 형식으로 출력해야 합니다: `"이름: [이름] 나이: [나이] 성적: [성적]"`
4. 변수를 사용해서 학생 정보를 저장하고 변수를 사용해서 학생 정보를 출력해야 합니다.

**예시 출력**
```
이름: 학생1 나이: 15 성적: 90
이름: 학생2 나이: 16 성적: 80
```

변수를 사용해서 이 문제를 풀어보면 다음과 같이 프로그램을 만들어볼 수 있습니다.

```java
package class1;

public class ClassStart1 {

  public static void main(String[] args) {
    String firstStudentName = "학생1";
    String secondStudentName = "학생2";

    int firstStudentAge = 15;
    int secondStudentAge = 16;

    int firstStudentGrade = 90;
    int secondStudentGrade = 80;

    System.out.println("이름: " + firstStudentName + " 나이: " + firstStudentAge + " 성적: " + firstStudentGrade);
    System.out.println("이름: " + secondStudentName + " 나이: " + secondStudentAge + " 성적: " + secondStudentGrade);
  }
}
```

* 위 코드에서는 학생 2명을 다루어야 하기 때문에 각각 다른 변수를 사용했습니다.
* 이 코드의 문제는 학생이 늘어날 때 마다 변수를 추가로 선언해야 하고, 또 출력하는 코드도 추가해야 합니다.

**이런 문제를 어떻게 해결할 수 있을까요?**

이번에는 위 코드를 **"배열을 사용하여 리펙토링"** 해봅시다.

아래의 코드는 **"배열을 활용한 코드"** 입니다.

```java
package class1;

public class ClassStart2 {

  public static void main(String[] args) {
    String[] studentNames = { "학생1", "학생2" };
    int [] studentAges = { 15, 16 };
    int [] studentGrades = { 90, 80 };

    for (int i = 0; i < studentNames.length; i++) {
      System.out.println("이름: " + studentNames[i] + " 나이: " + studentAges[i] + " 성적: " + studentGrades[i]);
    }
  }
}
```

이전 코드보다 훨씬 깔끔해졌으며, 위 코드에서 문제가 됐었던 학생이 늘어날 때 마다 변수를 새롭게 추가할 필요도 없어졌습니다.

## 배열 사용의 한계.

**하지만** 배열을 사용해서 코드를 최소화하는데 성공했지만 **"한 한생의 데이터가 `studentNames[]`, `studentAges[]`, `studentGrades`라는 3개의 배열에 나누어져 있습니다."**
* 따라서 **"데이터를 변경할 때 매우 조심해서 작업해야 합니다."**
    * 예를 들어서 학생 2의 데이터를 제거하려면 각각의 배열마다 학생2의 요소를 정확하게 찾아서 제거해주어야 합니다.

**학생2 제거**
```java
String[] studentNames = { "학생1", "학생3", "학생4", "학생5" };
int [] studentAges = { 15, 17, 10, 16 };
int [] studentGrades = { 90, 100, 80, 50 };
```

* 한 학생의 데이터가 3개의 배열에 나누어져 있기 때문에 3개의 배열을 각각 변경해야 합니다!!
    * 그리고 한 학생의 데이터를 관리하기 위해 3개의 배열의 인덱스 순서를 항상 정확하게 맞추어야 합니다.(조금이라도 실수하면 😱)
        * 이렇게 하면 특정 학생의 데이터를 변경시 실수할 가능성이 매우 높습니다.

이 코드는 컴퓨터가 볼 때는 아무 문제가 없지만, 사람이 관리하기에는 좋은 코드가 아닙니다. 😵‍💫

**정리**

* 지금처럼 이름, 나이, 성적을 각각 따로 나누어서 관리하는 것은 사람이 관리하기 좋은 방식이 아닙니다.
* 사람이 관리하기 좋은 방식은 학생이라는 개념을 하나로 묶는 것입니다.
    * 그리고 각각의 학생 별로 본인의 이름, 나이, 성적을 관리하는 것 입니다.
