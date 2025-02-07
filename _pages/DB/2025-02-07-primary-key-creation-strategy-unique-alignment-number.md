---
title: "💾[Database] Primary Key 생성 전략 - 유니크 정렬 숫자"
tags:
    - Database
    - Primary Key
    - DBMS
date: "2025-02-07"
thumbnail: "/assets/img/thumbnail/database.jpeg"
---

# "💾[Database] Primary Key 생성 전략 - 유니크 정렬 숫자"
## 🍎 Intro.
- **PK(Primary Key)를 생성할 때,** 각 ID가 **고유하면서도 생성된 순서대로 정렬이 가능**하도록 만드는 방식입니다.
- 이 방식은 **숫자 기반 ID(예: Snowflake, Sonyflake, TikTok ID 등)를 사용하여 생성된 순서대로 정렬 가능하도록 설계**된 것이 특징입니다.

## ✅1️⃣ 유니크 정렬 숫자 방식이 필요한 이유.
### ✅1️⃣ 유니크 정렬 숫자 방식의 장점.
- **생성된 순서대로 정렬 가능 ➞** 최근 데이터를 쉽게 조회 가능.
- **숫자형 ID이므로 인덱싱 성능이 우수 ➞** UUID보다 빠름.
- **다중 서버 환경(분산 시스템)에서도 고유성 보장 가능.**
- **대량 트래픽 처리에 적합.**

### ❌2️⃣ 기존 숫자 기반 PK(AUTO_INCREMENT)의 문제점.
- **단일 서버 환경에서는 문제 없지만, 다중 서버(분산 환경)에서는 충돌 가능성 있음.**
- **ID가 예측 가능하여 보안상 취약함 ➞** 공격자가 특정 ID를 추측할 수 있음.
- **대량 트래픽 처리에 적합하지 않음.**(숫자가 순차적으로 증가하면서 성능 저하 가능)

## ✅2️⃣ 유니크 정렬 숫자 방식의 종류.

|전략|예제|길이|특징|
| -------- | -------- | --- | -------- |
|**Snowflake (Twitter 방식)**|146789123456789012|19자|시간 기반 정렬 가능, 성능 최적화.|
|**Sonyflake (Go 언어 기반)**|404168231342172160|19자|Snowflake보다 더 빠름.|
|**TikTok ID**|720615963569438720|18~19자|Snowflake 변형, 글로벌 서비스 사용.|
|Flake ID (Custom 방식)|1706745678123456|16자|Snowflake를 단순화한 버전|

## ✅3️⃣ 유니크 정렬 숫자 방식의 예제 코드
### 🛠️1️⃣ Snowflake(Twitter에서 개발한 분산 ID)
#### 📌 Snowflake란?
- Twitter에서 **대량의 트래픽을 처리하기 위해 개발한 ID 생성 알고리즘.**
- **64비트 정수 (19자리)를 사용하여 정렬 가능.**
- **시간 정보 + 서버 정보 + 시퀀스 번호로 ID를 생성.**

#### 📝 Snowflake ID 구조

|비트 수|설명|
| -------- | -------- |
|1비트|예약(항상 0)|
|41비트|**타임스탬프(현재 시간 기준)**|
|10비트|**서버 ID 또는 데이터센터 ID**|
|12비트|**시퀀스 번호 (같은 밀리초 내에서 증가)**|

#### 📌 Snowflake ID Java 구현.
```java
import java.time.Instant;

public class Snowflake {
    private static final long EPOCH = 1640995200000L; // 기준 시간(2022-01-01 00:00:00)
    private static final long MACHINE_ID = 1L;
    private static final long SEQUENCE_BITS = 12L;
    
    private long lastTimestamp = -1L;
    private long sequence = 0L;
    
    public synchronized long nextId() {
        long timestamp = Instant.now().toEpochMilli();
        if (timestamp == lastTimestamp) {
            sequence = (sequence + 1) & ((1 << SEQUENCE_BITS) - 1);
            if (sequence == 0) {
                while (timestamp <= lastTimestamp) {
                    timestamp = Instant.now().toEpochMilli();
                }
            }
        } else {
            sequence = 0;
        }
        lastTimestamp = timestamp;
        return ((timestamp - EPOCH) << (64 - 41)) | (MACHINE_ID << (64 - 41 - 10)) | sequence;
    }
}
```

#### ✅ Snowflake ID 특징.
- 시간 기반 ID이므로 **자동 정렬 가능.**
- **19자리 정수(BIGINT)로 저장 가능.**
- 다중 서버 환경에서도 **충돌 없음.**

### 🛠️2️⃣ Sonyflake(Go 언어 기반의 빠른 ID 생성)
#### 📌 Sonyflake란?
- **Snowflake보다 단순하고 빠른 ID 생성 알고리즘.**
- **시간. 정보 + 머신 ID + 시퀀스 번호**로 구성.

#### 📌 Sonyflake ID Java 구현.
```java
import java.time.Instant;

public class Sonyflake {
    private static final long EPOCH = 1640995200000L; // 기준 시간(2022-01-01 00:00:00)
    private static final long MACHINE_ID = 1L;
    private static final long SEQUENCE_BITS = 8L;
    
    private long lastTimestamp = -1L;
    private long sequence = 0L;
    
    public synchronized long nextId() {
        long timestamp = Instant.now().toEpochMilli();
        if (timestamp == lastTimestamp) {
            sequence = (sequence + 1) & ((1 << SEQUENCE_BITS) - 1);
            if (sequence == 0) {
                while (timestamp <= lastTimestamp) {
                    timestamp = Instant.now().toEpochMilli();
                }
            }
        } else {
            sequence = 0;
        }
        lastTimestamp = timestamp;
        return ((timestamp - EPOCH) << (64 - 39)) | (MACHINE_ID << (64 - 39 - 8)) | sequence;
    }
}
```

#### ✅ Sonyflake의 장점.
- Snowflake보다 **더 빠르고 경량.**
- **비트 연산을 최적화하여 성능 향상.**
- **간단한 구조로 서버에서 쉽게 사용 가능.**

### 🛠️3️⃣ TikTok ID (Snowflake 변형)
#### 📌 TikTok ID란?
- Snowflake 기반이지만 **시퀀스 번호가 더 크고, 글로벌 사용에 최적화됨.**
- **데이터센터 ID, 머신 ID 등을 최적화하여 사용.**

#### 📌 TikTok ID Java 구현
```java
public class TikTokIdGenerator {
    private static final long START_TIMESTAMP = 1420070400000L; // 2015-01-01 00:00:00
    private static final long SEQUENCE_BITS = 12L;
    private static final long MACHINE_ID = 1L;
    
    private long lastTimestamp = -1L;
    private long sequence = 0L;

    public synchronized long nextId() {
        long timestamp = System.currentTimeMillis();
        if (timestamp == lastTimestamp) {
            sequence = (sequence + 1) & ((1 << SEQUENCE_BITS) - 1);
            if (sequence == 0) {
                while (timestamp <= lastTimestamp) {
                    timestamp = System.currentTimeMillis();
                }
            }
        } else {
            sequence = 0;
        }
        lastTimestamp = timestamp;
        return ((timestamp - START_TIMESTAMP) << (64 - 41)) | (MACHINE_ID << (64 - 41 - 10)) | sequence;
    }
}
```

#### ✅ TikTok ID의 특징.
- **Snowflake보다 더 유연한 구조.**
- **분산 시스템에서 고유성 유지.**

## ✅4️⃣ 유니크 정렬 숫자ㅏ 방식의 비교.

|전략|예제|길이|특징|
| -------- | -------- | --- | -------- |
|**Snowflake**|146789123456789012|19자|시간 기반 정렬 가능, 성능 최적화|
|**Sonyflake**|404168231342172160|19자|Snowflake보다 더 빠름.|
|**TikTok ID**|720615963569438720|18 ~ 19자|Snowflake 변형, 글로벌 서비스 사용|
|**Flake ID**|1706745678123456|16자|Snowflake를 단순화한 버전|

## ✅5️⃣ 결론..
- **Snowflake, Sonyflake, TikTok ID 같은 유니크 정렬 숫자 방식은 고유성과 정렬 가능성을 동시에 제공.**
- **UUID보다 인덱싱 성능이 우수하며, 대량 트래픽에서도 안정적.**
- **고성능 시스템, 금융 서비스, 로그 저장 등에 적합.**
