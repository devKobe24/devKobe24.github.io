---
title: "🍃[Spring] API"
tags:
    - Spring
    - Framework
date: "2024-02-18"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# API

## `@ResponseBody 문자 반환`
```java
@Controller
public class HelloController {
    
    @GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name) {
        return "hello " + name;
    }
}
```
* `@ResponseBody`를 사용하면 `뷰 리졸버('viewResolver')`를 사용하지 않습니다
    * 대신 HTTP의 BODY에 문자 내용을 직접 반환합니다(HTML BODY TAG를 말하는 것이 아닙니다.)

**실행**
http://localhost:8080/hello-string?name=spring

## `@ResponseBody 객체 반환`
```java
@Controller
public class HelloController {
    
    @GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name) {
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }
    
    static class Hello {
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
}
```
* `@ResponseBody`를 사용하요, 객체를 반환하면 객체가 JSON으로 변환됩니다.

**실행**
* http://localhost:8080/hello-api?name=spring

## `@ResponseBody 사용 원리`

<img src="https://github.com/devKobe24/images/blob/main/@ResponseBody%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%8B%E1%85%AF%E1%86%AB%E1%84%85%E1%85%B5.jpeg?raw=true">

1. 웹 브라우저에서 `localhost:8080/hello-api` 로 접속을 시도합니다.
2. 톰켓 내장 서버에서 "**`hello-api`** 가 왔어" 하고 스프링에 던집니다.
3. 스프링은 "**`hello-api`** 가 있네?! 어? 근데 `@RespinseBody` 어노테이션이 붙어있다!!" 하고 인식합니다.
    * 이런 어노테이션이 붙어있지 않았다면 viewResolver에게 던집니다. -> "나에게 맞는 템플릿을 찾아 돌려줘"
    * `@ResponsBody`가 있다면 HTTP 응답에 그대로 넘겨야겠다 하고 동작합니다.
        * 여기서 문자가 오면 바로 문자를 돌려줍니다.
        * 하지만 객체가 오면 기본 디폴트인 `JSON` 방식으로 데이터를 만들어서 `HTTP 응답`에 반환합니다. -> 기본 정책
4. `@ResponseBody return: hello(name: spring)`은 객체를 넘겨줍니다. 그러면 **"`HttpMessageConverter`"** 가 동작합니다.
5. 만약 반환하는 아이가 단순 문자일 경우 **"`StringConverter`"** 가 동작합니다.
6. 그게 아니고 반환하는 아이가 객체일 경우 **"`JsonConverter`"** 가 동작합니다.
7. 결국 JSON 형식으로 변환한 것을 요청한 웹 브라우저에게 보내줍니다.

**정리**
* `@ResponseBody`를 사용
    * HTTP의 BODY에 문자 내용을 직접 반환
    * `viewResolver` 대신에 `httpMessageConverter`가 동작
    * 기본 문자처리: `StringHttpMessageConverter`
    * 기본 객체처리: `MappingJackson2HttpMessageConverter`
    * byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음

> 참고: 클라이언트 HTTP Accept 헤더와 서버의 컨트롤러 반환 타입 정보 들을 조합해서 `HttpMessageConverter`가 선택됩니다.
