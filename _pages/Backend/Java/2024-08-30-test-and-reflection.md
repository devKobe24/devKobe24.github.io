---
title: "☕️[Java] 테스트 코드와 Reflection."
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-08-30"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] 테스트 코드와 Reflection.

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

public class MemberService {
	private final MemberRepository memberRepository = new MemoryMemberRepository();

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
}
```

## 2️⃣ MemberService를 Test.
```java
import com.devkobe.hello_spring.domain.Member;
import com.devkobe.hello_spring.repository.MemoryMemberRepository;
import java.lang.reflect.Field;
import java.util.List;
import java.util.Optional;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.assertThrows;

class MemberServiceTest {

	MemberService memberService;
	MemoryMemberRepository memberRepository;

	@BeforeEach
	public void setUp() throws Exception {
		memberService = new MemberService();
		memberRepository = new MemoryMemberRepository();

		// Reflection을 사용하여 memberRepository 필드에 값을 설정.
		Field repositoryField = MemberService.class.getDeclaredField("memberRepository");
		repositoryField.setAccessible(true);
		repositoryField.set(memberService, memberRepository);
	}

	@AfterEach
	public void afterEach() {
		memberRepository.clearStore();
	}

	@Test
	public void 회원가입() {
		// given
		Member member = new Member();
		member.setName("spring");

		// when
		Long savedId = memberService.join(member);

		// then
		Optional<Member> foundMember = memberRepository.findById(savedId);
		assertThat(foundMember.isPresent()).isTrue();
		assertThat(foundMember.get().getName()).isEqualTo("spring");
	}

	@Test
	public void 중복_회원_제외() {
		// given
		Member member1 = new Member();
		member1.setName("spring");

		Member member2 = new Member();
		member2.setName("spring");

		// join
		memberService.join(member1);

		// then
		IllegalStateException exception = assertThrows(IllegalStateException.class, () -> {
			memberService.join(member2);
		});

		assertThat(exception.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
	}

	@Test
	public void 전체회원조회() {
		// given
		Member member1 = new Member();
		member1.setName("spring1");

		Member member2 = new Member();
		member2.setName("spring2");

		memberService.join(member1);
		memberService.join(member2);

		// when
		List<Member> members = memberService.findMembers();

		// then
		assertThat(members.size()).isEqualTo(2);
		assertThat(members).contains(member1, member2);
	}
}
```

## 3️⃣ 모르는 코드 설명.
- 1. **`"@BeforeEach"`** 애노테이션.
    - JUnit 5에서 테스트 메서드가 실행되기 전에 매번 호출되는 메서드에 사용됩니다.
    - 이 애노테이션이 붙은 메서드는 각 테스트 메서드가 실행되기 직전에 실행되므로, 테스트 환경을 초기화하거나 준비 작업을 수행하는 데 유용합니다.
    - **🙋‍♂️ 이 애노테이션의 주요 역할은 다음과 같습니다.**
        - 1. **테스트 환경 초기화**
            - 각 테스트 메서드가 실행될 때마다 동일한 초기 상태를 보장하기 위해 사용됩니다.
                - 예를 들어, 테스트할 객체를 새로 생성하거나, 필요한 데이터를 설정하는 등의 작업을 수행합니다.
        - 2. **반복 작업 처리**
            - 여러 테스트에서 반복적으로 수행해야 하는 설정 작업이 있을 때, **`"@BeforeEach"`** 를 사용하여 중복 코드를 줄일 수 있습니다.
        - 3. **독립적인 테스트 보장**
            - 테스트 간의 상호 의존성을 없애고, 각 테스트가 독립적으로 실행되도록 보장할 수 있습니다.
                - 이를 통해 테스트 간에 상태가 공유되지 않도록 하여 신뢰성 있는 테스트를 구현할 수 있습니다.
    - **`"@BeforeEach"`** 는 테스트 환경을 일관되게 유지하고, 
- 2. **`"@AfterEach"`** 애노테이션.
    - JUnit 5에서 각 테스트 매서드가 실행된 후에 실행되는 메서드를 붙이는 애노테이션 입니다.
    - 이 메서드는 테스트가 완료된 후에 정리(clean-up) 작업을 수행하는 데 사용됩니다.
    - **🙋‍♂️ 이 애노테이션의 주요 역할은 다음과 같습니다.**
        - 1. **자원 정리**
            - 테스트 중에 사용된 자원(예: 파일, 데이터베이스 연결, 네트워크 연결 등)을 해제하거나 정리하는 데 사용됩니다.
                - 이는 메모리 누수나 리소스 잠금을 방지할 수 있습니다.
        - 2. **테스트 환경 복원**
            - 테스트 실행 중에 변경된 상태나 데이터를 초기 상태로 되돌려, 다른 테스트에 영향을 미치지 않도록 합니다.
                - 이는 테스트 간의 독립성을 유지하는 데 중요한 역할을 합니다.
        - 3. **로그 남기기**
            - 테스트가 끝난 후 테스트 결과나 상태에 대한 로그를 기록할 수 있습니다.
                - 이를 통해 테스트 결과를 모니터링하거나 디버깅할 때 유용할 수 있습니다.
    - **`"@AfterEach"`** 는 테스트 후에 정리 작업을 자동으로 수행하여, 코드의 안정성과 유지보수성을 높이는 데 중요한 역할을 합니다.
- 3. **Reflection**
    - **Reflection** 은 자바에서 런타임 시에 클래스, 인터페이스, 메서드, 필드 등의 정보를 동적으로 조사하고, 조작할 수 있는 기능을 제공합니다.
    - **Reflection** 을 사용하면 코드에서 특정 객체의 클래스 타입이나 메서드, 필드 등에 접근하고, 해당 요소들을 동적으로 호출하거나 값을 변경하는 것이 가능합니다.
    - **🙋‍♂️ Reflection의 주요 개념과 역할은 다음과 같습니다.**
        - 1. **클래스 정보 조사**
            - Reflection을 사용하면 특정 객체의 클래스 타입을 런타임에 알아낼 수 있습니다.
                - 예를 들어, **`Class<?> clazz = obj.getClass();`** 를 사용하여 객체 **`obj`** 의 클래스 정보를 가져올 수 있습니다.
        - 2. **필드, 메서드, 생성자 접근**
            - Reflection을 통해 클래스에 선언된 필드, 메서드 ,생성자에 접근할 수 있습니다.
                - 이를 통해 특정 필드의 값을 가져오거나 설정하고, 메서드를 호출하거나 생성자를 통해 객체를 생성할 수 있습니다.
                    - 예를 들어, **`Field field = clazz.getDeclaredField("fieldName);"`** 를 사용하여 특정 필드에 접근할 수 있습니다.
        - 3. **접근 제어 무시**
            - Reflection을 사용하면 **`private`** 으로 선언된 필드나 메서드에도 접근할 수 있습니다. **`setAccessible(true)`** 메서드를 사용하여 접근 제어자를 무시할 수 있습니다.
                - 이는 보통 테스트나 프레임워크에서 사용되며 예를 들어, 프레임워크에서 자동으로 의존성을 주입하거나, 테스트에서 private 필드에 접근할 때 유용합니다.
        - 4. **런타임에 동적 객체 생성 및 메서드 호출**
            - Reflection을 사용하여 런타임에 동적으로 객체를 생성하거나 메서드를 호출할 수 있습니다.
                - 이는 매우 유연한 코드 작성을 가능하게 하지만, 일반적으로 성능 저하가 있을 수 있습니다.
        - 5. **애노테이션 처리**
            - Reflection을 사용하여 클래스나 메서드에 선언된 애노테이션을 런타임에 읽어들이고 처리할 수 있습니다.
                - 이는 주로 프레임워크에서 사용되며, 예를 들어, 스프링 프레임워크에서 애노테이션 기반으로 설정을 처리하는 경우가 있습니다.
    - **🙋‍♂️ Reflection의 장단점은 다음과 같습니다.**
        - **장점**
            - **동적 기능 제공 :** 코드의 유연성과 확장성을 높여줍니다. 런타임에 클래스나 메서드를 동적으로 호출할 수 있어, 컴파일 타임에 알 수 없는 구조를 처리할 수 있습니다.
            - **프레임워크에서 유용 :** 많은 자바 프레임워크, 예를 들어 스프링(Spring), 하이버네이트(Hibernate) 등은 Reflection을 활용하여 애플리케이션의 동작을 제어합니다.
        - **단점**
            - **성능 이슈 :** Reflection은 일반적인 메서드 호출에 비해 성능이 떨어질 수 있습니다. 따라서 중요한 애플리케이션에서는 중의가 필요합니다.
            - **안전성 문제 :** Reflection을 사용하면 컴파일 타임에 확인할 수 없는 동작이 많아, 잘못 사용하면 런타임 에러가 발생할 수 있습니다.
            - **보안 이슈 :** Reflection은 접근 제어를 무시할 수 있으므로, 잘못된 사용은 보안상의 취약점을 초래할 수 있습니다.

## 4️⃣ 코드 설명.
- 1. **Reflection을 사용하여 `memberRepository`** 설정.
    - **`MemberService`** 의 **`memberRepository`** 필드는 **`private final`** 로 선언되어 있으므로, 일반적인 방식으로 접근할 수 없습니다.
        - 이 문제를 해결하기 위해 **`Reflection`** 을 사용하여 필드에 접근하고, 테스트용 **`MemoryMemberRepository`** 를 설정합니다.
- 2. **`@BeforeEach`** 애노테이션.
    - 각 테스트가 실행되기 전에 **`MemberService`** 인스턴스를 생성하고. **`Reflection`** 을 사용하여 **`memberRepository`** 필드를 **`MemoryMemberRepository`** 인스턴스로 초기화합니다.
- 3. **`@AfterEach`** 애노테이션.
    - 각 테스트가 완료된 후, **`MemoryMemberRepository`** 의 저장소를 초기화하여 테스트 간의 상태 간섭을 방지합니다.
- 4. **테스트 메서드.**
    - **`회원가입` :** 새로운 회원을 가입시키고, 저장된 회원이 올바르게 반환되는지 검증합니다.
    - **`중복_회원_제외` :** 중복된 이름으로 회원을 가입하려 할 때 **`IllegalStateException`** 이 발생하는지를 확인합니다.
    - **`전체회원조회` :** 저장된 모든 회원이 올바르게 반환되는지 검증합니다.

이 방법을 통해 **`MemberService`** 를 수정하지 않고도 해당 클래스의 동작을 테스트할 수 있습니다.
다만, 실제 코드에서는 생성자를 통한 주입이나 다른 테스트 가능한 구조로 변경하는 것이 더 바람직합니다.
