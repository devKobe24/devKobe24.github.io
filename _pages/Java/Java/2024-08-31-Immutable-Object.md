---
title: "☕️[Java] 불변 객체 - 예제"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-08-31"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] 불변 객체 - 예제

- 조금 더 복잡하고 의미있는 예제를 통해서 불변 객체의 사용 예를 확인해봅시다.

## 1️⃣ Address, ImmutableAddress 코드.

```java
// Address
public class Address {
	private String value;

	public Address(String value) {
		this.value = value;
	}

	public String getValue() {
		return value;
	}

	public void setValue(String value) {
		this.value = value;
	}

	@Override
	public String toString() {
		return "Address{" +
				"value='" + value + '\'' +
				'}';
	}
}

// ImmutableAddress
public class ImmutableAddress {
	private final String value;

	public ImmutableAddress(String value) {
		this.value = value;
	}

	public String getValue() {
		return value;
	}

	@Override
	public String toString() {
		return "Address{" +
				"value='" + value + '\'' +
				'}';
	}
}
```

## 2️⃣ 변경 클래스 사용.

```java
// MemberV1
public class MemberV1 {
	private String name;
	private Address address;

	public MemberV1(String name, Address address) {
		this.name = name;
		this.address = address;
	}

	public Address getAddress() {
		return address;
	}

	public void setAddress(Address address) {
		this.address = address;
	}

	@Override
	public String toString() {
		return "MemberV1{" +
				"name='" + name + '\'' +
				", address=" + address +
				'}';
	}
}

// MemberMainV1
public class MemberMainV1 {

	public static void main(String[] args) {
		Address address = new Address("서울");

		MemberV1 memberA = new MemberV1("회원A", address);
		MemberV1 memberB = new MemberV1("회원B", address);

		// 회원A, 회원B의 처음 주소는 모두 서울
		System.out.println("memberA = " + memberA);
		System.out.println("memberB = " + memberB);

		// 회원B의 주소를 부산으로 변경해야함
		memberB.getAddress().setValue("부산");
		System.out.println("부산 -> memberB.address");
		System.out.println("memberA = " + memberA);
		System.out.println("memberB = " + memberB);
	}
}
```

- **`회원A`** 와 **`회원B`** 는 둘 다 서울에 살고 있습니다.
- 중간에 **`회원B`** 의 주소를 부산으로 변경해야 합니다.
- 그런데 **`회원A`** 와 **`회원B`** 는 **`Address`** 인스턴스를 참조하고 있습니다.
- **`회원B`** 의 주소를 부산으로 변경하는 순간 **`회원A`** 의 주소도 부산으로 변경됩니다.

### 실행 결과
```bash
memberA = MemberV1{name='회원A', address=Address{value='서울'}}
memberB = MemberV1{name='회원B', address=Address{value='서울'}}
부산 -> memberB.address
memberA = MemberV1{name='회원A', address=Address{value='부산'}}
memberB = MemberV1{name='회원B', address=Address{value='부산'}}
```

## 3️⃣ 불변 클래스 사용.

```java
// MemberV2
public class MemberV2 {
	private String name;
	private ImmutableAddress address;

	public MemberV2(String name, ImmutableAddress address) {
		this.name = name;
		this.address = address;
	}

	public ImmutableAddress getAddress() {
		return address;
	}

	public void setAddress(ImmutableAddress address) {
		this.address = address;
	}

	@Override
	public String toString() {
		return "MemberV1{" +
				"name='" + name + '\'' +
				", address=" + address +
				'}';
	}
}
```

- **`MemberV2`** 는 주소를 변경할 수 없는, 불변인 **`ImmutableAddress`** 를 사용합니다.

```java
// MemberMainV2
public class MemberMainV2 {

	public static void main(String[] args) {
		ImmutableAddress address = new ImmutableAddress("서울");

		MemberV2 memberA = new MemberV2("회원A", address);
		MemberV2 memberB = new MemberV2("회원B", address);

		// 회원A, 회원B의 처음 주소는 모두 서울
		System.out.println("memberA = " + memberA);
		System.out.println("memberB = " + memberB);

		// 회원B의 주소를 부산으로 변경해야함
		// memberB.getAddress().setValue("부산"); // 컴파일 오류
		memberB.setAddress(new ImmutableAddress("부산"));
		System.out.println("부산 -> memberB.address");
		System.out.println("memberA = " + memberA);
		System.out.println("memberB = " + memberB);
	}
}
```

- **`회원B`** 의 주소를 중간에 부산으로 변경하려고 시도합니다.
    - 하지만 **`ImmutableAddress`** 에는 값을 변경할 수 있는 메서드가 없습니다.
        - 따라서 컴파일 오류가 발생합니다.
            - 결국 **`memberB.setAddress(new ImmutableAddress("부산"))`** 와 같이 새로운 주소 객체를 만들어서 전달합니다.

### 실행 결과

```bash
memberA = MemberV1{name='회원A', address=Address{value='서울'}}
memberB = MemberV1{name='회원B', address=Address{value='서울'}}
부산 -> memberB.address
memberA = MemberV1{name='회원A', address=Address{value='서울'}}
memberB = MemberV1{name='회원B', address=Address{value='부산'}}
```
- 사이드 이펙트가 발생하지 않습니다. **`회원A`** 는 기존 주소를 그대로 유지합니다.
