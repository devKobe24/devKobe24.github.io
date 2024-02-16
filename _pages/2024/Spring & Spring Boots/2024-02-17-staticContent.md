---
title: "🍃[Spring] 정적 컨텐츠"
tags:
    - Spring
    - Framework
date: "2024-02-17"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 정적 컨텐츠.

웹을 개발한다는 것은 크게 3가지 방법으로 나뉩니다.

1. 정적 컨텐츠: 서버에서의 별다른 작업 없이 파일을 웹 브라우저에 그대로 내려주는 것 입니다.
2. MVC와 템플릿 엔진: JSP, PHP 등 활용하여 HTML을 그냥 주는 것이 아니라 서버에서 프로그래밍을 해서 HTML을 동적으로 바꿔서 내려주는 것을 템플릿 엔진이라고 말하며, 그것을 하기 위하여 Model-View(템플릿 엔진 화면)-Controller 이 세가지를 MVC라고 합니다.
3. API: JSON이라는 데이터구조 포맷으로 클라이언트에게 데이터를 전달하는 것이 보통 API 방식이라고 합니다.

**"스프링 부트는 자동으로 정적 컨텐츠 기능을 제공합니다."**

```java
<!DOCTYPE html>
<html lang="en">
<head>
  <meta http-equiv="Content-Type" conten="text/html"; charset="UTF-8" />
  <title>Static Content</title>
</head>
<body>
정적 컨텐츠 입니다.
</body>
</html>
```

<img src="https://github.com/devKobe24/images/blob/main/:static%E1%84%91%E1%85%A9%E1%86%AF%E1%84%83%E1%85%A5.png?raw=true">

* Spring Boot Docs에 다음과 같이 명시되어 있습니다. **"기본적으로 Spring Boot는 `\static` 이라는 디렉터리에서 정적 콘텐츠를 제공합니다."**

> [Spring Boot Docs - Static Content](https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content) 참고해주세요.

<img src="https://github.com/devKobe24/images/blob/main/%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%E1%84%8F%E1%85%A5%E1%86%AB%E1%84%90%E1%85%A6%E1%86%AB%E1%84%8E%E1%85%B3%E1%84%8B%E1%85%B5%E1%84%86%E1%85%B5%E1%84%8C%E1%85%B5.jpeg?raw=true">

**"위 그림은 큰 개념을 잡기 위하여 이해하기 쉽게 그린 그림입니다."**
Spring MVC를 본격적으로 학습하면서 깊이있게 들어가게 되면 이 내부에 여러가지가 많이 나오게 됩니다.
지금 이 포스팅의 목적 자체는 **"디테일"** 보다는 **"크게 어떻게 돌아가는지를 이해하는데 목적이 있습니다."**
즉, '큰 그림을 보는 것'이라고 이해하면 됩니다.

* 먼저 "localhost:8080/hello-static.html"을 접속했습니다.
    * 제일 처음에 내장 톰켓 서버가 요청을 받습니다.
1. 그러면 내장 톰켓 서버가 "hello-static.html이 왔데요~"하고 "Spring"에게 넘깁니다.
    * 이때, Spring은 Controller에서 hello-static이 있는지 찾아봅니다.
        * **"즉, Controller가 먼저 우선순위를 가진다는 뜻 입니다."**

<img src="https://github.com/devKobe24/images/blob/main/hello-spring%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%E1%84%82%E1%85%A2%E1%84%87%E1%85%AE%E1%84%80%E1%85%AE%E1%84%8C%E1%85%A9.png?raw=true">

* 컨트롤러 내부를 보니 "hello-static" 관련 컨트롤러가 없음을 확인할 수 있습니다.
  * 따라서 이후에 스프링 부트는 "resources: static/hello-static.html"을 찾습니다.
    * 그래서 "오 있다!!" 하고 찾으면 바로 "hello-static.html"을 반환해주는 것 입니다.
