---
title: "☕️[Java] 불변 객체 - 문제와 풀이"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-09-01"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] 불변 객체 - 문제와 풀이

## 1️⃣ 불변 객체 - 문제와 풀이.

**문제 설명**
- `MyDate` 클래스는 불변이 아니어서 공유 참조시 사이드 이펙트가 발생합니다. 이를 불변 클래스로 만들어야 합니다.
- 새로운 불변 클래스는 `ImmutableMyDate`
- 새로운 실행 클래스는 `ImmutableMyDateMain`

### 1️⃣ 불변이 아닌 `MyDate` 클래스

```java
// MyDate
public class MyDate {
	private int year;
	private int month;
	private int day;

	public MyDate(int year, int month, int day) {
		this.year = year;
		this.month = month;
		this.day = day;
	}

	public void setYear(int year) {
		this.year = year;
	}

	public void setMonth(int month) {
		this.month = month;
	}

	public void setDay(int day) {
		this.day = day;
	}

	@Override
	public String toString() {
		return year + "-" + month + "-" + day;
	}
}

// MyDateMain
public class MyDateMain {

	public static void main(String[] args) {
		MyDate date1 = new MyDate(2024, 9, 1);
		MyDate date2 = date1;
		System.out.println("date1 = " + date1);
		System.out.println("date2 = " + date2);

		System.out.println("2025 -> date1");
		date1.setYear(2025);
		System.out.println("date1 = " + date1);
		System.out.println("date2 = " + date2);
	}
}
```

**실행 결과**
```bash
date1 = 2024-9-1
date2 = 2024-9-1
2025 -> date1
date1 = 2025-9-1
date2 = 2025-9-1
```

### 2️⃣ 불변 클래스인 `ImmutableMyDate`

```java
// ImmutableMyDate
public class ImmutableMyDate {
	private final int year;
	private final int month;
	private final int day;

	public ImmutableMyDate(int year, int month, int day) {
		this.year = year;
		this.month = month;
		this.day = day;
	}

	public ImmutableMyDate withYear(int newYear) {
		return new ImmutableMyDate(newYear, month, day);
	}

	public ImmutableMyDate withMonth(int newMonth) {
		return new ImmutableMyDate(year, newMonth, day);
	}

	public ImmutableMyDate withDay(int newDay) {
		return new ImmutableMyDate(year, month, newDay);
	}

	@Override
	public String toString() {
		return year + "-" + month + "-" + day;
	}
}

// ImmutableMyDateMain
public class ImmutableMyDateMain {

	public static void main(String[] args) {
		ImmutableMyDate immutableDate1 = new ImmutableMyDate(2024, 9, 1);
		ImmutableMyDate immutableDate2 = immutableDate1;
		System.out.println("immutableDate1 = " + immutableDate1);
		System.out.println("immutableDate2 = " + immutableDate2);

		System.out.println("2025 -> immutableDate1");
		// 방법 1.
		//immutableDate1 = new ImmutableMyDate(2025, 9, 1);

		// 방법 2. -> 이 방법을 더 지향함
		// 주의: 불변 객체에서 값을 변경하는 메서드가 있을 경우에는 무조건 반환값을 받아서 참조를 가지고 가야 바뀐 값을 사용할 수 있습니다.
		immutableDate1 = immutableDate1.withYear(2025); // x002
		System.out.println("immutableDate1 = " + immutableDate1); // x002
		System.out.println("immutableDate2 = " + immutableDate2); // x001
	}
}
```

**실행 결과**
```bash
immutableDate1 = 2024-9-1
immutableDate2 = 2024-9-1
2025 -> immutableDate1
immutableDate1 = 2025-9-1
immutableDate2 = 2024-9-1
```

### 3️⃣ 참고 - withXxx()
- 불변 객체에서 값을 변경하는 경우 `withYear()` 처럼 **"with"** 로 시작하는 경우가 많습니다.
    - 예를 들어, **"coffee with sugar"** 라고 하면, 커피에 설탕이 추가되어 원래의 상태를 변경하여 새로운 변형을 만든가는 것을 의미합니다.
        - 이 개념을 프로그래밍에 적용하면, 불변 객체의 메서드가 **"with"** 로 이름 지어진 경우, 그 메서드가 지정된 수정사항을 포함하는 객체의 새 인스턴스를 반환한다는 사실을 뜻합니다.
            - 정리하면 **"with"** 는 관례처럼 사용되는데, 원본 객체의 상태가 그대로 유지됨을 강조하면서 변경사항을 새 복사본에 포함하는 과정을 간결하게 표현합니다.
