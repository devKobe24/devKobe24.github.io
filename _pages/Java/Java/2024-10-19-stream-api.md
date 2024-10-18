---
title: "☕️[Java] Java Stream이란 무엇일까요?"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-10-19"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] Java Stream이란 무엇일까요?

- **Java Stream**은 Java 8에서 도입된 **데이터 처리 API로, 컬렉션(Collection), 배열(Array), 파일 등의 데이터 소스를 효율적으로 처리할 수 있게 해주는 기능**입니다.
- Stream을 사용하면 **데이터의 필터링, 변환, 정렬, 집계 등의** 작업을 함수형 스타일로 간결하게 작성할 수 있습니다.

## 1️⃣ Stream의 주요 특징.

### 1️⃣ 선언형 프로그래밍 스타일.
- 기존의 반복문을 사용한 명령형(Imperative) 코드와 달리, Stream은 **선언형(Declarative)** 스타일을 사용하여 더 간결하고 읽기 쉬운 코드를 작성할 수 있게 합니다.

### 2️⃣ 함수형 프로그래밍.
- Stream은 **함수형 프로그래밍**을 지원하여, 람다 표현식(Lambda Expression)과 메서드 참조(Method Reference)를 사용하여 간결한 코드를 작성할 수 있습니다.

### 3️⃣ 데이터의 흐름.
- Stream은 데이터 소스로부터 **데이터 흐름**을 처리하며, **데이터를 직접 저장하지 않습니다.**
    - 즉, 컬렉션이나 배열에 데이터를 저장하는 것이 아니라, 데이터를 **처리하는 역속적인 작업**을 제공합니다.

### 4️⃣ 지연 연산(Lazy Evaluation)
- Stream은 **지연 연산(Lazy Evaluation)을** 사용하여 필요한 경우에만 데이터를 처리합니다.
    - 중간 연산이 지연되어 최종 연산이 호출될 때만 실제로 데이터 처리가 이루어집니다.

### 5️⃣ 병렬 처리.
- Stream API는 쉽게 **병렬 처리**를 수행할 수 있는 메서드를 제공하여, 멀티코어 시스템에서 성능을 최적화할 수 있습니다.
    - `parallelStream()` 메서드를 사용하면 병렬 처리를 활성화할 수 있습니다.

## 2️⃣ Stream의 기본 구성.
- Stream은 크게 **중간 연산**과 **최종 연산**으로 구성됩니다.

### 👉 중간 연산.
- 필터링, 변환, 정렬 등과 같이 데이터의 흐름을 처리하는 작업을 수행하지만, 최종 연산이 호출되기 전까지는 실행되지 않습니다.(예: `filter`, `map`, `sorted`)

### 👉 최종 연산.
- 스트림의 데이터를 결과로 수집하거나 출력하는 연산으로, 최종 연산이 호출되면 스트림이 처리되고 종료됩니다.(예: `collect`, `forEach`, `reduce`)

## 3️⃣ Stream 사용 예시.

### 1️⃣ 컬렉션에서 Stream 사용.
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Jhon", "Jane", "Jack", "Doe");
        
        // 필터링과 변환
        List<String> filteredNames = names.stream()
            .filter(name -> name.startWith("J")) // "J"로 시작하는 이름만 필터링
            .map(String::toUpperCase)            // 대문자로 변환
            .collect(Collectors.toList());       // 결과를 리스트로 수집
        
        System.out.println(filteredNames) // 출력: [JHON, JANE, JACK]
    }
}
```

## 4️⃣ Stream의 기본 연산.

### 👉 중간 연산.

#### 1️⃣ `filter(Predicate)`
- 조건에 맞는 요소만 걸러냅니다.
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> evenNumbers = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList()); // [2, 4]
```

#### 2️⃣ `map(Function)`
- 각 요소를 변환합니다.(예: 문자열을 대문자로 변환, 객체의 특정 필드를 추출)
```java
List<String> words = Arrays.asList("apple", "banana", "cherry");
List<Integer> lengths = words.stream()
    .map(String::length)
    .collect(Collectors.toList()); // [5, 6, 7]
```

#### 3️⃣ `sorted()`
- 요소를 정렬합니다. 기본적으로 오름차순 정렬하며, 커스텀 Comparator를 사용할 수도 있습니다.
```java
List<Integer> sortedNumbers = numbers.stream()
    .sorted
    .collect(Collectors.toList()); // [1, 2, 3, 4, 5]
```

#### 4️⃣ distinct()
- 중복된 요소를 제거합니다.
```java
List<Integer> distinctNumbers = Arrays.asList(1, 2, 2, 3, 3, 3)
    .stream()
    .distinct()
    .collect(Collectors.toList()); // [1, 2, 3]
```

### 👉 최종 연산.

#### 1️⃣ `collect(Collector)`
- 스트림의 요소를 모아서 리스트, 집합 등의 컬렉션으로 변환합니다.
```java
List<String> names = Arrays.asList("John", "Jane", "Jack");
List<String> upperNames = names.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList()); // [JOHN, JANE, JACK]
```

#### 2️⃣ `forEach(Consumer)`
- 각 요소에 대해 지정된 작업을 수행합니다.
    - 출력이나 로그 작업 등에 사용됩니다.
```java
names.stream()
    .forEach(System.out::println);
```

#### 3️⃣ `reduce(BinaryOperator)`
- 스트림의 요소를 누적하여 하나의 결과로 만듭니다.
    - 주로 합계, 곱, 문자열 연결 등에 사용됩니다.
```java
int sum = Arrays.asList(1, 2, 3, 4, 5)
    .stream()
    .reduce(0, Integer::sum) // 15
```

#### 4️⃣ `count()`
- 스트림의 요소 개수를 반환합니다.
```java
long count = names.stream()
    .filter(name -> name.startWith("J"))
    .count(); // 3
```

## 5️⃣ 병렬 스트림
- Stream API는 **병렬 처리를** 쉽게 수행할 수 있도록 지원합니다.
    - `paralleStream()` 또는 `parallel()` 메서드를 사용하여 스트림을 병렬로 처리할 수 있습니다.
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
int sum = numbers.parallelStream()
    .reduce(0, Integer::sum); // 병렬로 합 계산
```

- 병렬 스트림은 **멀티 코어 시스템**에서 성능을 최적화할 수 있지만, 병렬로 처리할 때는 **데이터 간의 독립성**이 보장되어야 합니다.

## 6️⃣ 요약.
- **Java Stream은 데이터의 필터링, 변환. 정렬, 집계 등을 간결하고 선언적인 스타일로 작성할 수 있는 기능을 제공합니다.**
- 중간 연산과 최종 연산을 조합하여 복잡한 데이터 처리를 간단하게 구현할 수 있으며, 병렬 처리를 통해 성능을 향상시킬 수도 있습니다.
- Stream API를 사용하면 기존의 반복문과 조건문을 대체할 수 있어, 코드의 가독성과 유지보수성이 크게 향상됩니다.
