---
title: "📚[Backend Development] Spring Boot Service Layer와 ORM 완벽 이해하기"
tags:
    - Backend Ddevelopment
    - Programming Language
    - Object
    - OOP
    - ORM
    - Spring Boot
    - Service Layer
date: "2025-08-08"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 🚀 Spring Boot Service Layer와 ORM 완벽 이해하기

> **왜 이 문서를 작성했나?** 🤔  
> Java Spring Boot 백엔드 프로젝트에서 Service Layer의 CRUD 메서드를 구현하면서, Response DTO 생성과 반환 과정에서 ORM과 서비스 로직의 핵심을 제대로 이해하지 못해 정리한 학습 노트입니다.

## 🎯 핵심 목표
**Service Layer에서 Entity → DTO 변환 과정과 ORM의 역할을 완벽히 이해하자!**

---

## 📖 목차
1. [ORM을 이해하기 위한 객체(Object) 개념 정리](#-orm을-이해하기-위한-객체object-개념-정리)
2. [ORM이란 무엇인가?](#-orm이란-무엇인가)
3. [Spring Boot Service Layer에서의 실전 적용](#-spring-boot-service-layer에서의-실전-적용)
4. [Entity vs DTO: 언제, 왜 변환하는가?](#-entity-vs-dto-언제-왜-변환하는가)

---

## 🧩 ORM을 이해하기 위한 객체(Object) 개념 정리

### 🔍 객체(Object)의 두 가지 관점

#### 1️⃣ 프로그래밍 언어의 객체
```java
// 단순한 데이터와 메서드의 조합
Map<String, Object> user = new HashMap<>();
user.put("name", "홍길동");
user.put("age", 25);
```
- **정의**: 데이터(속성)와 기능(메서드)를 하나로 묶은 프로그래밍 단위
- **특징**: 언어별로 다양한 형태로 존재 (JavaScript 객체, Python 딕셔너리 등)

#### 2️⃣ 객체지향 프로그래밍(OOP)의 객체
```java
// 클래스 기반의 체계적인 객체
public class User {
    private String name;
    private int age;
    
    public void introduce() {
        System.out.println("안녕하세요, " + name + "입니다!");
    }
}

User user = new User(); // 클래스로부터 생성된 인스턴스
```
- **정의**: 클래스(설계도)를 기반으로 생성된, 캡슐화/상속/다형성/추상화를 따르는 독립적 단위
- **특징**: 객체 간 협력과 상호작용이 핵심

### 💡 핵심 차이점
| 구분 | 프로그래밍 언어의 객체 | OOP의 객체 |
|------|----------------------|------------|
| 생성 방식 | 다양한 방법 | 클래스 기반 |
| 설계 원칙 | 자유로움 | OOP 4대 원칙 준수 |
| 목적 | 데이터 구조화 | 현실 세계 모델링 |

---

## 🔗 ORM이란 무엇인가?

### 📊 ORM의 핵심 개념
**Object-Relational Mapping**: **OOP의 객체**와 **관계형 데이터베이스의 테이블**을 자동으로 연결해주는 기술

```java
// 데이터베이스 테이블
/*
users 테이블
+----+---------+-----+
| id | name    | age |
+----+---------+-----+
| 1  | 홍길동   | 25  |
| 2  | 김철수   | 30  |
+----+---------+-----+
*/

// ↕️ ORM이 자동 매핑 ↕️

// Java 객체 (Entity)
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    private int age;
    
    // getters, setters...
}
```

### 🎯 ORM에서 말하는 "Object"
**답: OOP의 객체(클래스 인스턴스)**
- JPA Entity는 클래스 기반으로 정의
- OOP 원칙을 따라 설계
- 데이터베이스 테이블과 1:1 매핑되는 **도메인 객체**

---

## ⚡ Spring Boot Service Layer에서의 실전 적용

### 🏗️ 전체 아키텍처 흐름
```
Controller → Service → Repository → Database
     ↑           ↓
   DTO        Entity
```

### 💻 실제 코드 예시

#### 1️⃣ Entity 정의
```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    private String email;
    private int age;
    
    // constructors, getters, setters...
}
```

#### 2️⃣ Repository Layer
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
    List<User> findByAgeGreaterThan(int age);
}
```

#### 3️⃣ Service Layer (핵심! 🔥)
```java
@Service
@Transactional
public class UserService {
    
    private final UserRepository userRepository;
    
    // CREATE: DTO → Entity → DTO
    public UserResponseDto createUser(UserCreateDto createDto) {
        // 1. DTO를 Entity로 변환
        User user = User.builder()
                .name(createDto.getName())
                .email(createDto.getEmail())
                .age(createDto.getAge())
                .build();
        
        // 2. ORM이 Entity를 DB에 저장
        User savedUser = userRepository.save(user);
        
        // 3. Entity를 Response DTO로 변환하여 반환
        return UserResponseDto.from(savedUser);
    }
    
    // READ: Entity → DTO
    public UserResponseDto getUser(Long userId) {
        // 1. ORM이 DB에서 데이터를 조회하여 Entity 생성
        User user = userRepository.findById(userId)
                .orElseThrow(() -> new UserNotFoundException("사용자를 찾을 수 없습니다."));
        
        // 2. Entity를 Response DTO로 변환
        return UserResponseDto.from(user);
    }
    
    // UPDATE: DTO + Entity → DTO
    public UserResponseDto updateUser(Long userId, UserUpdateDto updateDto) {
        // 1. 기존 Entity 조회
        User user = userRepository.findById(userId)
                .orElseThrow(() -> new UserNotFoundException("사용자를 찾을 수 없습니다."));
        
        // 2. Entity 상태 변경 (Dirty Checking)
        user.updateInfo(updateDto.getName(), updateDto.getAge());
        
        // 3. @Transactional에 의해 자동 저장
        // 4. Entity를 Response DTO로 변환
        return UserResponseDto.from(user);
    }
}
```

#### 4️⃣ DTO 정의
```java
// Response DTO
public class UserResponseDto {
    private Long id;
    private String name;
    private String email;
    private int age;
    
    // Entity → DTO 변환 메서드
    public static UserResponseDto from(User user) {
        return UserResponseDto.builder()
                .id(user.getId())
                .name(user.getName())
                .email(user.getEmail())
                .age(user.getAge())
                .build();
    }
}
```

---

## 🔄 Entity vs DTO: 언제, 왜 변환하는가?

### 🎭 각자의 역할

#### 🏛️ Entity의 역할
- **도메인 로직 담당**: 비즈니스 규칙과 제약사항 포함
- **데이터베이스와 직접 매핑**: ORM이 관리하는 영속성 객체
- **생명주기 관리**: JPA가 추적하고 관리

#### 📦 DTO의 역할
- **데이터 전송 전용**: 계층 간 데이터 이동
- **API 스펙 정의**: 클라이언트와의 인터페이스
- **보안**: 필요한 데이터만 노출

### ⚙️ 변환하는 이유

1. **보안** 🔒: Entity의 모든 필드를 노출하면 안 됨
2. **유지보수** 🔧: API 스펙과 도메인 모델의 독립성
3. **성능** ⚡: 필요한 데이터만 전송
4. **유연성** 🤸: 클라이언트 요구사항에 맞는 응답 구조

### 💡 Service Layer에서의 핵심 패턴

```java
// 📥 Input: 클라이언트로부터 DTO 받기
// 🔄 Process: Entity로 변환하여 비즈니스 로직 처리
// 📤 Output: Entity를 DTO로 변환하여 응답
```

---

## 🎉 정리

### 🔑 핵심 포인트
1. **ORM의 "Object"는 OOP의 객체(Entity)** 를 의미
2. **Service Layer는 DTO ↔ Entity 변환의 중심지**
3. **Entity는 도메인 로직**, **DTO는 데이터 전송**의 역할 분담
4. **ORM은 Entity와 DB 테이블 간의 자동 매핑** 담당

### 💪 실무에서 기억할 것
- Entity는 비즈니스 로직과 데이터베이스 매핑을 담당
- DTO는 API 계층에서만 사용하여 외부 의존성 차단
- Service Layer에서 두 객체 간 변환 로직을 명확히 구현
- @Transactional과 Dirty Checking을 활용한 효율적인 UPDATE 처리
