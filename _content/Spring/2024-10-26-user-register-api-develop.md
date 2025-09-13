---
title: 🍃[Spring] 회원 가입 - 유저 생성 API 개발하기.
tags:
    - Spring
    - Framework
date: "2024-10-26"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] 회원 가입 - 유저 생성 API 개발하기.

## 1️⃣ API 설계하기.

### 1️⃣ 사용자를 등록하기 위한 API 명세.

<img src = "https://github.com/devKobe24/images2/blob/main/springboot/Spring_CreateAccount.png?raw=true">

- 회원 가입 웹 UI가 먼저 만들어져 있습니다.
    - 그러므로 프론트엔드에서 사용한 API, 즉 기능들을 채워 넣어야 합니다.
        - 회원 가입을 위한 API 명세는 다음과 같습니다.
            - **HTTP Method : POST** 
            - **HTTP Path : /register**
            - **Http Body (JSON)**

            ```java
            {
                "username": String (null 불가능),
                "email": String (null 불가능),
                "password": String (null 불가능),
                "confirmPassword": String (null 불가능)
            }
            ```
            
            - **결과 반환 X (HTTP 상태 200OK 이면 충분함)**

> 🙋‍♂️ [API(Applicatioon Programming Interface)란 무엇일까?](https://www.devkobe24.com/CS/2024/2024-10-09-what-is-the-api.html)

## 2️⃣ UserController 생성하기.
- 우선 `UserController`를 생성한 후 내부에 다음과 같이 코드를 생성해줍니다.

```java
package com.kobe.signUpApp.controller.user;

import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {
    @PostMapping("/register")
    public void registerUser() {

    }
}
```

## 3️⃣ dto 패키지 -> user 패키지 -> request 패키지 -> UserCreateRequest 클래스 생성하기

- 아래와 같이 dto 객체인 userCreateRequest 클래스를 생성해줍니다.

```java
package com.kobe.signUpApp.dto.user.request;

public class UserCreateRequest {
    private String username;
    private String email;
    private String password;
    private String confirmPassword;

    public String getUsername() {
        return username;
    }

    public String getEmail() {
        return email;
    }

    public String getPassword() {
        return password;
    }

    public String getConfirmPassword() {
        return confirmPassword;
    }
}
```

## 4️⃣ UserController 내부 `registerUser` 메서드 리팩토링.
- POST API에서 들어온 HTTP Body를 위에서 만든 `UserCreateRequest` 클래스로 변환하기 위해서 `@RequestBody` 어노테이션을 사용해줍니다.

```java
package com.kobe.signUpApp.controller.user;

import com.kobe.signUpApp.dto.user.request.UserCreateRequest;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {
    @PostMapping("/register")
    public void registerUser(@RequestBody UserCreateRequest request) {

    }
}
```

- 여기까지 API의 구조를 잡았습니다.

## 5️⃣ 실제 유저가 저장되도록 구현하기.
- 유저를 저장하기 위하여 `User`라는 객체를 만들어서 저장할 것 입니다.
    - 다음과 같은 과정을 거쳐서 만들어 볼 것 입니다.
        - 먼저, domain 패키지 내부에 user라는 패키지를 만들어 줍니다.
        - user 패키기 내부에 User 클래스를 만들어 줍니다.
        ```java
        @PostMapping("/register")
        public void registerUser(@RequestBody UserCreateRequest request) {
        }  
        ```
        - 위 API가 사용되서 유저가 저장되어야 한다면 User 객체를 만들어서 실제 리스트에 저장을 할 것 입니다.
- `User`의 경우에는 다음과 같은 필드가 포함됩니다.
    - username
    - email
    - password
    - confirmPassword

```java
package com.kobe.signUpApp.domain.user;

public class User {
    private String username;
    private String email;
    private String password;
    private String confirmPassword;

    public User(
            String username,
            String email,
            String password,
            String confirmPassword
    ) {
        if (username == null || username.isBlank()) {
            throw new IllegalArgumentException(String.format("잘못된 username(%s)이 들어왔습니다.", username));
        } else if (email == null || email.isBlank()) {
            throw new IllegalArgumentException(String.format("잘못된 email(%s)이 들어왔습니다.", email));
        } else if (password == null || password.isBlank()) {
            throw new IllegalArgumentException(String.format("잘못된 password(%s)이 들어왔습니다.", password));
        } else if (confirmPassword == null || confirmPassword.isBlank()) {
            throw new IllegalArgumentException(String.format("잘못된 confirmPassword(%s)이 들어왔습니다.", confirmPassword));
        }


        this.username = username;
        this.email = email;
        this.password = password;
        this.confirmPassword = confirmPassword;
    }
}
```

- `User` 객체를 만들 때, 생성자(Constructor)를 통해 필드에 값을 넣어줍니다.
    - 이 때, `null`이 들어 와서는 안되는 필드들을 위해 조건문을 사용하여 `null` 값이 들어왔을 때 `IllegalAragumentException` 예외를 던집니다(`throw`).
        - 즉, `null` 값이 들어올 경우 예외가 던져지므로 `User` 객체 자체가 생성되지 않기 때문에 저장도 되지 않습니다.
        - 그리고 예외를 던지는 과정에서 어떤 값이 들어왔는지 알려주면 좋기 때문에 `String.format`을 사용하여 예외의 메세지로 담을 수 있게끔 처리했습니다.

## 6️⃣ `User` 클래스를 인스턴스화 시켜 저장되게 만들기.
- `UserController` 클래스 안에 다음과 같이 코드를 작성해줍니다.

```java
package com.kobe.signUpApp.controller.user;

import com.kobe.signUpApp.domain.user.User;
import com.kobe.signUpApp.dto.user.request.UserCreateRequest;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;
import java.util.List;

@RestController
public class UserController {

    private final List<User> users = new ArrayList<>();

    @PostMapping("/register")
    public void registerUser(@RequestBody UserCreateRequest request) {
        users.add(new User(request.getUsername(), request.getEmail(), request.getPassword(), request.getConfirmPassword()));
    }
}
```

- 먼저 `private final List<User> users = new ArrayList<>();` 로 리스트를 만들어 줍니다.
- `registerUser`라는 함수가 호출되면, 즉 `POST/registerUser`가 호출될 경우 `users.add(new User(request.getUsername(), request.getEmail(), request.getPassword(), request.getConfirmPassword()));` 가 실행 됩니다.
    - User를 만들때는 API를 통해 들어온 유저이름, 이메일, 비밀번호, 확인용 비밀번호를 집어 넣어줍니다
        - 즉, 위 코드는 "API를 통해 들어온 유저이름, 이메일, 비밀번호, 확인용 비밀번호를 집어 넣는 코드입니다."

## 7️⃣ 요약.
- `POST/registerUser`가 호출될 경우, `UserController` 내부에 있는 `registerUser` 함수가 실행됩니다.
    - 이 때, JSON 형식으로 `HTTP Body`에 `username, email, password, confirmPassword`가 들어오면 `UserCreateRequest`의 객체의 값으로 맵핑되고 `public void registerUser(@RequestBody UserCreateRequest request)`의 `request`는 새로운 `User`를 만드는데 사용됩니다.
        - 이렇게 새롭게 만드는데 사용된 `User` 객체는 리스트에 저장됩니다.
            - `registerUser` 함수가 예외 없이 끝나게 된다면 200 OK를 반환하게 됩니다.
