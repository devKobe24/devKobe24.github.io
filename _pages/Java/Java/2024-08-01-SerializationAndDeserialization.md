---
title: "☕️[Java] ObjectMapper 클래스, 직렬화와 역직렬화"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-08-01"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] ObjectMapper 클래스, 직렬화와 역직렬화.

- **`ObjectMapper`** 는 주로 JSON 데이터를 처리하기 위해 사용되는 Jackson 라이브러리의 핵심 클래스입니다.
- 이 클래스는 자바 객체와 JSON 형식 간의 직렬화(Serialization)와 역직렬화(Deserialization)를 수행합니다.
- **`ObjectMapper`** 는 JSON 데이터를 자바 객체로 변환하거나 자바 객체를 JSON 데이터로 변환하는 등의 작업을 매우 효율적으로 처리할 수 있게 해줍니다.

## 1️⃣ 직렬화(Serialization)

- **`ObjectMapper`** 를 사용하여 자바 객체를 JSON 문자열로 직렬화하는 과정은 다음과 같습니다.

```java
import com.fasterxml.jackson.databind.ObjectMapper;

// 예시 자바 객체
pulbic class User {
    public String name;
    public int age;
}

// 직렬화 예제
ObjectMapper mapper = new ObjectMapper();
User user = new User();
user.name = "Kobe";
user.age = "30";

String json = mapper.writeValueAsString(user); // 자바 객체를 JSON 문자열로 변환

System.out.println(json);
```

## 2️⃣ 역직렬화(Deserialization)

- **`ObjectMapper`** 를 사용하여 JSON 문자열을 자바 객체로 역직렬화하는 과정은 다음과 같습니다.

```java
import com.fasterxml.jackson.databind.ObjectMapper;

// 예시 자바 객체
public class User {
    public String name;
    public int age;
}

// 역직렬화 예제
ObjectMapper mapper = new ObjectMapper();
String json = "{\"name\":\"Kobe\", \"age\":30}";

User user = mapper.readValue(json, User.class); // JSON 문자열을 자바 객체로 변환

Systeom.out.println(user.name + " is" + user.age + " year old.");
```

## 3️⃣ 주요 기능

- **다양한 데이터 포맷 지원 :**  **`ObjectMapper`** 는 JSON 외에도 XML, CSV 등 여러 데이터 포맷을 지원합니다.(Jackson 데이터 포맷 모듈 설치 필요).

- **유연성과 설정 :** **`ObjectMapper`** 는 맞춤 설정이 가능하여, 다양한 JSON 직렬화/역직렬화 방법을 지원합니다.
    - 예를 들어, 필드 이름의 자동 감지, 날짜 형식 지정, 무시할 필드 설정 등을 조정할 수 있습니다.

- **성능 :** Jackson은 JSON 처리를 위해 최적화된 라이브러리 중 하나로, 대용량 데이터 처리에도 뛰어난 성능을 보입니다.

---

# 🤔 직렬화와 역직렬화란?

- 직렬화(Serialization)와 역직렬화(Deserialization)는 데이터 구조 또는 객체 상태를 저장하고 전송하기 위해 다루기 쉬운 데이터 포맷으로 변환하는 과정을 의미합니다.
    - 컴퓨터 과학의 맥락에서 이 개념은 특히 중요하며, 객체 지향 프로그래밍에서 널리 사용됩니다.

## 1️⃣ 직렬화(Serialization)
- 직렬화는 객체의 상태(즉, 객체가 가진 데이터와 그 구조)를 일련의 바이트로 변환하는 과정입니다.
    - 이 바이트 스트림은 나중에 파일, 데이터베이스 또는 네트워크를 통해 쉽게 저장하거나 전송할 수 있습니다.
        - 예를 들어, 자바에서는 **`Serialization`** 인터페이스를 구현한 객체를 바이트 스트림으로 변환하여 파일 시스템에 저장하거나 네트워크를 통해 다른 시스템으로 보낼 수 있습니다.

## 2️⃣ 직렬화의 주요 목적.
- 1. **영속성 :** 객체의 상태를 영구적으로 저장하여 나중에 다시 로드할 수 있습니다.
- 2. **네트워크 전송 :** 객체를 네트워크를 통해 다른 시스템으로 전송하기 위해 사용됩니다.
- 3. **데이터 교환 :** 다양한 언어나 플랫폼 간의 데이터 교환이 가능하도록 합니다.

## 3️⃣ 역직렬화(Deserialization)
- 역직렬화는 직렬화된 바이트 스트림을 다시 원래의 객체 상태로 복원하는 과정입니다.
    - 즉, 파일, 데이터베이스 또는 네트워크로부터 바이트 스트림을 읽어 들여서 실행 중인 프로그램에서 사용할 수 있는 실제 객체로 변환합니다.
    - 이 과정은 직렬화의 반대 과정으로, 복원된 객체는 원복 객체와 동일한 상태를 가집니다.

## 4️⃣ 역직렬화의 주요 사용 사례.
- 1. **객체 복원 :** 저장되거나 전송된 데이터로부터 객체를 재구성합니다.
- 2. **상태 복구 :** 애플리케이션의 이전 상태를 복구하는 데 사용됩니다.
- 3. **데이터 접근 :** 다른 시스템에서 전송된 데이터를 로컬 시스템에서 접근하고 사용할 수 있게 합니다.

## 5️⃣ 데이터 포맷과 직렬화 도구
- 다양한 데이터 포맷(JSON, XML, YAML 등)과 여러 프로그래밍 언어 또는 라이브러리에서 직렬화와 역직렬화를 지원합니다.
- 자바에서는 **`ObjectMapper`** 를 사용해 JSON 데이터 포맷으로의 직렬화와 역직렬화를 처리하며, 이는 데이터를 쉽게 읽고 쓸 수 있는 구조로 만드는 데 유용합니다.
