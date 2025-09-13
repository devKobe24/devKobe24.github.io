---
title: "☕️[Java] 래퍼 클래스 - 기본형의 한계(2)"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-09-13"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] 래퍼 클래스 - 기본형의 한계(2)

## 1️⃣ 기본형과 null.
- 기본형(Primitive Type)은 항상 값을 가져야 합니다.
    - 하지만 때로는 데이터가 '없음'이라는 상태가 필요할 수 있습니다.

```java
public class MyIntegerNullMain0 {

	public static void main(String[] args) {
		int[] intArr = {-1, 0, 1, 2, 3};
		System.out.println(findValue(intArr, -1)); // -1
		System.out.println(findValue(intArr, 0));
		System.out.println(findValue(intArr, 1));
		System.out.println(findValue(intArr, 100)); // -1

	}

	private static int findValue(int[] intArr, int target) {
		for (int value : intArr) {
			if (value == target) {
				return value;
			}
		}
		return -1;
	}
}
```

- `findValue()`는 배열에 찾는 값이 있으면 해당 값을 반환하고, **찾는 값이 없으면 `-1`을 반환합니다.**
- `findValue()`는 결과로 `int`를 반환합니다.
    - `int`와 같은 기본형은 항상 값이 있어야 합니다.
    - 여기서도 값을 반환할 때 값을 찾지 못하면 숫자 중에 하나를 반환해야 하는데 보통 `-1` 또는 `0`을 사용합니다.

**실행 결과**
```bash
-1
0
1
-1
```

- 실행 결과를 보면 입력값이 `-1`일 때 `-1`을 반환합니다.
- 그런데 배열에 없는 값인 `100`을 입력해도 같은 `-1`을 반환합니다.
- 입력값이 `-1`일 때를 생각해보면, 배열에 `-1` 값이 있어서 `-1`을 반환한 것인지, 아니면 `-1` 값이 없어서 `-1`을 반환한 것인지 명확하지 않습니다.

## 2️⃣ 참조형과 null.
- 객체의 경우 데이터가 없다는 `null`이라는 명확한 값이 존재합니다.

```java
public class MyIntegerNullMain1 {

	public static void main(String[] args) {
		MyInteger[] intArr = {new MyInteger(-1), new MyInteger(0), new MyInteger(1)};
		System.out.println(findValue(intArr, -1)); // -1
		System.out.println(findValue(intArr, 0));
		System.out.println(findValue(intArr, 1));
		System.out.println(findValue(intArr, 100)); // null

	}

	private static MyInteger findValue(MyInteger[] intArr, int target) {
		for (MyInteger myInteger : intArr) {
			if (myInteger.getValue() == target) {
				return myInteger;
			}
		}
		return null;
	}
}
```

**실행 결과**
```bash
-1
0
1
null
```

- 이전에 만든 [`MyInteger` 래퍼 클래스](https://www.devkobe24.com/Java/Java/2024-09-13-Wrapper-class-Limitations-of-Primitive-Type-1.html)를 사용했습니다.
- 실행 결과를 보면 `-1`을 입력했을 때는 `-1`을 반환합니다.
- `100`을 입력했을 때는 값이 없다는 `null`을 반환합니다.

## 3️⃣ 요약.
- 기본형은 항상 값이 존재해야 합니다.
    - 숫자의 경우 `0`, `-1` 같은 값이라도 항상 존재해야 합니다. 
- 객체인 참조형은 값이 없다는 `null`을 사용할 수 있습니다.
    - `null` 값을 반환하는 경우 잘못하면 `NullPointException`이 발생할 수 있기 때문에 주의해서 사용해야 합니다.
