---
title: "💾 [CS] 전략 패턴(Strategy pattern)"
tags:
    - CS
date: "2024-09-03"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 전략 패턴(Strategy pattern)

## 1️⃣ 전략 패턴(Strategy pattern)
- **전략 패턴(Strategy pattern)** 은 **정책 패턴(Policy pattern)** 이라고도 하며, 객채의 행위를 바꾸고 싶은 경우 **'직접'** 수정하지 않고 전략이라고 부르는 **'캡슐화한 알고리즘'** 을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴입니다.

<img src = "https://github.com/devKobe24/images2/blob/main/CS_IMG/cs-strategy-pattern.png?raw=true">

- 아래의 예시 코드는 우리가 어떤 것을 살 때 네이버페이, 카카오페이 등 다양한 방법으로 결제하듯이 어떤 아이템을 살 때 `LUNACard`로 사는 것과 `KAKAOCard`로 사는 것을 구현한 예제입니다.
    - 결제 방식의 **'전략'** 만 바꿔서 두 가지 방식으로 결제하는 것을 구현했습니다.

```java
// PaymentStrategy - interface

public interface PaymentStrategy {
	void pay(int amount);
}

// KAKAOCardStrategy

public class KAKAOCardStrategy implements PaymentStrategy{
	private String name;
	private String cardNumber;
	private String cvv;
	private String dateOfExpiry;

	public KAKAOCardStrategy(String name, String cardNumber, String cvv, String dateOfExpiry) {
		this.name = name;
		this.cardNumber = cardNumber;
		this.cvv = cvv;
		this.dateOfExpiry = dateOfExpiry;
	}

	@Override
	public void pay(int amount) {
		System.out.println(amount + " paid using KAKAOCard.");
	}
}

// LUNACardStrategy

public class LUNACardStrategy implements PaymentStrategy {
	private String emailId;
	private String password;

	public LUNACardStrategy(String emailId, String password) {
		this.emailId = emailId;
		this.password = password;
	}

	@Override
	public void pay(int amount) {
		System.out.println(amount + " paid using LUNACard");
	}
}

// Item

public class Item {
	private String name;
	private int price;
	public Item(String name, int price) {
		this.name = name;
		this.price = price;
	}

	public String getName() {
		return name;
	}

	public int getPrice() {
		return price;
	}
}

// ShoppingCart

import java.util.ArrayList;
import java.util.List;

public class ShoppingCart {
	List<Item> items;

	public ShoppingCart() {
		this.items = new ArrayList<>();
	}

	public void addItem(Item item) {
		this.items.add(item);
	}

	public void removeItem(Item item) {
		this.items.remove(item);
	}

	public int calculateTotal() {
		int sum = 0;
		for (Item item : items) {
			sum += item.getPrice();
		}
		return sum;
	}

	public void pay(PaymentStrategy pamentMethod) {
		int amount = calculateTotal();
		pamentMethod.pay(amount);
	}
}

// Main

import designPattern.strategy.Item;
import designPattern.strategy.KAKAOCardStrategy;
import designPattern.strategy.LUNACardStrategy;
import designPattern.strategy.ShoppingCart;

public class Main {

	public static void main(String[] args) {
		ShoppingCart cart = new ShoppingCart();

		Item A = new Item("A", 100);
		Item B = new Item("B", 300);

		cart.addItem(A);
		cart.addItem(B);

		// pay by LUNACard
		cart.pay(new LUNACardStrategy("kobe@google.com", "1234"));

		// pay by KAKAOCard
		cart.pay(new KAKAOCardStrategy("Minseong Kang", "123456789", "123", "12/01"));
	}
}
```

**실행 결과**

```bash
400 paid using LUNACard
400 paid using KAKAOCard.
```
- 위 코드는 쇼핑 카드에 아이템을 담아 **`LUNACard`** 또는 **`KAKAOCard`** 라는 두 개의 전략으로 결제하는 코드입니다.

> **용어 : 컨텍스트**
> 프로그래밍에서의 컨텍스트는 상황, 맥락, 문맥을 의미하며 개발자가 어떠한 작업을 완료하는 데 필요한 모든 관련 정보를 말합니다.
