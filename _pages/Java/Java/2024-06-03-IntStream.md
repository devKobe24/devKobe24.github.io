---
title: "☕️[Java] IntStream"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-06-03"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# 1️⃣ Java Docs - IntStream.

- **Module** : [java.base](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/module-summary.html)
- **Package** : [java.util.stream](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/stream/package-summary.html)

## Interface IntStream

**All SuperInterfaces :** AutoCloseble, BaseStream<Integer, IntStream>

- [AutoCloseble](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AutoCloseable.html)
- [BaseStream](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/stream/BaseStream.html)
- [Integer](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Integer.html)
- [IntStream](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/stream/IntStream.html)

---

```java
public interface IntStream extends BaseStream<Integer, IntStream>
```

- 순차 및 병렬 집계 연산을 지원하는 기본 int 값 요소의 시퀀스입니다. 이것은 Stream의 int 기본형 특수화입니다.
    - **`IntStream`** 이 **`Stream`** 의 한 형태로, **`int`** 값의 시퀀스를 처리하며 순차 및 병렬 연산을 지원한다는 의미입니다.

- 다음 예제는 Stream과 IntStream을 사용하여 빨간색 위젯의 무게 합계를 계산하는 집계 연산을 보여줍니다.

```java
int sum = widgets.stream()
                 .filter(w -> w.getColor() == RED)
                 .mapToInt(w -> w.getWeight())
                 .sum();
```

- streams(스트림), stream operations(스트림 연산), stream pipelines(스트림 파이프라인), and parallelism(및 병렬 처리)에 대한 추가적인 명세는 Stream 클래스 문서와 [java.util.stream](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/stream/package-summary.html) 패키지 문서를 참조하십시오.

**Since :** 1.8

## Nested Class Summary

**Nested Classes**
- **Modifier and Type**: static interface
- **Interface:** [IntStream.Builder](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/stream/IntStream.Builder.html)
- **Description:** IntStream용 변경 가능한 빌더입니다.

# 2️⃣ IntStream.

**`IntStream`** 은 Java의 스트림 API(Stream API)의 일부로, 기본형 **`int`** 에 특화된 스트림을 나타냅니다.

**`IntStream`** 은 Java 8에서 도입된 스트림 API의 일부로, 컬렉션(리스트, 배열 등)과 같은 데이터 소스를 함수형 프로그래밍 스타일로 처리할 수 있게 해줍니다.

**`IntStream`** 은 **`Stream<Integer>`** 와는 달리 오토박싱과 언박싱의 오버헤드가 없는 것이 특징입니다.

## 🙋‍♂️ IntStream의 주요 기능

### 1. 생성:

- **`IntStream`** 을 생성하는 방법은 여러가지가 있습니다.
    - 예를 들어, 배열, 범위, 임의의 수 등을 사용하여 생성할 수 있습니다.

### 2. 연산:

- 스트림 연산은 두 가지로 나뉩니다.
    - 중간 연산과 최종 연산.
        - 중간 연산은 또 다른 스트림을 반환하고, 지연(lazy) 평가됩니다.
        - 최종 연산은 스트림을 소비하여 결과를 반환합니다.

## 🙋‍♂️ IntStream 생성 방법.

### 1. of() 메서드:

- 고정된 개수의 **`int`** 값을 스트림으로 생성합니다.

```java
IntStream stream = IntStream.of(1, 2, 3, 4, 5);
```

### 2. range() 및 rangeClosed() 메서드:

- 범위를 지정하여 스트림을 생성합니다. **`range`** 는 시작 값 포함, 끝 값 미포함, **`rangeClosed`** 는 시작 값과 끝 값을 모두 포함합니다.

```java
IntStream stream = IntStream.range(0, 5); // 0, 1, 2, 3, 4, 5
IntStream closedStream = IntStream.rangeClosed(0, 5); // 0, 1, 2, 3, 4, 5
```

### 3. generate() 메서드:

- 람다 표현식을 사용하여 무한 스트림을 생성합니다.
    - 🚨 **주의: 무한 스트림은 반드시 제한을 걸아야 합니다.**

```java
IntStream stream = IntStream.generate(() -> 1).limit(5); // 1, 1, 1, 1, 1
```

### 4. iterate() 메서드:

- 초기값과 반복 함수로 스트림을 생성합니다.

```java
IntStream stream = IntStream.iterate(0, n -> n + 2).limit(5); // 0, 2, 4, 6, 8
```

### 5. builder() 메서드:

- **`IntStream.Builder`** 를 사용하여 스트림을 생성합니다.

```java
IntStream.Builder builder = IntStream.builder()l
builder.add(1).add(2).add(3).add(4).add(5);
IntStream stream = builder.builder();
```

### 6. 배열에서 생성:

- 배열을 스트림으로 변환합니다.

```java
int[] array = {1, 2, 3, 4, 5};
IntStream stream = Arrays.stream(array);
```

## 🙋‍♂️ IntStream의 주요 메서드.

### 1. 중간 연산:

- **`map()` :** 각 요소에 함수 적용.
- **`filter()` :** 조건에 맞는 요소만 통과
- **`distinct()` :** 중복 요소 제거
- **`sorted()` :** 정렬
- **`limit()` :** 스트림 크기 제한
- **`skip()` :** 처음 n개 요소 건너뛰기

### 2. 최종 연산:

- **`forEach()` :** 각 요소에 대해 액션 수행
- **`toArray()` :** 배열로 변환
- **`reduce()` :** 모든 요소를 누적하여 하나의 값으로
- **`collect()` :** 컬렉션으로 변환
- **`sum()` :** 합계 연산
- **`average()` :** 평균 계산
- **`min()`, `max()` :** 최소, 최대값 찾기
- **`count()` :** 요소 개수 반환

## 💻 예제 코드

### 예제 1: 0에서 5까지 거꾸로 출력.

```java
import java.util.stream.IntStream;

public class Reverse {

	public static void main(String[] args) {
		IntStream.rangeClosed(0, 5)
		         .map(i -> 5 - i)
		         .forEach(System.out::println);
	}
}
/*
=== 출력 ===
5
4
3
2
1
0
*/
```

### 예제 2: 배열의 합계 계산

```java
import java.util.stream.IntStream;

public class ArraySum {

	public static void main(String[] args) {
		int[] array = {1, 2, 3, 4, 5};
		int sum = IntStream.of(array).sum();
		System.out.println("sum = " + sum); // sum = 15
	}
}
```

### 예제 3: 짝수 필터링

```java
import java.util.stream.IntStream;

public class FilterEvenNumber {

	public static void main(String[] args) {
		IntStream.rangeClosed(1, 10)
		         .filter(n -> n % 2 == 0)
		         .forEach(System.out::println);
	}
}
/*
=== 출력 ===
2
4
6
8
10
*/
```

# 📝 요약

**`IntStream`** 은 Java의 스트림 API의 일부분으로, 기본형 `int`에 특화된 스트림입니다.

이를 통해 컬렉션이나 배열을 함수형 프로그래밍 스타일로 처리할 수 있습니다.

**`IntStream`** 은 다양한 생성 방법과 중간 및 최종 연산을 제공하여 효율적이고 직관적인 데이터 처리를 가능하게 합니다.

# 📚 참고 문헌.
- [Java Docs - IntStream](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/stream/IntStream.html)
