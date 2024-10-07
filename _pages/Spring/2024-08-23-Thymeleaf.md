---
title: 🍃[Spring] Welcome Page 구현 및 동작 방법.
tags:
    - Spring
    - Framework
date: "2024-08-23"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Welcome Page 구현 및 동작 방법.

## 1️⃣ Welcom Page 만들기.
- Welcom Page는 **`'resource/static'`** 내부에 **`'index.html'`** 이라는 파일로 만들면 됩니다.
```html
<!DOCTYPE html>
<html>
	<head>
		<title>Hello</title>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
	</head>
	<body>
		Hello
		<a href="/hello">hello</a>
	</body>
</html>
```

- 스프링 부트가 제공하는 Welcom Page 기능.
    - **`'static/index.html'`** 을 올려두면 Welcom page 기능을 제공합니다.
    - [Spring.io 공식 도큐먼트](https://docs.spring.io/spring-boot/3.3-SNAPSHOT/reference/web/reactive.html#web.reactive.webflux.welcome-page)

## 2️⃣ thymeleaf 템플릿 엔진.
- [thymeleaf 공식 사이트](https://www.thymeleaf.org/)
- [스프링 공식 튜토리얼](https://spring.io/guides/gs/serving-web-content)
- [스프링부트 메뉴얼](https://docs.spring.io/spring-boot/3.3-SNAPSHOT/reference/web/servlet.html#web.servlet.spring-mvc.template-engines)

```java
// com/devkobe/hello_spring/controller
@Controller
public class HelloController {
    
    @GetMapping("hello")
    public String hello(Model model) {
        model.addAttribute("data", "hello!!");
        return "hello";
    }
}
```

```html
<!-- `resource/static/index.html` -->

<!DOCTYPE html>
<html>
	<head>
		<title>Hello</title>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
	</head>
	<body>
		Hello
		<a href="/hello">hello</a>
	</body>
</html>
```

```html
<!-- `resource/templates/hello.html` -->
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
	<title>Hello</title>
</head>
<meta charset="UTF-8">
<body>
<p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
</body>
</html>
```

- **thymeleaf 템플릿엔진 동작 확인**
    - 실행 : `http://localhost:8080/hello`
- **동작 환경 그림.**
<img src = "https://github.com/devKobe24/images2/blob/main/springboot/spring-thymeleaf.png?raw=true">

- 컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버(**`'viewResolver'`**) 가 화면을 찾아서 처리합니다.
    - 스프링 부트 템플릿엔진 기본 **`'viewName'`** 매핑.
        - **`'resources:template/'`** + **`'{ViewName}'`** + **`'.html'`**

> 🙋‍♂️ 참고: **`'spring-boot-devtools'`** 라이브러리를 추가하면 **`'html'`** 파일을 컴파일만 해주면 서버 재시작 없이 View 파일 변경이 가능합니다.
> IntelliJ 컴파일 방법 : 메뉴 build ➔ Recompile
