---
title: "☕️[Java] Primitive Type과 Wrapper Class."
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-06-08"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 1️⃣ 도화선 🧨

'int와 Integer의 차이가 무엇이 있을까?' 하는 호기심이 도화선이 되어 이 글을 쓰게 되었습니다 :)

먼저 'int와 Integer의 차이'를 알아보니 **'int'** 는 **'Primitive Type'** 이고, **'Integer'** 는 **'Wrapper Class'** 라는 것을 알게 되었습니다.

# 2️⃣ Primitive Type과 Wrapper Class.

Java에서 **Primitive Type** 과 **Wrapper Class**는 각각 기본 데이터 타입과 그 기본 타입을 객체로 감싸는 클래스입니다.

이 둘의 개념과 차이점을 이해하는 것은 Java 프로그래밍에서 매우 중요합니다.

## 1️⃣ Primitive Type (기본 데이터 타입).

**Primitive Type**은 Java에서 가장 기본적인 데이터 타입을 말합니다.

Java는 다음과 같은 8가지 기본 데이터 타입을 제공합니다.

- 1. **boolean :** 논리값(true 또는 false).

- 2. **byte :** 8비트 정수.

- 3. **short :** 16비트 정수.

- 4. **int :** 32비트 정수.

- 5. **long :** 64비트 정수.

- 6. **float :** 32비트 부동 소수점.

- 7. **double :** 64비트 부동 소수점.

- 8. **char :** 16비트 유니코드 문자.

이러한 타입들은 성능이 뛰어나고 메모리를 적게 사용하며, 객체를 생성할 필요 없이 값 그 자체를 저장하고 조작할 수 있습니다.

### 예시

```java
int a = 10;
boolean isJavaFun = true;
char letter = 'A';
```

## 2️⃣ Wrapper Class (래퍼 클래스)

**Wrapper Class** 는 각 **Primitive Type** 에 대응되는 클래스입니다.

이 클래스들은 **Primitive Type**을 객체로 감싸기 때문에 **"래퍼 클래스"** 라고 불립니다.

Java는 각 기본 타입에 대한 래퍼 클래스를 제공합니다.

- 1. **boolean -> 'Boolean'**

- 2. **byte -> 'Byte'**

- 3. **short -> 'Short'**

- 4. **int -> 'Integer'**

- 5. **long -> 'Long'**

- 6. **float -> 'Float'**

- 7. **double -> 'Double'**

- 8. **char -> 'Character'**

**Wrapper Class** 는 다음과 같은 이유로 사용됩니다.

- **Primitive Type** 을 객체로 다루어야 할 때(예: 컬렉션 프레임워크에서는 객체만 저장할 수 있음)
- **null** 값을 처리해야 할 때
- 추가 메서드 및 기능을 사용해야 할 때(예: 문자열을 정수로 변환하는 메서드 등)

### 예시

```java
Integer a = 10;
Boolean isJavaFun = true;
Character letter = 'A';
```

# 3️⃣ Autoboxing 과 Unboxing

Java는 기본 타입과 래퍼 클래스 간의 자동 변환을 지원합니다.

이를 **Autoboxing** 과 **Unboxing** 이라고 합니다.

- **Autonboxing :** 기본 타입이 자동으로 해당 래퍼 클래스 객체로 변환되는 것.

- **Unboxing :** 래퍼 클래스 객체가 자동으로 해당 기본 타입으로 변환되는 것,

### 예시

```java
int primitiveInt = 5;
Integer wrapperInt = primitiveInt; // Autoboxing

Integer anotherWrapperInt = 10;
int anotherPrimitiveInt = anotherWrapperInt; // Unboxing
```

# 4️⃣ Primitive Type 과 Wrapper Class의 차이점.

- 1. **메모리 사용 :**
    - **Primitive Type :** 메모리 효율적, 객체 오버헤드 없음.
    - **Wrapper Class :** 객체 오버헤드가 있어 더 많은 메모리 사용.

- 2. **기본값 :**
    - **Primitive Type :** 기본값이 정의되어 있음 (예: int는 0, boolean은 false).
    - **Wrapper Class :** 기본값이 **'null'** 일 수 있음.

- 3. **성능 :**
    - **Primitive Type :** 빠른 연산 속도.
    - **Wrapper Class :** 객체 생성과 가비지 컬렉션의 오버헤드로 인해 상대적으로 느립.

- 4. **기능성 :**
    - **Primitive Type :** 단순한 데이터 저장과 연산에 적합.
    - **Wrapper Class :** 다양한 유틸리티 메서드 제공 (예: 문자열 반환, 비교 메서드 등).

### 예제 코드

다음은 Primitive Type과 Wrapper Class의 사용 예를 보여주는 코드입니다.

```java
import java.util.ArrayList;
import java.util.List;

public class Main {

	public static void main(String[] args) {
		// Primitive Type 사용
		int primitiveInt = 100;
		boolean primitiveBoolean = true;

		// Wrapper Class 사용
		Integer wrapperInt = Integer.valueOf(100); // 명시적 변환
		Boolean wrapperBoolean = Boolean.valueOf(true);

		// Autoboxing and Unboxing
		Integer autoboxedInt = 200; // Autoboxing
		int unboxedInt = autoboxedInt; // Unboxing

		// Primitive Type은 컬렉션에 저장할 수 없음
		List<Integer> intList = new ArrayList<>();
		intList.add(primitiveInt); // Autoboxing
		intList.add(wrapperInt);

		// 컬렉션에서 값을 가져올 때 Unboxing
		int sum = 0;
		for (int num : intList) {
			sum += num; // Unboxing
		}

		System.out.println("Sum: " + sum); // Output: Sum: 200
	}
}
```

> 이 예제에서는 기본 타입과 래퍼 클래스를 사용하여 변수레 값을 저장하고, 컬렉션에 저장된 래퍼 클래스 객체를 사용하려 연산을 수행하는 과정을 보여줍니다.
