---
title: "☕️[Java] 테스트 코드와 Dependancy Injection"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-08-30"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] 테스트 코드와 Dependancy Injection

## 1️⃣ 전체 코드.

```java
// Member
public class Member {

	private Long id;
	private String name;

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
}

// MemberRepository - Interface
import com.devkobe.hello_spring.domain.Member;
import java.util.List;
import java.util.Optional;

public interface MemberRepository {
	Member save(Member member);
	Optional<Member> findById(Long id);
	Optional<Member> findByName(String name);
	List<Member> findAll();
}

// MemoryMemberRepository
import com.devkobe.hello_spring.domain.Member;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
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

// MemberService
import com.devkobe.hello_spring.domain.Member;
import com.devkobe.hello_spring.repository.MemberRepository;
import com.devkobe.hello_spring.repository.MemoryMemberRepository;
import java.util.List;
import java.util.Optional;

public class MemberService {
	private final MemberRepository memberRepository;

	public MemberService(MemberRepository memberRepository) {
		this.memberRepository = memberRepository;
	}

	/*
	 * 회원 가입
	 */
	public Long join(Member member) {

		validateDuplicateMember(member); // 중복 회원 검증
		memberRepository.save(member);
		return member.getId();
	}

	private void validateDuplicateMember(Member member) {
		memberRepository.findByName(member.getName())
				.ifPresent(m -> {
					throw new IllegalStateException("이미 존재하는 회원입니다.");
				});
	}

	/*
	 * 전체 회원 조회
	 */
	public List<Member> findMembers() {
		return memberRepository.findAll();
	}

	public Optional<Member> findOne(Long memberId) {
		return memberRepository.findById(memberId);
	}
}

// MemberServiceTest
import com.devkobe.hello_spring.domain.Member;
import com.devkobe.hello_spring.repository.MemoryMemberRepository;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.*;

class MemberServiceTest {

	MemberService memberService;
	MemoryMemberRepository memberRepository;

	@BeforeEach
	public void beforeEach() {
		memberRepository = new MemoryMemberRepository();
		memberService = new MemberService(memberRepository);
	}

	@AfterEach
	public void afterEach() {
		memberRepository.clearStore();
	}

	@Test
	void 회원가입() {
		// given
		Member member = new Member();
		member.setName("spring");

		// when
		Long saveId = memberService.join(member);

		// then
		Member findMember = memberService.findOne(saveId).get();
		assertThat(member.getName()).isEqualTo(findMember.getName());
	}

	@Test
	void findMembers() {
		// given
		Member member1 = new Member();
		member1.setName("spring");

		Member member2 = new Member();
		member2.setName("spring");

		// when
		memberService.join(member1);
		IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));

		assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");

		// then
	}

	@Test
	void findOne() {
	}
}
```

## 2️⃣ 의존성 주입(DI, Dependency Injection).
- **의존성 주입(DI, Dependency Injection)** 는 객체 지향 프로그래밍에서 객체 간의 의존성을 외부에서 주입하는 디자인 패턴입니다.

### 1️⃣ 의존성 주입(DI, Dependency Injection)의 개념.
- **의존성(Dependency)**
    - 클래스가 다른 클래스의 기능을 사용해야 하는 상황을 의미합니다.
        - 예를 들어, **`MemberService`** 클래스는 회원 데이터를 처리하기 위해 **`MemberRepository`** 를 필요로 합니다. 여기서 **`MemberService`** 는 **`MemberRepository`** 에 의존합니다.
- **주입(Injection)**
    - 외부에서 객체를 생성하여 주입해주는 것을 의미합니다.
        - 이는 보통 생성자 주입, 세터(Setter) 주입, 필드 주입 등의 방식으로 이루어집니다.

### 2️⃣ DI의 주요 목적.
- **느슨한 결합(Loose Coupling)**
    - DI를 통해 클래스 간의 결합도를 낮출 수 있습니다.
        - 클래스는 자신이 사용하는 의존 객체가 무엇인지 알 필요가 없고, 이로 인해 클래스 간의 결합이 느슨해집니다.
            - 이는 코드의 유연성을 높여주며, 코드 변셩 시 영향 범위를 줄여줍니다.
- **유연성 및 확장성**
    - 의존 객체를 외부에서 주입받기 때문에, 애플리케이션이 다른 의존 객체로 쉽게 전환될 수 있습니다.
        - 예를 들어, 테스트 환경에서 **`MemoryMemberRepository`** 를 사용하고, 실제 운영 환경에서는 데이터베이스 연결을 사용하는 **`JpaMemberRepository`** 를 사용할 수 있습니다.
- **테스트 용이성**
    - DI를 통해 클래스 간의 의존 관계를 외부에서 주입받으면, 테스트 시에 쉽게 Mock 객체나 Stub 객체를 주입할 수 있어 테스트를 더 용이하게 만듭니다.

### 3️⃣ DI의 사용 방법.
- **1. 생성자 주입(Constructor Injection) :** 의존성을 클래스의 생성자를 통해 주입하는 방법입니다. 위 코드에서는 생성자 주입이 사용되었습니다.

```java
public MemberService(MemberRepository memberRepository) {
    this.memberRepository = memberRepository;
}
```

- **2. 세터 주입(Setter Injection) :** 의존성을 세터 메서드를 통해 주입하는 방법입니다.
```java
public void setMemberRepository(MemberRepository memberRepository) {
    this.memberRepository = memberRepository;
}
```

- **3. 필드 주입(Field Injection) :** 의존성을 필드에 직접 주입하는 방법으로, 보통 **`@Autowired`** 와 같은 애노테이션을 사용합니다. 하지만 필드 주입은 테스트하기 어려울 수 있어 일반적으로 권장되지 않습니다.

DI는 코드의 재사용성, 유지보수성, 테스트 용이성을 높여주는 중요한 디자인 패턴으로, 스프링 프레임워크와 같은 의존성 주입 컨테이너에서 널리 사용됩니다.

## 3️⃣ 코드에서 의존성 주입(DI, Dependency Injection)이 사용된 부분과 설명.
- 위 코드에서 DI(Dependency Injection)가 사용된 부분은 **`MemberService`** 클래스의 인스턴스를 생성하는 부분입니다.

```java
@BeforeEach
public void beforeEach() {
    memberRepository = new MemoryMemberRepository();
    memberService = new MemberService(memberRepository);
}
```

- 이 부분에서 **`MemberService`** 클래스의 생성자에 **`MemoryMemberRepository`** 객체를 주입하고 있습니다.
- **`MemberService`** 클래스는 생성자에서 **`MemberRepository`** 인터페이스를 받아 사용합니다.
    - 아렇게 함으로써, **`MemberService`** 는 직접적으로 **`MemoryMemberRepository`** 를 생성하지 않고 외부에서 주입받게 됩니다.

### 1️⃣ 설명.
- **`memberRepository = new MemoryMemberRepository();`**
    - **`MemoryMemberRepository`** 객체를 생성합니다. 이는 **`MemberRepository`** 인터페이스의 구현체입니다.
- **`memberService = new MemberService(memberRepository);`**
    - **`MemberService`** 객체를 생성 할 때, 생성자에 **`memberRepository`** 를 주입합니다.

이 방식은 **`MemberService`** 가 **`MemoryMemberRepository`** 에 강하게 결합되지 않도록 하여, 다른 구현체로 쉽게 변경할 수 있게 만듭니다.

예를 들어, 테스트 환경에서는 **`MemoryMemberRepository`** 를 사용하고, 실제 서비스에서는 데이터베이스와 연결된 구현체를 사용할 수 있습니다.
