---
title: 🍃[Spring] MVC와 템플릿 엔진.
tags:
    - Spring
    - Framework
date: "2024-08-24"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] MVC와 템플릿 엔진.
- MVC
    - Model
    - View
    - Controller

- **Controller**

```java
@Controller
public class HelloController {
    
    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam("name") String name, Model model) {
        model.addAttribute("name", name);
        return "hello-template";
    }
}
```

- **View**

```html
<html xmlns:th="htt[://www.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```

- **실행**
    - http://localhost:8080/hello-mvc?name=spring

<img src = "https://github.com/devKobe24/images2/blob/main/Inflearn-Java-Mid/mvc-execute.png?raw=true">

- **MVC 템플릿 엔진 이미지**

<img src = "https://github.com/devKobe24/images2/blob/main/Inflearn-Java-Mid/mvc-template-engine-img.png?raw=true">
