---
title: "☕️[Java] 메서드 체이닝 - Method Chaining"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-09-07"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] 메서드 체이닝 - Method Chaining.

간단한 예제 코드로 메서드 체이닝(Method Chaining)에 대해 알아봅시다.

## 1️⃣ 예제 코드.
```java
public class ValueAdder {
	private int value;

	public ValueAdder add(int addValue) {
		value += addValue;
		return this;
	}

	public int getValue() {
		return value;
	}
}
```
- 단순히 값을 누적해서 더하는 기능을 제공하는 클래스입니다.
- `add()` 메서드를 호출할 때 마다 내부의 `value`에 값을 누적합니다.
- `add()` 메서드를 보면 자기 자신(`this`)의 참조값을 반환합니다.(이 부분을 유의해서 봅시다.)

```java
public class MethodChainingMain1 {

	public static void main(String[] args) {
		ValueAdder adder = new ValueAdder();

		adder.add(1);
		adder.add(2);
		adder.add(3);

		int result = adder.getValue();
		System.out.println("result = " + result);
	}

}
```

**실행 결과**
```bash
result = 6
```
- `add()` 메서드를 여러번 호출해서 값을 누적하여 더하고 출력합니다.
- 여기서는 `add()` 메서드의 반환값은 사용하지 않았습니다.

이번에는 `add()` 메서드의 반환값을 사용해봅시다.

```java
public class MethodChainingMain2 {

	public static void main(String[] args) {
		ValueAdder adder = new ValueAdder();

		ValueAdder adder1 = adder.add(1);
		ValueAdder adder2 = adder1.add(2);
		ValueAdder adder3 = adder2.add(3);

		int result = adder3.getValue();
		System.out.println("result = " + result);
	}

}
```

**실행 결과**
```bash
result = 6
```
- 실행 결과는 기존과 같습니다.

<img src = "https://github.com/devKobe24/images2/blob/main/Inflearn-Java-Mid/java-method-chaining-1.png?raw=true">

- 1. `adder.add(1)` 을 호출합니다.
- 2. `add()` 메서드는 결과를 누적하고 자기 자신의 참조값인 `this(x001)`를 반환합니다.
- 3. `adder1` 변수는 `adder`와 같은 `x001` 인스턴스를 참조합니다.

<img src = "https://github.com/devKobe24/images2/blob/main/Inflearn-Java-Mid/java-method-chaining-2.png?raw=true">

- `add()` 메서드는 자기 자신(`this`)의 참조값을 반환합니다.
    - 이 반환값을 `adder1`, `adder2`, `adder3`에 보관했습니다.
        - 따라서 `adder`, `adder1`, `adder2`, `adder3`은 모두 같은 참조값을 사용합니다.
            - 왜냐하면 `add()` 메서드가 자기 자신(`this`)의 참조값을 반환했기 때문입니다.

그런데 이 방식은 처음 방식보다 더 불편하고, 코드도 잘 읽히지 않습니다.
이런 방식을 왜 사용할까요?

이번에는 방금 사용했던 방식에서 반환된 참조값을 새로운 변수에 담아서 보관하지 않고, 대신에 바로 메서드에 호출에 사용해보겠습니다.

```java
public class MethodChainingMain3 {

	public static void main(String[] args) {
		ValueAdder adder = new ValueAdder();

		int result = adder.add(1).add(2).add(3).getValue();
		System.out.println("result = " + result);
	}

}
```

**실행 결과**
```bash
result = 6
```

**실행 순서**
- `add()` 메서드를 호출하면 `ValueAdder` 인스턴스 자신의 참조값(`x001`)이 반환됩니다.
    - 이 반환된 참조값을 변수에 담아두지 않아도 됩니다.
        - 대신에 반환된 참조값을 즉시 사용해서 바로 메서드를 호출할 수 있습니다.

다음과 같은 순서로 실행됩니다.
```java
adder.add(1).add(2).add(3).getValue(); // value = 0
x001.add(1).add(2).add(3).getValue(); // value = 0, x001.add(1)을 호출하면 그 결과로 x001을 반환합니다.
x001.add(2).add(3).getValue(); // value = 1, x001.add(2)을 호출하면 그 결과로 x001을 반환합니다.
x001.add(3).getValue(); // value = 3, x001.add(3)을 호출하면 그 결과로 x001을 반환합니다.
x001.getValue(); // value = 6
6
```
- 메서드 호출의 결과로 자기 자신의 참조값을 반환하면, 반환된 참조값을 사용해서 메서드 호출을 계속 이어갈 수 있습니다.
    - 코드를 보면 `.`을 찍고 메서드를 계속 연결해서 사용합니다.
        - 마치 메서드가 체인으로 연결된 것 처럼 보입니다.
            - 이러한 기법을 메서드 체이닝이라고 합니다.
                - 물론 실행 결과도 기존과 동일합니다.
- 기존에는 메서드를 호출할 때 마다 계속 변수명에 `.`을 찍어야 했습니다
    - 예) `adder.add(1)`, `adder.add(2)`
- 메서드 체이닝 방식은 메서드가 끝나는 시점에 바로 `.` 을 찍어서 변수명을 생략할 수 있습니다.
- 메서드 체이닝이 가능한 이유는 자기 자신의 참조값을 반환하기 때문입니다.
    - 이 참조값에 `.`을 찍어서 바로 자신의 메서드를 호출할 수 있습니다.
- **메서드 체이닝 기법은 코드를 간결하고 읽기 쉽게 만들어줍니다.**

## 2️⃣ StringBuilder와 메서드 체인(Chain)
- `StringBuilder`는 메서드 체이닝 기법을 제공합니다.
- `StringBuilder`의 `append()` 메서드를 보면 자기 자신의 참조값을 반환합니다.
```java
public StringBuilder append(String str) {
    super.append(str);
    return this;
}
```

- `StringBuilder`에서 문자열을 변경하는 대부분의 메서드도 메서드 체이닝 기법을 제공하기 위해 자기 자신을 반환합니다.
    - 예) `insert()`, `delete()`, `reverse()`

```java
public class StringBuilderMain1_1 {

	public static void main(String[] args) {
		StringBuilder sb = new StringBuilder();
		sb.append("A");
		sb.append("B");
		sb.append("C");
		sb.append("D");
		System.out.println("sb = " + sb);

		sb.insert(4, "Java");
		System.out.println("sb = " + sb);

		sb.delete(4, 8);
		System.out.println("delete = " + sb);

		sb.reverse();
		System.out.println("reverse = " + sb);

		// StringBuild -> String
		String string = sb.toString();
		System.out.println("string = " + string);
	}
}
```
- 위 코드를 메서드 체이닝 기법을 사용하면 아래와 같이 코드를 작성할 수 있습니다.

```java
public class StringBuilderMain1_2 {

	public static void main(String[] args) {
		StringBuilder sb = new StringBuilder();
		String string = sb.append("A").append("B").append("C").append("D")
		                  .insert(4, "Java")
		                  .delete(4, 8)
		                  .reverse()
		                  .toString();
		System.out.println("string = " + string);
	}
}
```

**실행 결과**
```bash
string = DCBA
```

## 3️⃣ 정리.
- **"만드는 사람이 수고로우면 쓰는 사람이 편하고, 만드는 사람이 편하면 쓰는 사람이 수고롭다"** 는 말이 있습니다.
    - 메서드 체이닝은 구현하는 입장에서는 번거롭지만 사용하는 개발자는 편리해집니다.
        - 참고로 자바의 라이브러리와 오픈 소스들은 메서드 체이닝 방식을 종종 사용합니다.
