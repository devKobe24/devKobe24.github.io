---
title: 🍃[Spring] 자바 코드로 직접 스프링 빈 등록하기.
tags:
    - Spring
    - Framework
date: "2024-09-03"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 자바 코드로 직접 스프링 빈 등록하기.
- 스프링에서 자바 코드로 스프링 빈을 직접 등록하는 방법은 주로 **`@Configuration`** 애노테이션과 **`@Bean`** 애노테이션을 사용하여 이루어 집니다.
    - 이 방식은 XML 설정 파일 대신 자바 클래스를 사용하여 스프링 빈을 정의하고 관리하는 방법입니다.

## 1️⃣ `@Configuration` 과 `@Bean` 을 사용한 빈 등록
- **`@Configuration`**
    - 이 애노테이션은 해당 클래스가 하나 이상의 **`@Bean`** 메서드를 포함하고 있으며, 스프링 컨테이너에서 빈 정의를 생성하고 처리할 수 있는 설정 클래스임을 나타냅니다.
- **`@Bean`**
    - 이 애노테이션은 메서드 레벨에서 사용되며, 메서드의 리턴값이 스프링 컨테이너에 의해 관리되는 빈(Bean)이 됨을 나타냅니다.

## 2️⃣ 예시.
아래는 **`MemoryMemberRepository`** 클래스를 자바 코드로 스프링 빈으로 등록하는 방법을 보여주는 예시입니다.
- **1. 빈으로 등록할 클래스 정의**
```java
package com.devkobe.hello_spring.repository;

import com.devkobe.hello_spring.domain.Member;
import java.util.HashMap;
import java.util.Map;
import java.util.Optional;

public class MemoryMemberRepository implements MemberRepository {

    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id));
    }

    @Override
    public Optional<Member> findByName(String name) {
        return store.values().stream()
                .filter(member -> member.getName().equals(name))
                .findAny();
    }

    @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }

    public void clearStore() {
        store.clear();
    }
}
```

- **2. 자바 설정 파일에서 빈 등록**
```java
package com.devkobe.hello_spring.config;

import com.devkobe.hello_spring.repository.MemberRepository;
import com.devkobe.hello_spring.repository.MemoryMemberRepository;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {
    
    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
}
```

- **3. 스프링 컨테이너에서 빈 사용**
```java
package com.devkobe.hello_spring.service;

import com.devkobe.hello_spring.repository.MemberRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class MemberService {
    
    private final MemberRepository memberRepository;
    
    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
    
    // 비즈니스 로직 메서드들...
}
```

## 3️⃣ 설명.
- **`AppConfig` 클래스**
    - **`@Configuration`** 애노테이션을 사용하여 이 클래스가 스프링 설정 클래스로 사용될 것임을 명시합니다.
    - **`memberRepository()`** 메서드는 **`@Bean`** 애노테이션으로 정의되어 있으며, 이 메서드의 리턴값이 스프링 컨테이너에 의해 관리되는 빈이 됩니다.
        - 이 경우, **`MemoryMemberRepository`** 객체가 빈으로 등록됩니다.
- **빈 사용**
    - **`MemberService`** 클래스에서 **`MemberRepository`** 타입의 빈이 생성자 주입 방식으로 주입됩니다.
        - 이때, **`AppConfig`** 클래스에서 등록된 **`MemoryMemberRepository`** 빈이 주입됩니다.

## 4️⃣ 왜 자바 설정을 사용할까?
- **1. 타입 안정성**
    - 자바 코드는 컴파일 시점에 타입을 체크할 수 있으므로, XML보다 타입 안정성이 높습니다.
- **2. IDE 지원**
    - 자바 기반 설정은 IDE의 자동 완성 기능과 리팩토링 도구를 잘 지원받을 수 있습니다.
- **3. 코드 재사용성**
    - 자바 설정 클래스는 일반 자바 코드처럼 재사용 사능하며, 상속과 조합 등을 활용할 수 있습니다.

## 5️⃣ 결론
- 스프링에서 자바 코드로 빈을 등록하는 방법은 **`@Configuration`** 과 **`@Bean`** 애노테이션을 사용한 방법입니다.
    - 이 방식은 XML 기반 설정보다 더 타입 안전하고, 유지보수하기 쉬우며, 현대적인 스프링 애플리케이션에서 자주 사용됩니다.
