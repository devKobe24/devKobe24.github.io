---
title: "☕️[Java] 패키지 - import"
tags:
    - Java
    - Programming Language
date: "2024-02-25"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 패키지 - import

## import

```java
package pack;

public class PackageMain1 {

  public static void main(String[] args) {
   Data data = new Data();
   pack.a.User user = new pack.a.User();
  }
}
```
* 위 코드와 같이 패키지가 다르다고 `pack.a.User`와 같이 항상 전체 경로를 적어주는 것은 불편합니다.
    * **"이때는 `import`를 사용하면 됩니다."**

**PackageMain2**
```javascript
package pack;

import pack.a.User;

public class PackageMain2 {

  public static void main(String[] args) {
   Data data = new Data();
   User user = new User();
  }
}
```

**실행 결과**
```
패키지 pack Data 생성
패키지 pack.a 회원 생성
```

* 코드에서 첫줄에는 `package`를 사용하고, 다음 줄에는 `import`를 사용할 수 있습니다.
    * `import`를 사용하면 다른 패키지에 있는 클래스를 가져와서 사용할 수 있습니다.
        * `import`를 사용한 덕분에 코드에서는 패키지 명을 생략하고 클래스 이름만 적을 수 있습니다.

> 참고로 특정 패키지에 포함된 모든 클래스를 포함해서 사용하고 싶으면 `import` 시점에 `*(별)`을 사용하면 됩니다.

**패키지 별(`*`)사용**

**tree 구조**
```
* pack -> [Package]
├──* Data -> (Class)
├──* PackageMain1 -> (Class)
├──* PackageMain1 -> (Class)
└──* a -> [Package]
   ├──* User -> (Class)
   └──* UserManager -> (Class)
```

```java
package pack;

import pack.a.*; // pack.a의 모든 클래스 사용

public class PackageMain2 {

  public static void main(String[] args) {
   Data data = new Data();
   User user = new User(); // import 사용으로 패키지 명 생략 가능
   UserManager manager = new UserManager(); // import 사용으로 패키지 명 생략 가능
  }
}
```
* 이렇게 하면 `pack.a` 패키지에 있는 모든 클래스를 패키지 명을 생략하고 사용할 수 있습니다.

**실행 결과**
```
패키지 pack Data 생성
패키지 pack.a 회원 생성
패키지 pack.a 회원 매니저 생성
```

## 클래스 이름 중복
패키지 덕분에 클래스 이름이 같아도 패키지 이름으로 구분해서 같은 이름의 클래스를 사용할 수 있습니다.

```
pack.a.User
pack.b.User
```

**"이런 경우 클래스 이름이 둘다 `User`이지만 패키지 이름으로 대상을 구분할 수 있습니다."**
* 이렇게 이름이 같은 경우 둘다 사용하고 싶으면 어떻게 해야할까요?

**pack.b.User**
```java
package pack.b;

public class User {
  public  User() {
    System.out.println("패키지 pack.b 회원 생성");
  }
}
```

**PackageMain3**
```java
package pack;

import pack.a.User;

public class PackageMain3 {

  public static void main(String[] args) {
    User userA = new User();
    pack.b.User userB = new pack.b.User();
  }
}
```

* 같은 이름의 클래스가 있다면 `import`는 둘중 하나만 선택할 수 있습니다.
    * 이때는 자주 사용하는 클래스를 `import`하고 나머지를 패키지를 포함한 전체 경로를 적어주면 됩니다.
        * 물론 둘다 전체 경로를 적어준다면 `import`를 사용하지 않아도 됩니다.
