---
title: 🍃[Spring] `application.yml`과 `application.properties`의 차이점.
tags:
    - Spring
    - Framework
date: "2024-10-01"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] `application.yml`과 `application.properties`의 차이점.
Java 백엔드 애플리케이션에서 `application.yml`과 `application.properties`는 모두 **애플리케이션의 설정** 을 관리하는 데 사용되는 구성 파일입니다.

이 두 파일은 **Spring Boot**와 같은 프레임워크에서 애플리케이션의 환경 설정, 데이터베이스 연결, 포트 번호, 보안 설정 등을 정의하는 데 사용됩니다.

두 파일은 기능적으로 비슷하지만 **형식과 가독성에서 차이가 있습니다.**

## 1️⃣ 차이점.

### 1. 파일 형식.
- `application.properties`
    - **키-값 쌍** 형식의 구성을 사용합니다.
    - 각 설정은 한 줄에 하나씩, `key=value` 형식으로 작성됩니다.
- `application.yml`
    - **YAML** 형식을 사용합니다.
    - YAML은 **계층적 구조**와 **들여쓰기**를 통해 설정을 정의하며, JSON과 유사한 방식으로 데이터를 구성합니다.

### 예시
- `application.properties` 예시
```bash
server.port=8080
spring.dataspurce.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=secret
logging.level.org.springframework=DEBUG
```

- `application.yml` 예시
```yaml
server:
    port: 8080
spring:
    datasource:
        url: jdbc:mysql://localhost:3306/mydb
        username: root
        password: secret
logging:
    level:
        org.springframework: DEBUG
```

### 2. 계층 구조 표현.
- `application.properties`
    - 각 설정 항목은 **점 표기법(dot natation)** 을 사용하여 계층 구조를 표현합니다.
        - 예를 들어, `spring.datasource.url` 처럼 점을 사용해 중첩된 속성을 정의합니다.
- `application.yml`
    - YAML은 **들여쓰기**를 통해 계층적 구조를 자연스럽게 표현할 수 있습니다.
    - 점 표기법 대신 들여쓰기로 중첩된 구조를 표현합니다.

### 3. 가독성
- `application.properties`
    - 모든 설정이 한 줄에 키-값 쌍으로 표시되므로 **간단한 설정**에서는 읽기 쉽습니다.
    - 그러나 계층적 데이터 구조를 표현해야 할 때는 점 표기법을 사용해야 하므로, 설정이 많아질 수록 읽기 어려워질 수 있습니다.

- `application.yml`
    - YAML은 **들여쓰기**를 사용해 계층 구조를 표현하기 때문애, **복잡한 설정을 더 가독성 있게 표현**할 수 있습니다.
    - 설정이 많거나 중첩된 경우에도 더 명확하게 구성할 수 있습니다.

### 4. 데이터 표현의 유연성.
- `application.properties`
    - 단순히 키-값 쌍으로 데이터 표현이 제한됩니다.
    - 배열이나 복잡한 데이터 구조를 표현할 때는 여러 줄에 걸쳐 점 표기법을 사용해야 합니다.
- `application.yml`
    - YAML은 배열, 객체, 중첩된 구조를 쉽게 표현할 수 있습니다.
    - 복잡한 데이터 구조를 표현하는 데 더 **유연**합니다.

### 배열 표현 예시.
- **`application.properties`** 에서 배열을 표현하는 방법.
```bash
mylist[0]=item1
mylist[1]=item2
mylist[2]=item3
```

- **`application.yml`** 에서 배열을 표현하는 방법.
```bash
mylist:
    - item1
    - item2
    - item3
```

### 5. 주석.
- `application.properties`
    - 주석은 `#` 기호로 시작합니다.
    - 주석은 한 줄에 추가할 수 있습니다.
- `application.yml`
    - 주석도 `#` 기호를 사용합니다.
    - 주석을 작성하는 방식은 `application.properties`와 동일하지만, YAML 형식에서는 여러 줄에 걸친 주석을 추가하기에 더 자연스럽습니다.

### 6. 사용 용도.
- `application.properties`
    - **단순한 설정**을 정의할 때 유용합니다.
    - 속성 수가 적고 계층적 구조가 많이 필요하지 않은 경우 더 직관적일 수 있습니다.
- `application.yml`
    - **복잡한 설정**을 정의할 때 적합합니다.
    - YAML은 데이터의 계층적 구조를 쉽게 표현 할 수 있어, 중첩된 설정이나 다수의 설정이 필요한 경우 더 적합합니다.

## 2️⃣ 선택 기준
- **작은 프로젝트**나 **단순한 설정**에는 `application.properties`가 적합할 수 있습니다.
    - 점 표기법으로 간단히 설정할 수 있기 때문에 직관적이고 빠르게 설정을 적용할 수 있습니다.
- **복잡한 프로젝트**나 **다중적인 설정**이 필요한 경우, 특히 설정 로깅 레벨 설정, 다중 환경 관리 등의 복잡한 구성이 요구될 때는 `application.yml`이 더 적합합니다.
    - YAML의 구조적 표현 덕분에 가독성과 유지보수성이 향상됩니다.

## 3️⃣ 요약.
- `application.properties`
    - 단순한 키-값 쌍으로 이루어진 설정 파일입니다.
    - 점 표기법을 사용해 계층적 구조를 표현하며, 단순한 설정에 적합합니다.
- `application.yml`
    - YAML 형식의 설정 파일로, 들여쓰기와 계층적 구조를 통해 복잡한 설정을 보다 직관적이고 가독성 있게 표현할 수 있습니다.
    - 복잡한 프로젝트에서 유리합니다.

결국, **둘 다 동일한 기능을 수행할** 수 있지만, 설정의 복잡도와 가독성 요구에 따라 `properties`와 `yml` 중 적합한 형식을 선택할 수 있습니다.
