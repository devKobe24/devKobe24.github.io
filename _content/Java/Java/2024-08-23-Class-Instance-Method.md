---
title: "☕️[Java] 클래스 메서드와 인스턴스 메서드"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-08-23"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] 클래스 메서드와 인스턴스 메서드.

## 1️⃣ 클래스 메서드.
```java
package langReview.object.tostring;

public class ObjectPrinter {
	public static void print(Object obj) {
		String string = "객체 정보 출력: " + obj.toString();
		System.out.println(string);
	}
}
```

- 위와 같이 **`'static'`** 키워드를 사용하여 클래스에 속한 메서드를 **"클래스 메서드"** 라고 부릅니다.
- 클래스 메서드는 특정 객체(instance)와 무관하게 클래스 자체에 속하며, 클래스 이름을 통해 직접 호출할 수 있습니다.
    - 예를 들어 **`'ObjectPrinter.print(obj)'`** 와 같이 사용합니다.

## 2️⃣ 인스턴스 메서드.
```java
package langReview.object.tostring;

public class ObjectPrinter {
	public void print(Object obj) {
		String string = "객체 정보 출력: " + obj.toString();
		System.out.println(string);
	}
}
```

- 위와 같은 메서드는 **`'인스턴스 메서드'`** 라고 부릅니다.
- 인스턴스 메서드는 **`'static'`** 키워드가 없으며, 특정 클래스의 인스턴스(객체)와 연관되어 있습니다.
- 인스턴스 메서드는 클래스의 객체를 생성한 후, 그 객체를 통해 호출할 수 있습니다.
    - 예를 들어, **`'ObjectPrinter printer = new ObjectPrinter();'`** 로 객체를 생성한 후, **`'printer.print(obj);'`** 와 같이 호출할 수 있습니다.
