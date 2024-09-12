---
title: "☕️[Java] 캡슐화"
tags:
    - Java
    - Programming Language
date: "2024-03-01"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 캡슐화

* 캡슐화(Encapsulation)는 객체 지향 프로그래밍의 중요한 개념 중 하나입니다.
* 캡슐화는 데이터와 해당 데이터를 처리하는 메서드를 하나로 묶어서 외부에서의 접근을 제한하는 것을 말합니다.
* 캡슐화를 통해 데이터의 직접적인 변경을 방지하거나 제한할 수 있습니다.
* 캡슐화는 쉽게 이야기해서 속성과 기능을 하나로 묶고, 외부에 꼭 필요한 기능만 노출하고 너머지는 모두 내부로 숨기는 것입니다.

**이전에 객체 지향 프로그래밍을 설명하면서 캡슐화에 대해 알아보았습니다.**
* 이때는 데이터와 데이터를 처리하는 메서드를 하나로 모으는 것에 초점을 맞추었습니다.
    * 여기서 한발짝 더 나아가 캡슐화를 안전하게 완성할 수 있게 해주는 장치가 바로 **"접근 제어자"** 입니다.

**그럼 어떤 것을 숨기고 어떤 것을 노출해야 할까요?**

## 1. 데이터를 숨겨라.
객체에는 속성(데이터)과 기능(메서드)이 있습니다.
* 캡슐화에서 가장 필수로 숨겨야 하는 것은 **"속성(데이터)"** 입니다.

```java
package access;

public class Speaker {
  private int volume;

  Speaker(int volume) {
    this.volume = volume;
  }

  void volumeUp() {
    if (volume >= 100) {
      System.out.println("음량을 증가할 수 없습니다. 최대 음량입니다.");
    } else {
      volume += 10;
      System.out.println("음량을 증가합니다.");
    }
  }

  void volumeDown() {
    volume -= 10;
    System.out.println("volumeDown 호출");
  }

  void showVolume() {
    System.out.println("현재 음량: " + volume);
  }
}
```

위 코드에서의 `Speaker`의 `volume`을 봐봅시다.
* 객체 내부의 데이터를 외부에서 함부로 접근하게 둔다면, 클래스 안에서 데이터를 다루는 모든 로직을 무시하고 데이터를 변경할 수 있습니다.
    * 결국 모든 안전망을 다 빠져나가게 됩니다.
        * **따라서 캡슐화가 깨집니다.**

우리가 자동자를 운전할 때 자동차 부품을 다 열어서 그 안에 있는 속도계를 직접 조절하지 않습니다.
* 단지 자동차가 제공하는 액셀 기능을 사용해서 액셀을 밟으면 자동차가 나머지는 다 알아서 하는 것입니다.

우리가 일상에서 생각할 수 있는 음악 플레이어를 떠올려봅시다.

음악 플레이어를 사용할 때 그 내부에 들어있는 전원부나, 볼륨 상태의 데이터를 직접 수정할 일이 있을까요?

우리는 그냥 음악 플레이어의 켜고, 끄고, 볼륨을 조절하는 버튼을 누를 뿐입니다.
* 그 내부에 있는 전원부나, 볼륨의 조절하는 버튼을 누를 뿐입니다.
    * 그 내부에 있는 전원부나, 볼륨의 상태 데이터를 직접 수정하지는 않습니다.
        * 전원 버튼을 눌렀을 때 실제 전원을 받아서 전원을 켜는 것은 음악 플레이어의 일입니다.
        * 볼륨을 높였을 때 내부에 있는 볼륨 장치들을 움직이고 볼륨 수치를 조절하는 것도 음악 플레이어가 스스로 해야하는 일입니다.
            * 쉽게 이야기해서 우리는 음악 플레이어가 제공하는 기능을 통해서 음악 플레이어를 사용하는 것입니다.
                * 복잡하게 음악 플레이어의 내부를 까서 그 내부 데이터까지 우리가 직접 사용하는 것은 아닙니다.

**객체의 데이터는 객체가 제공하는 기능인 메서드를 통해서 접근해야 합니다.**

## 2. 기능을 숨겨라.
**"객체의 기능 중에서 외부에서 사용하지 않고 내부에서만 사용하는 기능들이 있습니다."**
* 이런 기능도 모두 감추는 것이 좋습니다.

우리가 자동차를 운정하기 위해 자동차가 제공하는 복잡한 엔진 조절 기능, 배기 기능까지 우리는 알 필요가 없습니다.
* 우리는 단지 렉셀과 핸들 정도의 기능만 알면 됩니다.
    * 만약 사용자에게 이런 기능까지 모두 알려준다면, 사용자가 자동차에 대해 너무 많은 것을 알아야 합니다.

**"사용자 입장에서 꼭 필요한 기능만 외부에 노출합시다. 나머지 기능은 모두 내부로 숨깁시다."**

**"정리하면 데이터는 모두 숨기고, 기능은 꼭 필요한 기능만 노출하는 것이 좋은 캡슐화입니다."**

이번에는 잘 캡슐화된 예제를 하나 만들어봅시다.

**BankAccount**
```java
package access;

public class BankAccount {

  private int balance;

  public BankAccount() {
    balance = 0;
  }

  // public 메서드: deposit
  public void deposit(int amount) {
    if (isAmountValid(amount)) {
      balance += amount;
      System.out.println(amount + "원이 입금되었습니다.");
    } else {
      System.out.println("유효하지 않은 금액입니다.");
    }
  }

  // public 메서드: withdraw
  public void withdraw(int amount) {
    if (isAmountValid(amount) && (balance - amount >= 0)) {
      balance -= amount;
      System.out.println(amount + "원이 출금되었습니다.");
    } else {
      System.out.println("유효하지 않은 금액이거나 잔액이 부족합니다.");
    }
  }

  // public 메서드: getBalance
  public int getBalance() {
    return balance;
  }

  private boolean isAmountValid(int amount) {
    // 금액이 0보다 커야함.
    return amount > 0;
  }
}
```

**BankAccountMain**
```java
package access;

public class BankAccountMain {

  public static void main(String[] args) {
    BankAccount account = new BankAccount();
    account.deposit(10000);
    account.withdraw(3000);
    System.out.println("balance = " + account.getBalance());
  }
}
```

위 코드는 은행 계좌 기능을 다룹니다.

위 코드는 다음과 같은 기능을 가지고 있습니다.

**private**
* `balance` : 데이터 필드는 외부에 직접 노출하지 않습니다. `BankAccount`가 제공하는 메서드를 통해서만 접근할 수 있슷빈다.
* `isAmountValid()` : 입력 금액을 검증하는 기능은 내부에서만 필요한 기능입니다. 따라서 `private`을 사용했습니다.

**pulbic**
* `deposit()` : 입금
* `withdraw()` : 출금
* `getBalance()` : 잔고

**`BankAccount`를 사용하는 입장에서는 단 3가지 메서드만 알면 됩니다.**
* 나머지 복잡한 내용은 모두 `BankAccount` 내부에 숨어있습니다.

**"만약 `isAmountValid()`를 외부에 노출할 경우 어떻게될까요?"**
* `BankAccount`를 사용하는 개발자 입장에서는 사용할 수 있는 메서드가 하나 더 늘었습니다.
    * 여러분이 `BankAccount`를 사용하는 개발자라면 어떤 생각을 할까요?
        * 아마도 입금과 출금 전에 본인이 먼저 `isAmountValid()`를 사용해서 검증을 해야 하나? 라고 의문을 가질 것입니다.

**"만약 `balance` 필드를 외부에 노출하면 어떻게 될까요?"**
* `BankAccount`를 사용하는 개발자 입장에서는 이 필드를 직접 사용해도 된다고 생각할 수 있습니다.
    * **"왜냐하면 외부에 공개하는 것은 그것을 외부에서 사용해도 된다는 뜻이기 때문입니다."**
        * **"결국 모든 검증과 캡슐화가 깨지고 잔고를 무한정 늘리고 출금하는 심각한 문제가 발생할 수 있습니다."**

**"접근 제어자와 캡슐화를 통해 데이터를 안전하게 보호하는 것은 물론이고, `BankAccount`를 사용하는 개발자 입장에서 해당 기능을 사용하는 복잡도도 낮출 수 있습니다."**
