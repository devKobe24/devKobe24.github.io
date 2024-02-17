---
title: "🍃[Spring] MVC와 템플릿 엔진"
tags:
    - Spring
    - Framework
date: "2024-02-17"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# MVC와 템플릿 엔진

* MVC: Model, View, Controller

**Controller**

```java
package com.devkobe.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class HelloController {
    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam("name") String name, Model model) {
        model.addAttribute("name", name);
        return "hello-template";
    }
}
```

**View**
```html
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```

**실행**
* http://localhost:8080/hello-mvc?name=spring

**MVC, 템플릿 엔진 이미지.**
<img src="https://github.com/devKobe24/images/blob/main/MVC%E1%84%90%E1%85%A6%E1%86%B7%E1%84%91%E1%85%B3%E1%86%AF%E1%84%85%E1%85%B5%E1%86%BA%E1%84%8B%E1%85%A6%E1%86%AB%E1%84%8C%E1%85%B5%E1%86%AB%E1%84%8B%E1%85%B5%E1%84%86%E1%85%B5%E1%84%8C%E1%85%B5.jpeg?raw=true">

1. 웹 브라우저에서 `localhost:8080/hello-mvc`를 넘깁니다.
2. 스프링 부트를 띄울 때 함깨 띄우는 **"내장 톰켓 서버를 `localhost:8080/hello-mvc` 가 거칩니다"**.
3. **내장 톰켓 서버**가 **'`localhost:8080/hello-mvc`가 왔어~' 하고는 스프링에게 던집니다.**
4. 그러면 **스프링**은 '아! `localhost:8080/hello-mvc`가 `helloController` 내부 메서드에 매핑이 되어있네?!'하고는 **"그 메서드를 호출해줍니다."**
5. 호출한 메서드가 리턴시 이름을 "hello-temple"이라고 하고, "model에 키는 name이고, 값은 spring으로 넣은것"을 스프링에게 넘겨줍니다.
6. 그러면 스프링이 **"viewResolver"** (화면과 관련된 해결자 - 뷰를 찾아주고 템플릿 엔진을 연결 시켜주는 것입니다.)가 동작합니다.
7. viewResolver가 **"templates"** 의 **"hello-template"** 이라는 **"리턴의 String name과 똑같은 아이를 찾아서 Thymeleaf 템플릿 엔진에게 처리해달라고 넘깁니다."**
8. 그러면 "**템플릿 엔진이 랜더링**"을 하여 **"변환을 한 HTML"** 을 **"웹 브라우저에 반환합니다."**

* **정적일 때는 "변환을 하지 않았습니다." 즉, 그대로 반환을 해줬습니다.**
    * 이런 **"템플릿 엔진에서는 변환을해서 웹 브라우저에 넘겨줍니다."**
