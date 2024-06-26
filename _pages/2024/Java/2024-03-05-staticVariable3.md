---
title: "☕️[Java] static 변수3"
tags:
    - Java
    - Programming Language
date: "2024-03-05"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# static 변수 3

이번에는 `static` 변수를 정리해보겠습니다.

## 용어 정리
```java
public class Data3 {
    public String name;
    public static int count; // static
}
```

* 예제 코드에서 `name`, `count`는 둘 다 멤버 변수입니다.

멤버 변수(필드)는 `static`이 붙은 것과 아닌 것에 따라 다음과 같이 분류 할 수 있습니다.

### 멤버 변수(필드)의 종류.
* **인스턴스 변수**: `static`이 붙지 않은 멤버 변수, 예) `name`
    * `static` 이 붙지 않은 멤버 변수는 인스턴스를 생성해야 사용할 수 있고, 인스턴스에 소속되어 있습니다.
        * 따라서 인스턴스 변수라 합니다.
    * 인스턴스 변수는 인스턴스를 만들 때 마다 새로 만들어집니다.
* **클래스 변수**: `static`이 붙은 멤버 변수, 예) `count`
    * 클래스 변수, 정적 변수, `static` 변수등으로 부릅니다. **용어를 모두 사용하니 주의합시다.**
    * `static`이 붙은 멤버 변수는 인스턴스와 무관하게 클래스에 바로 접근해서 사용할 수 있고, 클래스 자체에 소속되어 있습니다.
        * 따라서 클래스 변수라 합니다.
    * 클래스 변수는 자바 프로그램을 시작할 때 딱 1개가 만들어집니다.
        * 인스턴스와는 다른게 보통 여러곳에서 공유하는 목적으로 사용됩니다.

## 변수와 생명주기
* **지역 변수(매개변수 포함)**: 지역 변수는 스택 영역에 있는 스택 프레임 안에 보관됩니다. 메서드가 종료되면 스택 프레임도 제거 되는데 이때 해당 스택 프레임에 포함된 지역 변수도 함께 제거됩니다.
    * 따라서 지역 변수는 생존 주기가 짧습니다.
* **인스턴스 변수**: 인스턴스에 있는 멤버 변수를 인스턴스 변수라 합니다. 인스턴스 변수는 힙 영역을 사용합니다.
    * 힙 영역은 GC(가비지 컬렉션)가 발생하기 전까지 생존하기 때문에 보통 지역 변수보다 생존 주기가 깁니다.
* **클래스 변수**: 클래스 변수는 메서드 영역의 static 영역에 보관 되는 변수입니다.
    * 메서드 영역은 프로그램 전체에서 사용하는 공용 공간입니다.
    * 클래스 변수는 해당 클래스가 JVM에 로딩 되는 순간 생성됩니다.
        * 그리고 JVM이 종료될 때까지 생명주기가 이어집니다.
            * 따라서 가장 긴 생명주기를 가집니다.

`static`이 정적이라는 이유는 바로 여기에 있습니다.
힙 역역에 생성되는 인스턴스 변수는 동적으로 생성되고, 제거됩니다.
반면에 `static`인 정적 변수는 거의 프로그램 실행 시점에 딱 만들어지고, 프로그램 종료 시점에 제거됩니다.
**정적 변수는 이름 그대로 정적입니다.**

## 정적 변수 접근 법
`static` 변수는 클래스를 통해 바로 접근할 수도 있고, 인스턴스를 통해서도 접근할 수 있습니다.
`DataCountMain3` 마지막 코드에 다음 부분을 추가하고 실행해보겠습니다.

**DataCountMain3 - 추가**
```java
// 추가
// 인스턴스를 통한 접근
Data3 data4 = new Data3("D");

// 클래스를 통합 접근
System.out.println(Data3.count);
```

**실행 결과 - 추가된 부분**
```
4
4
```
* 둘 다 차이는 없습니다. 둘다 결과적으로 정적 변수에 접근합니다.

**인스턴스를 통한 접근 `data4.count`**
* 정적 변수의 경우 인스턴스를 통한 접근은 추천하지 않습니다.
    * 왜냐하면 코드를 읽을 때 마치 인스턴스 변수에 접근하는 것 처럼 오해할 수 있기 때문입니다.

**클래스를 통한 접근 `Data3.count`**
* 정적 변수는 클래스에서 공용으로 관리하기 때문에 클래스를 통해서 접근하는 것이 더 명확합니다.
    * 따라서 정적 변수에 접근할 때는 클래스를 통해서 접근합시다.
