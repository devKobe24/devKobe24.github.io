---
title: 🍃[Spring] API에서 예외 던지기.
tags:
    - Spring
    - Framework
date: "2024-10-02"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] API에서 예외 던지기.

Java 백엔드 애플리케이션에서 API 호출 중 예외를 던지면, 예외가 적절하게 처리되지 않는 한 클라이언트에 오류 응답이 반환됩니다.

기본적으로 예외가 발생하면 애플리케이션은 예외가 발생한 시점에서 실행을 중단하고, 예외에 대한 처리를 하지 않으면 내부 서버 오류(HTTP 500)를 클라이언트에 반환하게 됩니다.

## 1️⃣ 예외 처리 흐름.

### 1. 예외 발생.
- API의 컨트롤러나 서비스 레이어에서 특정 로직을 수행하는 도중 예외가 발생할 수 있습니다.
- 예외가 발생하면 실행 흐름이 예외 처리 블록으로 넘어가거나, 예외가 호출된 메서드를 통해 상위로 전파됩니다.

### 2. 예외 전파.
- 만약 해당 예외를 `try-catch` 블록 내에서 처리하지 않으면 예외는 호출된 메서드를 따라 상위로 전파됩니다.
- 이 과정에서 최종적으로 컨트롤러 레벨이나 그 이상에서 예외를 잡지 않으면, Spring Framework와 같은 백엔드 프레임워크가 예외를 받아 처리하게 됩니다.

### 3. Spring의 기본 동작.
- Spring MVC에서는 예외가 전파되어 컨트롤러까지 도달하고도 처리되지 않으면, Spring이 `DefaultHandlerExceptionResolver`를 통해 기본적인 예외 처리를 수행합니다.
- 처리되지 않은 예외는 기본적으로 HTTP 상태 코드 500(내부 서버 오류)로 매핑되어 클라이언트에 반환됩니다.
- 클라이언트는 이 상태 코드를 통해 서버에서 오류가 발생했음을 인식하게 됩니다.

## 2️⃣ 예외 처리를 위한 방법.

### 1. `@ExceptionHandler` 사용.
- Spring MVC에서는 컨트롤러 레벨에서 예외를 처리하기 위해 `@ExceptionHandler` 어노테이션을 사용할 수 있습니다.
- 특정 예외가 발생했을 때 해당 메서드가 호출되어 예외를 처리하고, 적절한 응답을 클라이언트에게 반환할 수 있습니다.

```java
@RestController
public class MyController {
    @GetMapping("/example")
    public String example() {
        if (someConditionFails()) {
            throw new MyCustomException("Something went wrong!");
        }
        return "success";
    }
    
    @ExceptionHandler(MyCustomException.class)
    public ResponseEntity<String> handleMyCustomException(MyCustomException ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.BAD_REQUEST);
    }
}
```

이 경우 `MyCustomException`이 발생하면 HTTP 상태 코드 400과 함께 예외 메시지가 반환됩니다.

### 2. `@ControllerAdvice` 사용.
- `@ControllerAdvice`는 전역 예외 처리기를 정의하는 데 사용됩니다.
- 이를 통해 애플리케이션 전반에 발생하는 예외를 중앙에서 처리할 수 있습니다.

```java
@ControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(MyCustomException.class)
    public ResponseEntity<String> handleMyCustomException(MyCustomException ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.BAD_REQUEST);
    }
    
    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleGeneralException(Exception ex) {
        return new ResponseEntity<>("An unexpected error occurred.", HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

이 경우 특정 예외뿐만 아니라 모든 일반적인 예외도 처리할 수 있습니다.

### 3. ResponseEntity와 함께 예외를 반환.
- `ResponseEntity`를 사용하여 직접 예외 발생 시 HTTP 응답과 상태 코드를 반환할 수도 있습니다.

```java
@GetMapping("/example")
public ResponseEntity<String> example() {
    if (someConditionFails()) {
        return new ResponseEntity<>("Custom error message", HttpStatus.BAD_REQUEST);
    }
    return new ResponseEntiry<>("success", HttpStatus.OK);
}
```

## 3️⃣ 실제 코드 사례.
```java
// UserUpdateRequest - DTO
public class UserUpdateRequest {
    private long id;
    private String name;
    
    public long getId() {
        return id;
    }
    
    public String getName() {
        return name;
    }
}

// UserController - Controller
@PutMapping("/user")
public void updateUser(@RequestBody UserUpdateRequest request) {
    String sql = "UPDATE user SET name = ? WHERE id = ?";
    jdbcTemplate.update(sql, request.getName(), request.getId());
}

@DeleteMapping("/user")
public void deleteUser(@RequestParam String name) {
    String sql = "DELETE FROM user WHERE name = ?";
    jdbcTemplate.update(sql, name);
}
```

### 1. 문제 상황.
- 위 코드에서의 문제 상황은 없는 유저를 업데이트 하거나 삭제하려 해도 **200 OK**가 나온다는 점입니다.

### 2. 해결 방안.
- API에서 예외를 던저 **500 Internal Server Error**이 나오게 하면됩니다.

```java
@GetMapping("/user/error-test")
public void errorTest() {
    throw new IllegalArgumentException();
}
```

<img src = "https://github.com/devKobe24/images2/blob/main/springboot/postman-error-test.png?raw=true">

- POSTMAN을 활용하여 `http://localhost:8080/user/error-test`로 **GET** 호출을 보내면 **500 Internal Server Error**이 발생합니다.

## 4️⃣ 활용 예시.
```java
// UserUpdateRequest - DTO
public class UserUpdateRequest {
    private long id;
    private String name;
    
    public long getId() {
        return id;
    }
    
    public String getName() {
        return name;
    }
}

// UserController - Controller
@PutMapping("/user")
public void updateUser(@RequestBody UserUpdateRequest request) {
    String readSql = "SELECT * FROM user WHERE id = ?";
    boolean isUserExist = jdbcTemplate.query(readSql, (rs, rowNum) -> 0 request.getId()).isEmpty();
    
    if (isUserNotExist) {
        throw new IllegalArgumentException();
    }
    
    String sql = "UPDATE user SET name = ? WHERE id = ?";
    jdbcTemplate.update(sql, request.getName(), request.getId());
}

@DeleteMapping("/user")
public void deleteUser(@RequestParam String name) {
    String readSql = "SELECT * FROM user WHERE name = ?";
    boolean isUserExist = jdbcTemplate.query(readSql (rs, rowNum) -> 0, name).isEmpty();
    
    if (isUserNotExist) {
        throw new IllegalArgumentException();
    }
    
    String sql = "DELETE FROM user WHERE name = ?";
    jdbcTemplate.update(sql, name);
}
```

- 위와 같이 **API에서 데이터 존재 여부를 확인해 예외를 던지면 됩니다.**

```java
String readSql = "SELECT * FROM user WHERE id = ?";
```
- id를 기준으로 유저가 존재하는지 확인하기 위해 SELECT 쿼리를 작성했습니다.

```java
boolean isUserExist = jdbcTemplate.query(readSql, (rs, rowNum) -> 0 request.getId()).isEmpty();
```
- SQL을 날려 DB에 데이터가 있는지 확인합니다.
    - SELECT SQL의 결과가 있으면 0으로 변환됩니다.
    - 최종적으로 **List**로 반환됩니다.
        - 즉, 해당 id를 가진 유저가 있으면 **0이 담긴 List**가 나오고, 없다면 **빈 List**가 나오게 됩니다.
        - jdbcTemplate.query()의 결과인 List가 비어있다면, 유저가 없다는 뜻입니다.

```java
if (isUserNotExist) {
    throw new IllegalArgumentException();
}
```
- 만약 유저가 존재하지 않는다면 IllegalArgumentException을 던집니다.
