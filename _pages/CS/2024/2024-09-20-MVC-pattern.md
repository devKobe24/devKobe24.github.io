---
title: "💾 [CS] MVC 패턴."
tags:
    - CS
date: "2024-09-20"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] MVC 패턴.

## 1️⃣ MVC 패턴.
- MVC 패턴은 모델(Model), 뷰(View), 컨트롤러(Controller)로 이루어진 디자인 패턴입니다.

<img src = "https://github.com/devKobe24/images2/blob/main/CS_IMG/cs-mvc.png?raw=true">

- 애플리케이션의 구성 요소를 세 가지 역할로 구분하여 개발 프로세스에서 각각의 구성 요소에만 집중해서 개발할 수 있습니다.
- 재사용성과 확장성이 용이하다는 장점이 있고, 애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해지는 단점이 있습니다.

### Model(모델)
- 모델(model)은 애플리케이션의 데이터인 데이터베이스, 상수, 변수 등을 뜻합니다.
- 예를 들어 사각형 모양의 박스 안에 글자가 들어 있다면 그 사각형 모양의 박스 위치 정보, 글자 내용, 글자 위치, 글자 포맷(utf-8 등)에 관한 정보를 모두 가지고 있어야 합니다.
- 뷰에서 데이터를 생성하거나 수정하면 컨트롤러를 통해 모델을 생성하거나 갱신합니다.

### View(뷰)
- 뷰(View)는 Inputbox, checkbox, textarea 등 사용자 인터페이스 요소를 나타냅니다.
- 즉, 모델을 기반으로 사용자가 볼 수 있는 화면을 뜻합니다.
- 모델이 가지고 있는 정보를 따로 저장하지 않아야 하며 단순히 사각형 모양 등 화면에 표시하는 정보만 가지고 있어야 합니다.
- 또한, 변경이 일어나면 컨트롤러에 이를 전달해야 합니다.

### Controller(컨트롤러)
- 컨트롤러(Controller)는 하나 이상의 모델과 하나 이상의 뷰를 잇는 다리 역할을 하며 이벤트 등 메인 로직을 담당합니다.
- 또한, 모델과 뷰의 생명주기도 관리하며, 모델이나 뷰의 변경 통지를 받으면 이를 해석하여 각각의 구성 요소에 해당 내용에 대해 알려줍니다.

## 2️⃣ MVC 패턴의 예 리액트.
- MVC 패턴을 이용한 대표적인 프레임워크로는 자바 플랫폼을 위한 오픈 소스 애플리케이션 프레임워크인 스프링(Spring)이 있습니다.
- Spring의 WEB MVC는 웹 서비스를 구축하는 데 편리한 기능들을 많이 제공합니다.
- 예를 들어 `@RequestParam`, `@RequestHaader`, `@PathVariable` 등의 애너테이션을 기반으로 사용자의 요청 값들을 쉽게 분석할 수 있으며 사용자의 어떠한 요청이 유효한 요청인지 쉽게 거를 수 있습니다.
- 예를 들어 숫자를 입력해야 하는데 문자를 입력하는 사례 같은 것 말이죠.
- 또한 재사용 가능한 코드, 테스트, 쉽게 리디렉션할 수 있게 하는 등의 장점이 있습니다.

## 3️⃣ 자바에서의 MVC 패턴.
- 자바에서의 **MVC 패턴(Model-View-Contorller)** 은 웹 애플리케이션 개발에서 널리 사용되는 소프트웨어 디자인 패턴입니다.
- 이 패턴은 애플리케이션을 모델(Model), 뷰(View), 컨트롤러(Controller)로 분리하여 코드의 유지보수성과 확장성을 높이는 구조를 제공합니다.

### MVC 패턴의 구성 요소.

#### 1. Model(모델)
- **역할 :** 애플리케이션의 **데이터와 비즈니스 로직** 을 처리합니다.
- **기능 :**
    - 데이터베이스와의 상호작용.
    - 데이터 저장, 수정, 삭제와 같은 비즈니스 로직 처리.
    - 데이터를 가공하여 제공.
- **예 :** 데이터베이스 엔티티, DAO, 서비스 클래스.
- **예시 코드 :**
```java
public class User {
    private String username;
    private String email;
    
    // Getter and Setter
}
```

#### 2. View(뷰)
- **역할 :** 사용자가 보는 **UI(사용자 인터페이스)** 를 담당합니다. 모델로부터 데이터를 받아와서 사용자에게 보여줍니다.
- **기능 :**
    - HTML, JSP, Thymeleaf 같은 템플릿 엔진을 사용하여 사용자에게 데이터를 렌더링
    - 데이터 입력, 출력 및 이벤트 처리.
- **예 :** JSP, Thymeleaf, HTML 파일.
- **예시코드(Thymeleaf)**
```java
<html>
<body>
    <h1>User List</h1>
    <ul>
        <li th:each="user : ${users}">
            <span th:text="${user.username}"></span>
        </li>
    </ul>
</body>
</html>
```

#### 3. Controller(컨트롤러)
- **역할 :** 사용자의 요청을 처리하고, 필요한 데이터를 모델에서 가져와서 뷰에 전달하는 역할을 합니다.
- **기능 :**
    - 사용자 입력을 받고, 이를 처리할 적절한 로직(모델)으로 전달
    - 모델로부터 데이터를 받아서 적절한 뷰로 반환.
    - HTTP 요청을 처리하고, 결과를 뷰에 반영.
- **예 :** Spring MVC의 `@Controller` 클래스.
- **예시코드 (Spring Boot)**
```java
@Controller
public class UserController {
    @Autowired
    private UserService userService;
    
    @GetMapping("/users")
    public String listUsers(Model model) {
        List<User> users = userService.getAllUsers();
        model.addAttribute("users", users);
        return "userList"; // userList.html로 반환
    }
}
```

### MVC 패턴의 흐름.
- **1. 사용자의 요청**
    - 사용자가 브라우저에서 URL을 입력하거나 버튼을 클릭하는 등의 동작을 통해 컨트롤러로 요청이 전달됩니다.
- **2. 컨트롤러의 처리**
    - 컨트롤러는 사용자의 요청을 받고, 비즈니스 로직이 필요한 경우 모델을 호출하여 데이터를 처리하거나 가져옵니다.
- **3. 모델의 처리**
    - 모델은 데이터베이스와 상호작용하여 데이터를 읽고, 수정하거나 추가/삭제한 후, 컨트롤러로 결과를 반환합니다.
- **4. 뷰에 데이터 전달**
    - 컨트롤러는 모델에서 받은 데이터를 뷰로 전달하고, 해당 뷰가 사용자에게 보여지도록 응답을 생성합니다.
- **5. 결과 반환**
    - 최종적으로 사용자는 브라우저에서 컨트롤러가 처리한 결과를 볼 수 있습니다.

### 자바에서 MVC 패턴을 사용하는 예.
- 자바에서는 Spring MVC 프레임워크를 사용하요 MVC 패턴을 구현하는 것이 일반적입니다.
- Spring MVC는 컨트롤러, 모델, 뷰의 역할을 분리하여 웹 애플리케이션을 개발할 수 있게 해줍니다.

#### Spring MVC의 기본 흐름.
- **1. DispatcherServler**
    - 모든 요청은 먼저 `DispatcherServlet`으로 전달됩니다.
    - 이것은 Front Controller로서, 요청을 적절한 컨트롤러로 라우팅합니다.
- **2. Controller**
    - `DispatcherServler`은 요청을 처리할 적절한 컨트롤러 메서드를 호출합니다.
- **3. Model**
    - 컨트롤러는 필요한 경우 모델과 상호작용하여 데이터를 가져오거나 처리합니다.
- **4. View**
    - 컨트롤러는 모델에서 처리된 데이터를 뷰에 전달합니다.
- **5. View Resolver**
    - 뷰 리졸버(View Resolver)가 HTML, JSP, Thymeleaf 템플릿 등과 같은 뷰를 렌더링하여 클라이언트에게 응답을 보냅니다.

#### Spring MVC 코드 예시.
- **1. Controller**
```java
@Controller
public class ProductController {
    
    @Autowired
    private ProductService productService;
    
    @GetMapping("/products")
    public String getAllProducts(Model model) {
        List<Product> products = productService.getAllProducts();
        model.addAttribute("products", products);
        return "productList"; // productList.html로 반환
    }
}
```

- **2. Model(Service & Entity)**
```java
@Service
public class ProductService {
    
    @Autowired
    private ProductRepository productRepository;
    
    public List<Product> getAllProducts() {
        return productRepository.findAll();
    }
}

@Entity
public class Product {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private double price;
    
    // Getter and Setters
}
```

- **3. View(Thymeleaf 템플릿)**
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Product List</title>
</head>
<body>
    <h1>Product List</h1>
    <ul>
        <li th:each="product : ${products}">
            <span th:text="${product.name}">Product Name</span> - 
            <span th:text="${product.price}">Price</span>
        </li>
    </ul>
</body>
</html>
```

### MVC 패턴의 장점.
- **1. 유지보수성 향상.**
    - 비즈니스 로직(Model)과 사용자 인터페이스(View)를 분리함으로써 코드를 쉽게 유지보수할 수 있습니다.
    - UI를 수정해도 비즈니스 로직에는 영향을 미치지 않습니다.
- **2. 확장성.**
    - 각 부분(Model, View, Controller)을 독립적으로 확장할 수 있어 확장성이 뛰어납니다.
- **3. 테스트 용이성.**
    - 비즈니스 로직과 UI가 분리되어 있어, 각각의 부분을 독립적으로 테스트할 수 있습니다.

## 4️⃣ 결론.
- MVC 패턴은 자바 기반 웹 애플리케이션에서 중요한 디자인 패턴으로, 애플리케이션을 모델, 뷰, 컨트롤러로 분리하여 구조를 명확하게 유지하고 코드의 재사용성을 높이는 데 기여합니다.
- Spring MVC는 이를 자바에서 쉽게 구현할 수 있는 대표적인 프레임워크로, 많은 자바 개발자들이 사용하는 방식입니다.
