---
title: 💻[Code Review] WebConfig 클래스 코드 리뷰.
tags:
    - Code review
date: "2024-12-14"
thumbnail: "/assets/img/thumbnail/codereview.jpg"
---

# 💻[Code Review] WebConfig 클래스 코드 리뷰.
## 1️⃣ 전체 코드.
```java
import org.springframework.stereotype.Controller;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Controller
public class WebConfig implements WebMvcConfigurer {

	@Override
	public void addCorsMappings(CorsRegistry registry) {
		registry.addMapping("/**") // 모든 경로 허용
			.allowedOriginPatterns("http://localhost:*")
			.allowedMethods("GET", "POST", "PUT", "DELETE")
			.allowedHeaders("*")
			.allowCredentials(true) // 자격 증명 허용
			.maxAge(3600);
	}
}
```

- 위 코드는 **Spring Framework**에서 CORS(Cross-Origin Resource Sharing) 설정을 정의한 코드입니다.
- WebMvcConfigurer 인터페이스를 구현하여 CORS 정책을 구성하고, 클라이언트(예: 프론트엔드)에서 백엔트 API에 요청할 때 발생할 수 있는 CORS 문제를 해결합니다.

## 2️⃣ CORS란? 🤔
- CORS(Cross-Origin Resource Sharing)는 브라우저 보안 정책 중 하나로, 웹 애플리케이션이 자신과 다른 출처(origin)에서 리소스를 요청할 때 이를 허용하거나 제한하는 방식입니다.
    - 예를 들어:
        - 클라이언트가 `http://localhost:3000`에서 동작하고, 서버가 `http://localhost:8080`에서 동작하면 두 도메인은 서로 다른 출처로 간주됩니다.
        - 브라우저는 기본적으로 이런 교차 출처 요청을 차단하므로, 서버에서 명시적으로 이를 허용해야 합니다.

## 3️⃣ 코드 분석.
### 1️⃣ 클래스 선언.
```java
@Controller
public class WebConfig implements WebMvcConfigurer {...}
```
- WebMvcConfigurer는 Spring MVC 설정을 커스터마이징하기 위한 인터페이스입니다.
- WebConfig 클래스는 WebMvcConfigurer를 구현하여 Spring MVC 설정을 사용자 정의하고 있습니다.

### 2️⃣ addCorsMappings 메서드.
```java
@Override
public void addCorsMappings(CorsRegistry registry) {...}
```
- Spring MVC에서 CORS 매핑을 설정하기 위해 CorsRegistry를 사용합니다.
- 이 메서드는 클라이언트에서 특정 경로로의 요청을 허용하거나 제한하는 규칙을 정의합니다.

### 3️⃣ CORS 설정.
```java
registry.addMapping("/**") // 모든 경로 허용
        .allowedOriginPatterns("http://localhost:*") // localhost의 모든 포트 허용
        .allowedMethods("GET", "POST", "PUT", "DELETE") // 허용할 HTTP 메서드
        .allowedHeaders("*") // 모든 헤더 허용
        .alloweCredentials(true) // 쿠키와 같은 자격 증명 허용
        .maxAge(3600) // 캐시 시간 (초 단위)
```
- **`addMapping("/**")`**
    - 모든 URL 패턴`(/**)`에 대해 CORS 정책을 적용합니다.
    - 예: `/api/*, /resources/*` 등 모든 경로 허용.
- **`allowedOriginPatterns("http://localhost:*)"`**
    - 클라이언트가 `http://localhost`에서 시작되는 모든 포트(예: `http://localhost:3000, http://localhost:8080`)에 대해 요청을 허용합니다.
    - Spring Boot 2.4+에서는 allowedOrigin 대신 allowedOriginPatterns 사용을 권장합니다.
- **`allowedMethods("GET", "POST", "PUT", "DELETE")`**
    - 허용할 HTTP 메서드(GET, POST, PUT, DELETE)를 정의합니다.
    - 예를 들어, OPTIONS와 같은 다른 메서드는 허용되지 않습니다.
- **`allowedHeaders("*")`**
    - 모든 요청의 헤더를 허용합니다.
    - 예: Content-Type, Authorization 등이 허용됩니다.
- **`allowCredentials(true)`**
    - 클라이언트에서 쿠키를 사용한 요청을 허용합니다.
    - 예: 인증이 필요한 요청에서 쿠키를 통해 세션 정보를 전달할 수 있습니다.
- **`maxAge(3600)`**
    - 브라우저가 프리플라이트 요청(OPTIONS 요청)의 결과를 캐싱하는 시간을 정의합니다.
    - 여기서는 3600초(1시간) 동안 캐싱합니다.

## 4️⃣ 사용 사례.
- **1. 프론트엔드-백엔드 분리된 환경.**
    - 프론트엔드와 백엔드가 서로 다른 출처에서 동작하는 경우:
        - 프론트엔드: `http://localhost:3000`
        - 백엔드: `http:// localhost:8080`
            - 이 경우, 브라우저는 기본적으로 교차 출처 요청을 차단하므로 위와 같이 CORS 설정이 필요합니다.

## 5️⃣ 주의사항.
- **보안 고려**
    - 실제 배포 환경에서는 **`allowedOriginPatterns`에 와일드카드`("*")`를 사용하는 것을 피하고, 특정 출처만 허용해야 합니다.**
    - 예: `.allowedOriginPatterns("https://example.com)"`
- **`allowCredentials`와 `*`의 충돌**
    - `allowCredentials(true)`와 `allowedOriginPatterns("*")`는 함께 사용할 수 없습니다.
    - 쿠키를 사용한 요청을 허용하려면 특정 출처를 명시해야 합니다.
