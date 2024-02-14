---
title: "☕️[Java] 메서드 리펙토링 - 입.출금"
tags:
    - Java
    - Programming Language
date: 2024-02-14 13:33:00
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

**MethodEx3**
```java
package method.ex;

public class MethodEx3 {

  public static void main(String[] args) {
    int balance = 10000;

    // 입금
    int depositAmount = 1000;
    balance += depositAmount;
    System.out.println(depositAmount + "원을 입급하였습니다. 현재 잔액: " + balance + "원");

    // 출금
    int withdrawAmount = 2000;
    if (balance >= withdrawAmount) {
      balance -= withdrawAmount;
      System.out.println(withdrawAmount + "원을 출금하였습니다. 현재 잔액: " + balance + "원");
    } else {
      System.out.println(withdrawAmount + "원을 출금하려 했으나 잔액이 부족합니다.");
    }
    System.out.println("최종 잔액: " + balance + "원");
  }
}
```

위 코드는 입금, 출금을 나타내는 코드입니다.
**"입금(deposit)"** 과 **"출금(withdraw)"** 을 메서드로 만들어서 리펙토링 해보겠습니다.

**MethodEx3Ref**
```java
package method.ex;

public class MethodEx3Ref {

  public static void main(String[] args) {
    int balance = 10000;
    
    balance = deposit(10000, 1000);
    balance = withdraw(balance, 2000);
    
    System.out.println("최종 잔액: " + balance + "원");
  }

  public static int deposit(int balance, int amount) {
    balance += amount;
    System.out.println(amount + "원을 입급하였습니다. 현재 잔액: " + balance + "원");
    return balance;
  }

  public static int withdraw(int balance, int amount) {
    if (balance >= amount) {
      balance -= amount;
      System.out.println(amount + "원을 출금하였습니다. 현재 잔액: " + balance + "원");
    } else {
      System.out.println(amount + "원을 출금하려 했으나 잔액이 부족합니다.");
    }
    return balance;
  }
}
```

위 코드는 리펙토링을 한 코드입니다.

* 리펙토링 결과를 보면 `main()`은 세세한 코드가 아니라 전체 구조를 한눈에 볼 수 있게 되었습니다.
    * **"쉽게 이야기해서 책의 목자를 보는 것 같습니다."**
        * 더 자세히 알고 싶으면 해당 메서드를 찾아서 들어가면 됩니다.
        * 그리고 입금과 출금 부분이 메서드로 명확하게 분리되었기 때문에 이후에 변경 사항이 발생하면 관련된 메서드만 수정하면 됩니다.
        * 특정 메서드로 수정 범위가 한정되기 때문에 더 유지보수 하기 좋습니다.
* **"이런 리펙토링을 메서드 추출(Extract Method)이라 합니다."**
    * 메서드를 재사용하는 목적이 아니어도 괜찮습니다. "**메서드를 적절하게 사용해서 분류하면 구조적으로 읽기 쉽고 유지보수 하기 좋은 코드를 만들 수 있습니다.**"
        * 그리고 "메서드의 이름 덕분에 프로그램을 더 읽기 좋게 만들 수 있습니다."

```java
// MethodEx3 내부코드에서의

// <=== 입금로직 
int depositAmount = 1000;
balance += depositAmount;
System.out.println(depositAmount + "원을 입급하였습니다. 현재 잔액: " + balance + "원");
// ===>

// <=== 출금로직
    int withdrawAmount = 2000;
    if (balance >= withdrawAmount) {
      balance -= withdrawAmount;
      System.out.println(withdrawAmount + "원을 출금하였습니다. 현재 잔액: " + balance + "원");
    } else {
      System.out.println(withdrawAmount + "원을 출금하려 했으나 잔액이 부족합니다.");
    }
// ===>
```
위 코드 조각에서 볼 수 있듯 `입금로직` 부분과 `출금로직` 부분을 **뽑아내어**

```java
// MethodEx3Ref 내부 메소드
    // 입금 메서드
  public static int deposit(int balance, int amount) {
    balance += amount;
    System.out.println(amount + "원을 입급하였습니다. 현재 잔액: " + balance + "원");
    return balance;
  }
    // 출금 메서드
  public static int withdraw(int balance, int amount) {
    if (balance >= amount) {
      balance -= amount;
      System.out.println(amount + "원을 출금하였습니다. 현재 잔액: " + balance + "원");
    } else {
      System.out.println(amount + "원을 출금하려 했으나 잔액이 부족합니다.");
    }
    return balance;
  }
```

위의 두 **deposit(입금)** 메서드와 **withdraw(출금)** 메서드를 만들었습니다.

* 이런 리펙토링을 **메서드 추출(Extract Method)** 이라 합니다.

메서드가 단순하게 코드를 재사용하고 여러 곳에서 같이 사용한다는 것을 떠나서, 구조적으로 우선 비슷한 것을 그룹화하거나 카테고리화하면 이미 그것만으로도 큰 효용가치가 있습니다.
* 그래서 관련된 코드를 모아서 하나로 딱 묶어두는 것 입니다.
    * 그러면 읽기도 좋고, 유지보수에도 좋습니다.
