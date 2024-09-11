---
title: 🍃[Spring] Spring MVC에서 `View` 객체.
tags:
    - Spring
    - Framework
date: "2024-09-12"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] Spring MVC에서 `View` 객체.

## 1️⃣ `View` 객체.
- Spring MVC에서 `View` 객체는 클라이언트에게 응답을 생성하는 데 사용되는 최종 출력을 담당하는 구성 요소입니다.
- `View` 객체는 주어진 모델 데이터를 사용하여 HTML, JSON, XML, PDF 등 다양한 형태로 응답을 렌더링 합니다.
- Spring MVC의 `View` 객체는 추상화된 인터페이스를 통해 다양한 템플릿 엔진이나 출력 형식을 지원합니다.

## 2️⃣ `View` 객체의 주요 역할.
- **1. 모델 데이터 렌더링**
    - `View` 객체는 컨트롤러에서 전달된 `Model` 데이터를 이용해 클라이언트가 볼 수 있는 형식으로 변환합니다.
        - 예를 들어, HTML 템플릿 엔진을 사용하여 웹 페이지를 생성하거나, JSON으로 데이터를 변환해 API 응답으로 보낼 수 있습니다.
- **2. 응답 생성**
    - 클라이언트에게 전송될 최종 응답을 생성합니다.
        - 이는 브라우저에 표시될 HTML 페이지일 수도 있고, RESTful API의 JSON 응답일 수도 있습니다.
- **3. 컨텐츠 타입 설정**
    - `View` 객체는 생성된 응답 컨텐츠 타입(예: `text/html`, `application/json`, `application/pdf` 등)을 설정하여 클라이언트가 올바르게 해석할 수 있도록 합니다.

## 3️⃣ `View` 객체의 종류.
- Spring MVC에서는 여러 가지 유형의 `View` 객체를 지원합니다.
    - 이들은 다양한 응답 형식을 처리할 수 있도록 설계되었습니다.
### 1. JSP(JavaServer Page)
- Spring MVC에서 가장 전통적으로 사용되는 뷰 기술입니다.
- JSP는 HTML과 Java 코드를 혼합하여 동적인 웹 페이지를 생성합니다.

### 2. Thymeleaf
- 최근 많이 사용되는 템플릿 엔진으로, HTML 파일을 템플릿으로 사용하여, HTML 태그에 특수한 속성을 추가하여 동적인 콘텐츠를 생성할 수 있습니다.

### 3. FreeMarker
- HTML뿐만 아니라 텍스트 기반의 다양한 문서를 생성할 수 있는 템플릿 엔진입니다.

### 4. Velocity
- Apache 프로젝트에서 제공하는 템플릿 엔진으로, FreeMarker와 유사하게 다양한 텍스트 기반 응담을 생성할 수 있습니다.

### 5. JSON/XML View
- `MappingJackson2JsonView` 와 같은 JSON 또는 XML 뷰를 사용하여 데이터를 JSON 또는 XML 형식으로 변환하여 RESTful API 응답을 생성할 수 있습니다.

### 6. PDF/Excel View
- `AbstractPdfView`, `AbstractXlsView` 와 같은 뷰를 사용하여 PDF나 Excel 파일을 생성하여 응답으로 반환할 수 있습니다.

## 4️⃣ `View` 객체 사용 예시
- 컨트롤러에서 `View` 객체를 명시적으로 사용할 수도 있고, 뷰 리졸버(View Resolver)가 자동으로 처리할 수도 있습니다.

```java
@Controller
public class GreetingController {
    
    @GetMapping("/greeting")
    public ModelAndView greeting() {
        ModelAndView mav = new ModelAndView("greeting");
        mav.addObject("message", "Hello welcome to our website!");
        return mav;
    }
}
```

- 위의 예제에서 `ModelAndView` 객체를 사용하여 `View`를 명시적으로 지정했습니다.
- `greeting`이라는 뷰 이름은 보통 JSP나 Thymeleaf 템플릿 파일과 매핑되며, 이 템플릿 파일이 렌더링 됩니다.

## 5️⃣ 뷰 리졸버(View Resolver)
- Spring MVC에서 뷰 리졸버(View Resolver)는 컨트롤러가 반환한 뷰 이름을 실제 `View` 객체로 변환해주는 역할을 합니다.
- 일반적으로 뷰 리졸버는 템플릿 엔진이나 뷰 파일의 경로를 관리하고, 클라이언트에게 응답할 적절한 `View` 객체를 결정합니다.
    - 예를 들어, 다음과 같은 뷰 리졸버 설정이 있을 수 있습니다.
```java
@Bean
public InternalResourceViewResolver viewResolver() {
    InternalResourceViewResolver resolver = new InternalResourceViewResolver();
    resolver.setPrefix("/WEB-INF/views/");
    resolver.setSuffix(".jsp");
    return resolver;
}
```

- 위 설정에서는 컨트롤러가 `greeting`이라는 뷰 이름을 반환하면, `InternalResourceViewResolver`는 `/WEB-INF/views/greeting.jsp` 라는 JSP 파일을 찾아서 렌더링합니다.

## 6️⃣ 요약.
- `View` 객체는 Spring MVC에서 클라이언트에게 응답을 생성하고 렌더링하는 역할을 합니다.
- 이 객체는 모델 데이터를 사용하여 최종 응답을 생성하며, 다양한 형식의 출력을 지원합니다.
- Spring MVC는 `View` 객체를 직접 사용하거나 뷰 리졸버를 통해 간접적으로 사용하여, 클라이언트에게 최종 결과를 전달합니다.
