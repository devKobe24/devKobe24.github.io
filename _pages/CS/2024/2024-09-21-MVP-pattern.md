---
title: "💾 [CS] MVP 패턴."
tags:
    - CS
date: "2024-09-21"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] MVP 패턴.

## 1️⃣ MVP 패턴.
- MVP 패턴은 MVC 패턴으로부터 파생되었으며 MVC에서 C에 해당하는 컨트롤러가 프레젠터(Presenter)로 교체된 패턴입니다.

<img src = "https://github.com/devKobe24/images2/blob/main/CS_IMG/cs-mvp.png?raw=true">

- 뷰와 프레젠터는 일대일 관계이기 때문에 MVC 패턴보다 더 강한 결합을 지닌 디자인 패턴이라고 볼 수 있습니다.

## 2️⃣ 자바에서의 MVP 패턴.
- 자바에서의 MVP 패턴은 주로, 데스크탑 애플리케이션(JavaFX, Swing) 또는 안드로이드 애플리케이션을 개발할 때 많이 사용됩니다.
- 자바에서는 주로 MVC 패턴이 많이 사용되지만, MVP 패턴은 UI와 비즈니스 로직을 더욱 명확하게 분리할 수 있기 때문에 상황에 따라 더 적합할 수 있습니다.

## 3️⃣ MVP 패턴의 구조.
- MVP는 Model-View-Presenter의 약자로, 아래와 같은 세 가지 주요 구성 요소로 나뉩니다.
- **1. Model(모델)**
    - 애플리케이션의 데이터와 비즈니스 로직을 처리합니다.
    - 데이터베이스와 상호작용하고 데이터를 가공하는 역할을 담당합니다.
    - 예: 데이터베이스 접근, API 호출, 데이터 가공.
- **2. View(뷰)**
    - 사용자 인터페이스(UI)를 담당하며, 사용자가 보는 화면을 표시하고 입력을 받습니다.
    - View는 Presenter에 의존하여 데이터를 요청하고, 그 데이터를 표시하는 역할을 합니다.
    - 예: 자바의 `JPanel`, `JFrame`(Swing) 또는 `Activity`, `Fragment`(안드로이드)
- **3. Presenter(프레젠터)**
    - View와 Model간의 중재자 역할을 하며, 뷰에서 발생한 사용자 상호작용을 처리하고, 필요한 데이터를 모델에서 가져와 뷰에 전달합니다.
    - 비즈니스 로직을 처리하며, View와 Model을 직접 연결하지 않고 독립적으로 관리합니다.
    - Presenter는 View 인터페이스를 통해 View와 통신하고, 테스트 가능한 구조를 만듭니다.

## 4️⃣ 백엔드를 Java로 구현시 MVP 패턴이 사용되나요?
- 일반적으로 **MVP패턴(Model-View-Presenter)** 은 주로 **프론트엔드** 또는 **UI 중심** 애플리케이션에서 사용됩니다.
- MVP 패턴은 사용자 인터페이스와 비즈니스 로직을 분리하는 데 중점을 두기 때문에, 데스크탑 애플리케이션(JavaFX, Swing)이나 모바일 애플리케이션(안드로이드)에서 많이 사용됩니다.
- 따라서 **백엔드 애픝리케이션**을 Java로 구현할 때는 MVP 패턴이 거의 사용되지 않으며, 그 대신 다른 디자인 패턴이 주로 사용됩니다.

### 1. MVP 패턴의 목적.
- MVP 패턴은 기본적으로 사용자 인터페이스(UI)를 중심으로 **View**와 **비즈니스 로직(Presenter)** 을 분리하는 데 목적이 있습니다.
- 하지만 백엔드 애플리케이션은 사용자 인터페이스가 아닌 **서버 측 비즈니스 로직, 데이터 처리, API 제공** 등을 다루기 때문에, UI 요소가 존재하지 않습니다.
- 따라서 **View** 라는 개념이 백엔드에 적합하지 않습니다.

### 2. 백엔드에서는 MVC 패턴이 더 적합.
- Java 기반 백엔트 개발에서는 **MVC(Model-View-Controller) 패턴** 이나 **서비스 계층 패턴** 과 같은 구조가 더 일반적입니다.
- 특히, **Spring Framework** 같은 인기 있는 백엔드 프레임워크에서는 **MVC 패턴**이 기본적으로 사용됩니다.
- 백엔드에서 **컨트롤러(Controller)** 가 클라이언트의 요청을 처리하고, **모델(Model)** 이 데이터 처리와 비즈니스 로직을 담당하며, **뷰(View)** 는 API 응답(JSON, XML 등)을 생성하는 역할을 합니다.

#### 백엔드에서 자주 사용되는 디자인 패턴.
- **1. MVC 패턴(Model-View-Controller)**
    - 서버 요청을 처리하고, 데이터베이스와 상호작용하며, API 응답을 생성하는 데 사용됩니다.
- **2. 서비스 계층 패턴**
    - 비즈니스 로직을 서비스 계층으로 분리하여 재사용성과 유지보수성을 높이는 패턴입니다.
- **3. Repository 패턴**
    - 데이터베이스 액세스 로직을 추상화하여, 비즈니스 로직과 데이터 액세스를 분리합니다.
- **4. Command 패턴**
    - 사용자의 요청이나 명령을 객체로 변환하여 처리하는 방식으로, 여러 요청을 관리하는데 유리합니다.
- **5. Observer 패턴**
    - 상태 변화를 여러 객체가 구독하고 반응하는 패턴으로, 이벤트 기반 시스템에 자주 사용됩니다.

### 3. 백엔드에서 사용하는 디자인 패턴의 예시.
- **1. Spring MVC 패턴**
    - Spring에서는 **Controller**가 **HTTP** 요청을 받고, **Service**에서 비즈니스 로직을 처리한 뒤, **Model**을 사용하여 데이터를 전달하고 **View**를 반환하는 전형적인 MVC 패턴을 사용합니다.
    - 여기서 View는 HTML 또는 JSON, XML과 같은 응답 포맷을 의미합니다.

```java
@Controller
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.findUserById(id);
        return new ResponseEntity<>(user, HttpStatus.OK);
    }
}
```
- **Controller** 는 클라이언트 요청을 처리하고, 데이터를 가공한 후 응답합니다.
- **Service** 는 비즈니스 로직을 처리하는 중간 계층 역할을 합니다.
- **Repository** 는 데이터베이스와 상호작용하는 부분입니다.

- **2. 서비스 계층 패턴**
    - 서비스 계층을 사용하면 컨트롤러가 직접 비즈니스 로직을 다루지 않고, 서비스 클래스가 이를 처리합니다.
    - 이로 인해 코드가 더 구조적으로 관리되고 테스트 가능성이 높아집니다.
```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    public User findUserById(Long id) {
        return userRepository.findById(id).orElseThrow(() -> new UserNotFoundException(id));
    }
}
```

- **3. Repository 패턴**
    - 데이터베이스 관련 로직을 별도의 `Repository` 인터페이스로 분리하여 데이터 엑세스를 쉽게 관리하고 추상화할 수 있습니다.

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // 데이터베이스 접근 로직을 추상화
}
```

### 4. MVP 패턴이 백엔드에서 적합하지 않은 이유.
- **View에 대한 의존성.**
    - MVP 패턴의 핵심 요소는 View 이며, 백엔드에는 UI를 다루지 않기 때문에 View의 역할이 존재하지 않습니다.
    - 백엔드는 사용자 인터페이스를 렌더링하거나 다루지 않고, 데이터를 처리하고 클라이언트는 응답을 반환하는 역할을 합니다.
- **분리된 로직.**
    - 백엔드는 클라이언트와 데이터를 주고받으며, 이 과정에서 비즈니스 로직, 데이터 엑세스, API 응답 생성 등과 같은 복잡한 처리가 이루어 집니다.
    - 이러한 작업을 관리하는 데는 **MVC 패턴** 이나 **레이어드 아키텍처** 가 더 적합합니다.
