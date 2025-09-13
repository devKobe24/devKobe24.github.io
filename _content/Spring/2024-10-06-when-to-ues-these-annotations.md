---
title: 🍃[Spring] 언제 `@Service`,`@Repository`,`@Controller`와 같은 어노테이션을 사용할까?
tags:
    - Spring
    - Framework
date: "2024-10-06"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 언제 `@Service`,`@Repository`,`@Controller`와 같은 어노테이션을 사용할까?

`@Service`,`@Repository`,`@Controller`와 같은 어노테이션은 Spring Framework에서 **특정 레이어의 역할을 명확히 하고, 자동으로 빈을 등록할 때 사용됩니다.**

이 어노테이션들은 `@Component` 어노테이션의 특수화된 버전으로, 각각의 레이어를 구분하여 Spring 애플리케이션을 더 구조화하고, 책임을 명확하게 하기 위해 사용됩니다.

## 1️⃣ `@Service`
- **사용 시점**
    - **비즈니스 로직을** 처리하는 서비스 계층에서 사용됩니다.
- **설명**
    - `@Service`는 애플리케이션 내에서 **핵심 비즈니스 로직**을 구현하는 클래스에 붙입니다.
    - 이 계층은 컨트롤러에서 전달된 요청을 처리하고, 데이터를 조작하거나 다른 비즈니스 규칙을 적용합니다.
    - 또한, 이 계층은 트랜잭션 관리나 예외 처리와 같은 중요한 작업도 수행할 수 있습니다.
- **사용 예시**
```java
@Service
public class UserService {
    public User findUserById(Long id) {
        // 비즈니스 로직 처리
        return userRepository.findById(id).orElseThrow();
    }
}
```

- **사용 목적**
    - `@Service`를 사용함으로써 해당 클래스가 비즈니스 로직을 담당하는 서비스 계층의 역할을 한다는 점을 명확히 하고, Spring 컨테이너에 의해 자동으로 빈으로 등록되게 합니다.
    - 또한, `@Service` 어노테이션을 통해 Spring이 해당 클래스에 대해 추가적인 처리를 적용할 수 있습니다(예: 트랜잭션 관리).

## 2️⃣ `@Repository`
- **사용 시점**
    - **데이터 접근 계층(DAO, Data Access Object)** 에서 사용됩니다.
- **설명**
    - `@Repository`는 데이터베이스와 상호작용하는 **영속성 계층**에서 사용됩니다.
    - 보통 데이터베이스 CRUD 작업을 수행하며, 데이터베이스와의 직접적인 연결, 쿼리 실행, 결과 처리 등을 담당합니다.
    - Spring에서는 `@Repository`를 사용하여 **데이터베이스 예외를 Spring 예외로 변환**하는 등의 추가적인 기능도 제공합니다.
- **사용 예시**
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // 데이터베이스 작업을 위한 메서드 정의
}
```

- **사용 목적**
    - `@Repository`는 해당 클래스가 데이터베이스와 상호작용하는 DAO 역할을 한다는 점을 명확히하고, 자동으로 빈으로 등록되게 합니다.
    - 또한, `@Repository`는 데이터베이스와 관련된 예외를 표준화된 Spring 예외로 변환하는 기능을 제공합니다.

## 3️⃣ `@Controller`
- **사용 시점**
    - **웹 계층(프레젠테이션 계층)** 에서 사용됩니다.
- **설명**
    - `@Controller`는 **사용자 요청**을 처리하고, 적절한 응답을 반환하는 역할을 하는 **웹 컨트롤러 클래스에 사용됩니다.**
    - 주로, Spring MVC 애플리케이션에서 사용되며, 클라이언트 요청을 받아 비즈니스 로직을 처리하고, 결과를 HTML 페이지나 JSON 형식으로 반환합니다.
- **사용 예시**
```java
@Controller
public class UserController {
    @GetMapping("/user/{id}")
    public String getUser(@PathVariable Long id, Model model) {
        User user = userServie.findUserById(id);
        model.addAttribute("user", user);
        return "userDetail";
    }
}
```

- **사용 목적**
    - `@Controller`는 해당 클래스가 웹 요청을 처리하는 컨트롤러임을 명확히 하며, Spring 컨테이너에 의해 자동으로 빈으로 등록됩니다.
    - 웹 요청을 받아서 처리하고, 응답을 생성하는 역할을 하기 때문에 사용자와 애플리케이션 간의 인테페이스 역할을 합니다.

## 4️⃣ `@RestController`
- **사용 시점**
    - **RESTful 웹 서비스 계층에서 사용됩니다.**
- **설명**
    - `@RestController`는 `@Controller`와 `@ResponseBody`가 결합된 어노테이션으로, 주로 **JSON** 또는 **XML 형식**의 데이터를 반환하는 **RESTful API**를 만들 때 사용됩니다.
    - Spring MVC에서 데이터를 직렬화하여 클라이언트에게 전송할 때 사용됩니다.
- **사용 예시**
```java
@RestController
public class UserRestController {
    @GetMapping("/api/users/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findUserById(id);
    }
}
```
- **사용 목적**
    - `@RestController`는 주로 **REST API** 를 개발할 때 사용되며, 컨트롤러에서 반환하는 데이터를 HTML 페이지가 아닌 JSON이나 XML과 같은 형식으로 반환합니다.
    - REST API 설계를 할 때 이 어노테이션을 사용하면 개발자의 의도를 명확히 전달할 수 있습니다.

## 5️⃣ 언제 사용해야 하는가?
- **`@Service`**
    - 비즈니스 로직을 담당하는 클래스에 사용합니다.
    - 데이터 접근 계층에서 가져온 데이터를 가공하거나 규칙을 적용하는 등의 작업을 수행하는 곳입니다.
- **`@Repository`**
    - 데이터베이스와의 상호작용을 처리하는 클래스에 사용합니다.
    - 주로 데이터 저장, 수정, 조회, 삭제와 같은 영속성 로직이 포함된 DAO 또는 리포지토리에 붙입니다.
- **`@Controller`**
    - 사용자로부터 HTTP 요청을 받아 응답을 생성하는 웹 컨트롤러에 사용합니다.
    - 주로 Spring MVC에서 동적인 웹 페이즈를 랜더링할 때 사용됩니다.
- **`@RestController`**
    - RESTful 웹 서비스를 제공하는 컨트롤러에 사용합니다.
    - 이 어노테이션을 사용하면 웹 요청을 처리한 후 JSON 또는 XML 형식의 데이터를 반환할 수 있습니다.

## 6️⃣ 결론
- 이 어노테이션들은 각각의 클래스가 어떤 역할을 담당하는지 명확히 구분해 줌으로써, Spring 애플리케이션의 구조화와 관리에 도움을 줍니다.
- Spring 컨테이너는 이 어노테이션이 붙은 클래스들을 자동으로 감지하여 빈으로 등록하고, 필요한 곳에 의존성을 주입해줍니다.
