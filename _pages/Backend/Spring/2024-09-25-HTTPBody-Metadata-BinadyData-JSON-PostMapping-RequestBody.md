---
title: 🍃[Spring]  HTTP Body, 메타데이터(Metadata), 바이너리 데이터(Binary Data), JSON(JavaScript Object Notation), `@PostMapping` 애너테이션, `@RequestBody` 애너테이션.
tags:
    - Spring
    - Framework
date: "2024-09-25"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] HTTP Body

## 1️⃣ HTTP Body.
- **HTTP Body** 는 HTTP 요청 또는 응답에서 실제 데이터를 포함하는 부분을 말합니다.
- 주로 클라이언트와 서버 간의 데이터 교환을 위한 목적으로 사용되며, **요청 본문(Request Body)** 과 **응답 본문(Response Body)** 으로 나뉩니다.

### 1. HTTP Body의 구조.

#### HTTP 메시지의 구성요소.
- **1. 요청/응답 라인**
    - HTTP 메서드(GET, POST 등)나 상태 코드(200, 404 등)가 포함된 첫 번째 줄입니다.
- **2. 헤더(Header)**
    - 메타데이터(요청/응답의 속성, 데이터 형식, 인코딩 방식 등)를 포함하며, 각 필드가 **key-value 형식** 으로 나열됩니다.
- **3. 본문(Body)**
    - 실제 데이터가 들어가는 부분입니다.

HTTP Body는 이러한 메시지의 마지막 부분에 위치하며, 클라이언트가 서버로 보내는 데이너타 서버가 클라이언트로 보내는 데이터가 여기에 포함됩니다.

### 2. HTTP Body의 사용 목적.
- **요청(Request) Body**
    - 클라이언트가 서버로 보내는 데이터.
    - 주로 POST, PUT, PATCH 등의 요청에서 사용됩니다.
    - 예를 들어, 사용자 로그인 정보, 파일 업로드, 새로운 리소스 생성 등의 데이터를 서버로 보낼 때 사용됩니다.
- **응답(Response) Body**
    - 서버가 클라이언트에게 응답할 때, 필요한 데이터를 본문에 포함하여 보냅니다.
    - 예를 들어, API 호출에 대한 결과로 JSON 형식의 데이터를 반환하거나, HTML 페이지를 반환할 수 있습니다.

### 3. HTTP Body가 없는 경우.
일부 요청은 Body를 포함하지 않기도 합니다.

- **GET 요청**
    - 주로 데이터를 서버로부터 요청하는 데 사용되며 Body가 포함되지 않습니다.
- **DELETE 요청**
    - 리소스를 삭제할 때 사용되며, 일반적으로 Body가 없습니다.

그러나 **POST, PUT, PATCH 요청** 은 보통 Body에 데이터를 포함하여 전송합니다.

### 4. HTTP Body의 형식.
- HTTP Body는 다양한 데이터 형식을 가질 수 있으며, 데이터 형식은 **Content-Type** 헤더에 의해 정의됩니다.

#### 자주 사용되는 형식.
- **1. `application/json`**
    - JSON 형식의 데이터를 전달할 때 사용합니다.
    - 예시
    ```json
    {
        "name": "Kobe"
        "email": "kobe@example.com"
    }
    ```
    
- **2. `application/x-www-form-urlencoded`**
    - HTML 폼 데이터를 URL 인코딩하여 전달할 때 사용됩니다.
    - 폼 필드가 `key=value` 형식으로 인코딩되어 전송됩니다.
    - 예시
    ```bash
    name=Kobe&email=kobe%40example.com
    ```

- **3. `multipart/form-data`**
    - 파일 업로드와 같은 바이너리 데이터가 포함된 폼 데이터를 전송할 때 사용합니다.
    - 각 파트가 여러 부분으로 나뉘어 전송됩니다.
    - 예시
    ```json
    --boundary
    Content-Disposition: form-data; name="name"

    Kobe
    --boundary
    Content-Disposition: form-data; name="file"; filename="profile.jpg"
    Content-Type: image/jpeg

    [바이너리 데이터]
    --boundary--
    ```
    
- **4. `text/plain`**
    - 단순한 텍스트 데이터를 전송할 때 사용됩니다.
    - 예시
    ```bash
    Hello, this is a plain text message.
    ```

- **5. 기타 바이너리 데이터.**
    - PDF, 이미지, 비디오 등 다양한 미디어 파일이 Body에 포함될 수 있습니다.
    - `Content-Type` 헤더에 맞게 데이터가 인코딩됩니다.

### 5. HTTP Body의 예시.

#### 요청(Request) Body 예시 (POST 요청)

```http
POST /login HTTP/1.1

Host: www.example.com
Content-Type: application/json
Content-Length: 51

{
    "username": "Kobe",
    "password": "12345"
}
```

- 헤더에 `Content-Type: application/json`이 명시되어 있으며, 이는 Body가 JSON 형식임을 나타냅니다.
- Body에는 로그인 정보를 포함한 JSON 데이터가 담겨 있습니다.

#### 응답(Response) Body 예시.

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 85

{
    "status": "success",
    "data": {
        "id": 1,
        "name": "Kobe",
        "email": "kobe@example.com"
    }
}
```

- 헤더에 `Content-Type: application/json`이 설정되어 있으며, 서버가 클라이언트에게 JSON 데이터를 반환합니다.
- Body에는 서버가 응답으로 보낸 데이터가 JSON 형식으로 포함되어 있습니다.

### 6. 요약.
- HTTP Body는 클라이언트와 서버 간의 데이터를 전송하는 부분입니다.
- 주로 POST, PUT, PATCH 요청에서 데이터를 전송할 때 사용되며, 서버의 응답에도 데이터가 포함될 수 있습니다.
- 데이터 형식은 Content-Type 헤더를 통해 정의되며, JSON, HTML, XML, 바이너리 데이터 등 다양한 형식이 가능합니다.

## 2️⃣ 메타데이터(Metadata)
- **메타데이터(Metadata)** 는 데이터를 설명하는 데이터, 즉 데이터에 대한 추가 정보를 제공하는 데이터입니다.
- 메타데이터는 특정 데이터가 무엇을 나타내는지, 어떻게 사용되어야 하는지, 그리고 데이터의 속성에 대한 다양한 정보를 담고 있습니다.
- 이를 통해 데이터를 더 쉽게 이해하고 관리할 수 있게 합니다.

### 1. 메타데이터의 역할.
- 메타데이터는 데이터 자체에 대한 설명을 제공함으로써, 데이터의 구조, 의미,목적 등을 명확히 알려줍니다.
- 메타데이터는 여러 맥락에서 사용할수 있습니다.

#### 메타 데이터의 주요한 역할.
- **1. 데이터 설명.**
    - 데이터의 의미, 형식, 구조를 설명해 줍니다.
- **2. 데이터 검색.**
    - 데이터를 쉽게 검색하고 찾을 수 있도록 도와줍니다.
    - 예를 들어, 도서관의 카탈로그에서 책을 찾기 위해 제목, 저자, 출판 연도 등을 검색할 수 있는 것은 메타데이터 덕분입니다.
- **3. 데이터 관리.**
    - 데이터를 분류하고 조직화하는 데 도움을 줍니다.
- **4. 데이터 통제.**
    - 메타데이터를 통해 데이터의 접근 권한이나 사용 범위를 관리할 수 있습니다.

### 2. 메타데이터의 유형.
- 메타데이터는 다양한 유형으로 분류될 수 있으며, 그 목적에 따라 다르게 사용됩니다.

#### 일반으로 많이 사용되는 메타데이터의 세 가지 유형.
- **1. 구조적 메타데이터(Structural Metadata)**
    - 데이터의 구조나 형식을 설명하는 정보입니다.
    - 예를 들어, 데이터베이스에서 각 테이블의 필드와 데이터 유형을 설명하는 정보가 여기에 해당됩니다.

- **2. 기술적 메타데이터(Technical Metadata)**
    - 데이터를 생성하고 관리하는 데 필요한 기술적인 정보를 포함합니다.
        - 파일의 생성 날짜, 수정 날짜, 인코딩 방식 등이 여기에 해당됩니다.
    - 시스템에서 데이터를 처리하거나 이동시키는 데 필요한 정보가 포함됩니다.
    - 예시: 파일 생성 일자, 수정 일자, 파일 소유자, 인코딩 방식, 해상도.

- **3. 설명적 메타데이터(Descriptive Metadata)**
    - 데이터를 설명하는 정보로, 데이터의 내용, 주제 키워드 등을 설명하는 데 사용됩니다.
        - 도서관의 카탈로그나 미디어 파일의 제목, 저자, 출판 정보 등이 이에 해당됩니다.
    - 예시: 도서의 제목, 저자, 출판일, 키워드, 요약 정보.

### 3. 메타데이터의 예시.
- **1. 웹 페이지의 메타데이터.**
    - HTML 문서에서는 `<meta>` 태그를 사용하여 웹 페이지에 대한 메타데이터를 정의합니다.
    - 이는 주로 검색 엔진이 웹 페이지를 더 잘 이해하고, 인덱싱할 수 있도록 도와줍니다.
```html
<meta charset="UTF-8">
<meta name="description" content="An example of metadata in a webpage.">
<meta name="keywords" content="metadata, HTML, web development">
<meta name="author" content="Kobe">
```

- `charset` : 문서의 문자 인코딩 방식.
- `description` : 웹 페이지의 내용에 대한 설명.
- `keyword` : 웹 페이지와 관련된 키워드.
- `author` : 문서 작성자.

- **2. 이미지 파일의 메타데이터.**
    - 이미지 파일에도 메타데이터가 포함됩니다.
    - 이미지 파일의 메타데이터는 카메라 설정, 촬영 위치, 해상도 등 다양한 정보를 제공할 수 있습니다.
    - 이를 **EXIF(Exchangeable Image File Format)** 메타데이터라고 부릅니다.
```bash
예시 : 사진 메타데이터
- 카메라 제조사: Nikon
- 카메라 모델: D3500
- 촬영 일시: 2024-09-24
- 해상도: 6000 * 4000
- GPS 좌표: 촬영 위치 정보
```

- **3. 도서 메타데이터.**
    - 도서의 메타데이터는 도서관 카탈로그나 전자책 파일에서 찾아볼 수 있습니다.
    - 예를 들어, 책의 제목, 저자, 출판사, ISBN 등이 메타데이터입니다.

```bash
- 제목: "1984"
- 저자: 조지 오웰
- 출판사: 하퍼콜린스
- 출판 연도: 1949
- ISBN: 978-0451524935
```

- **4. 동영상 메타데이터.**
    - 동영상 파일에도 다양한 메타데이터가 포함될 수 있습니다.
    - 동영상의 제목, 길이, 해상도, 비트레이트, 제작 날짜 등이 여기에 해당됩니다.

```bash
- 제목: "Vaction Highlights"
- 길이: 2시간 15분
- 해상도: 1920 * 1080
- 비디오 코덱: H.264
- 파일 크기: 1.2GB
```

### 4. 메타데이터의 중요성.
- **1. 데이터 관리.**
    - 메타데이터는 데이터를 효율적으로 관리하고 유지하는 데 필수적입니다.
    - 예를 들어, 검색 시능을 향상시키고, 데이터를 분류하고 정리하는 데 도움을 줍니다.

- **2. 데이터 검색 및 접근성.**
    - 메타데이터는 대량의 데이터를 쉽게 탐색하고 원하는 데이터를 빠르게 찾을 수 있도록 돕습니다.
    - 검색 엔진은 웹페이지의 메타데이터를 분석하여 검색 결과를 더욱 정확하게 제공합니다.

- **3. 데이터 보존.**
    - 파일의 생성 일자, 수정 일자, 버전 정보 등의 메타데이터는 데이터의 이력을 추적하는 데 유용하며, 데이터를 장기적으로 보존하고 관리하는 데 도움을 줍니다.

### 5. 요약.
- 메타데이터는 데이터를 설명하는 데이터로, 파일이나 정보를 보다 쉽게 이해하고 관리할 수 있도록 도와줍니다.
- 메타데이터는 여러 유형으로 나뉘며, 웹페이지, 이미지, 동영상, 도서 등 다양한 형태의 데이터에 적용되어 데이터의 검색, 관리, 보존에 중요한 역할을 합니다.

## 3️⃣ 바이너리 데이터(Binary Data)
- **바이너리 데이터(Binary Data)** 는 0과 1로 구성된 이진수 형태로 저장된 데이터를 말합니다.
- 컴퓨터 시스템은 모든 데이터를 기본적으로 이진수, 즉 바이너리 형태로 처리하기 때문에 바이너리 데이터는 컴퓨터가 이해할 수 있는 가장 기본적인 데이터 형식입니다.
- 텍스트 데이터는 사람이 쉽게 읽을 수 있는 형태인 반면, 바이너리 데이터는 사람이 바로 읽을 수 없는 형식으로 저장됩니다.

### 1. 바이너리 데이터의 특징.
- **1. 이진수로 표현.**
    - 바이너리 데이터는 0과 1로 구성된 이진수로 표현됩니다.
    - 컴퓨터는 모든 데이터를 전기 신호(켜짐 = 1, 꺼짐 = 0)로 처리하기 때문에, 바이너리 데이터는 컴퓨터의 기본 데이터 표현 방식입니다.

- **2. 사람이 읽을 수 없는 형식.**
    - 바이너리 데이터는 사람이 직접 읽거나 해석하기 어렵습니다.
    - 예를 들어, 이미지 파일이나 실행 파일과 같은 데이터는 일반적으로 바이너리로 저장되며, 이를 직접 열면 알아볼 수 없는 기호들이 나옵니다.

- **3. 파일 형식.**
    - 대부분의 파일 형식이 바이너리 형식으로 저장됩니다.
    - 예를 들어, 이미지(JPEG, PNG), 비디오(MP4), 오디오(MP3), 실행 파일(EXE) 등은 모두 바이너리 형식으로 저장되며, 이를 사람이 읽을 수 있는 형식으로 변환하려면 특정 프로그램이나 소프트웨어가 필요합니다.

### 2. 바이너리 데이터의 예.
- **1. 이미지 파일(JPEG, PNG 등)**
    - 이미지 파일은 픽셀 값들이 0과 1로 구성된 바이너리 데이터로 저장됩니다.
    - 이를 열고 표시하려면 이미지 뷰어와 같은 소프트웨어가 필요합니다.

- **2. 오디오 파일(MP3, WAV 등)**
    - 오디오 파일은 음향 신호를 디지털화한 바이너리 데이터로 저장됩니다.
    - 음악 플레이어 프로그램을 통해 이를 재생할 수 있습니다.

- **3. 동영상 파일(MP4, AVI 등)**
    - 동영상 파일은 영상과 음향을 결합한 바이너리 데이터로 저장됩니다.
    - 이를 보기 위해서는 비디오 플레이어가 필요합니다.

- **4. 실행 파일(EXE, DLL 등)**
    - 운영 체제에서 실행할 수 있는 프로그램은 바이너리로 저장됩니다.
    - 사용자가 프로그램을 실행하면 컴퓨터는 이 바이너리 데이터를 해석하여 작업을 수행합니다.

- **5. 압축 파일(ZIP, RAR 등)**
    - 압축 파일은 여러 파일을 바이너리 형태로 압축하여 하나의 파일로 묶은 것입니다.
    - 압축 해제 소프트웨어를 통해 원래 파일로 복원할 수 있습니다.

### 3. 바이너리 데이터 VS 텍스트 데이터.
- **텍스트 데이터**
    - 텍스트 데이터는 사람이 읽을 수 있는 형태로 저장된 데이터입니다.
    - 주로 알파벳, 숫자, 기호 등으로 구성된 문자를 포함하며, 예를 들어 HTML, JSON, XML 파일 등이 있습니다.
    - 텍스트 데이터는 일반 텍스트 편집기로 쉽게 열고 읽을 수 있습니다.

- **바이너리 데이터**
    - 바이너리 데이터는 사람이 쉽게 읽을 수 없는 이진수(0과 1)의 조합으로 저장된 데이터입니다.
    - 이미지, 동영상, 실행 파일과 같은 데이터는 이진수로 인코딩된 바이너리 데이터 형식으로 저장됩니다.

### 4. 바이너리 데이터의 활용.
- **1. 네트워크 전송**
    - 대용량 파일(예: 이미지, 동영상, 소프트웨어 등)을 네트워크를 통해 전송할 때 바이너리 형식으로 전송됩니다.
    - 바이너리 데이터는 압축 및 인코딩을 통해 효율적으로 전송됩니다.

- **2. 파일 저장**
    - 컴퓨터의 저장 장치(HDD, SSD 등)에서 모든 데이터는 바이너리 형식으로 저장됩니다.
    - 텍스트, 이미지, 오디오, 비디오, 실행 파일 등 모든 파일은 결국 바이너리 데이터로 변환되어 저장됩니다.

- **3. 멀티미디어 처리**
    - 이미지, 오디오, 비디오 등의 멀티미디어 파일은 모두 바이너리 데이터로 저장되며, 이러한 데이터를 처리하기 위해서는 적절한 소프트웨어와 코덱이 필요합니다.

### 5. 바이너리 데이터를 처리하는 방법.
- 바이너리 데이터를 처리하려면 파일을 열어 이진수 데이터를 읽고 해석할 수 있는 소프트웨어나 프로그래밍 언어가 필요합니다.
- 예를 들어, Java, Python, C++ 등 다양한 프로그래밍 언어는 바이너리 데이터를 읽고 쓰는 기능을 제공합니다.

#### 예시: Java에서 바이너리 파일 읽기.
```java
import java.io.FileInputStream;
import java.io.IOException;

public class BinaryFileReader {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("image.jpg")) {
            int byteData;
            while ((byteData = fis.read()) != -1) {
                // 1바이트씩 읽어들임
                System.out.println(byteData);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- 위 예시에서는 `FileInputStream`을 사용하여 바이너리 파일을 1바이트씩 읽는 방법을 보여줍니다.

### 6. 요약.
- 바이너리 데이터는 컴퓨터가 처리하는 기본 데이터 형식으로, 0과 1의 이진수로 구성됩니다.
- 주로 이미지, 오디오, 비디오, 실행 파일, 압축 파일 등에서 사용되며, 사람이 직접 읽기 어렵습니다.
- 텍스트 데이터와는 달리, 바이너리 데이터는 컴퓨터가 해석할 수 있는 형식으로 저장되며, 파일을 열고 처리하려면 특정 소프트웨어가 필요합니다.

## 4️⃣ JSON(JavaScript Object Notation)
- **JSON(JavaScript Object Notation)** 은 데이터를 저장하고 교환하기 위한 경량 데이터 형식입니다.
- 사람이 읽고 쓰기 쉬우며, 기계가 분석하고 생성하기도 쉽도록 설계되었습니다.
- JSON은 텍스트 형식이므로 모든 프로그래밍 언어에서 쉽게 파싱하고 생성할 수 있습니다.

### 1. JSON의 특징.
- **1. 경량 데이터 형식.**
    - JSON은 구조가 간단하고 용량이 작어, 특히 웹 애플리케이션에서 서버와 클라이언트 간의 데이터를 주고받는 데 널리 사용됩니다.

- **2. 언어 독립적.**
    - JSON은 특정 프로그래밍 언어에 의존하지 않으며, 대부분의 언어에서 JSON 데이터를 처리할 수 있는 라이브러리나 메서드를 제공합니다.
    - 자바스크립트 문법을 기반으로 하지만, Python, Java, C#, PHP 등에서도 쉽게 사용 가능합니다.

- **3. 텍스트 기반.**
    - JSON은 텍스트로 이루어져 있어 사람이 읽고 이해하기 쉽습니다.
    - 이는 디버깅이나 데이터의 전송 및 저장에 매우 유리합니다.

### 2. JSON의 구조.

#### JSON 데이터의 기본적인 두 가지 구조.
- **1. 객체(Object)** 
    - **중괄호 `{}`** 로 감싸진 키-값 쌍의 집합.
        - 키는 문자열이고, 값은 문자열, 숫자 불리언, 배열, 객체 또는 `null`이 될 수 있습니다.

- **2. 배열(Array)**
    - **대괄호 `[]`** 로 감싸진 값들의 목록.
        - 배열 안의 값은 순차적으로 저장되며, 각 값은 문자열, 숫자, 불리언, 객체, 배열 또는 `null`일 수 있습니다.

#### 예시: JSON 객체와 배열.
```json
{
    "name": "Kobe",
    "age": 30,
    "isStudent": false,
    "skills": ["Java", "JavaScript", "Swift"],
    "address": {
        "city": "Seoul",
        "zipcode": "12345"
    }
}
```

- **객체**
    - 이 예제에서 전체 데이터는 JSON 객체로, 중괄호 `{}` 안에 여러 키-값 쌍이 포함되어 있습니다.

- **배열**
    - `skills`는 배열로, 사용자가 가진 프로그래밍 언어들을 리스트로 나타냅니다.

- **중첩된 객체**
    - `address`는 또 다른 JSON 객체로, 중첩된 구조를 가집니다.

### 3. JSON의 데이터 타입.

#### JSON에서 사용할 수 있는 기본 데이터 타입은 다음과 같습니다.
- **1. 문자열(String)**
    - 큰 따옵표`"`로 감싸인 텍스트.
        - 예: `"Kobe"`

- **2. 숫자(Number)**
    - 정수 또는 실수.
        - 예: `30`, `3.14`

- **3. 불리언(Boolean)**
    - `true` 또는 `false`
        - 예: `true`, `false`

- **4. 객체(Object)**
    - 중괄호 `{}`로 감싸인 키-값 쌍의 집합.
        - 예: `{"name": "Kobe", "age": 30}`

- **5. 배열(Array)**
    - 대괄호 `[]`로 감싸인 값들의 리스트.
        - 예: `["Java", "JavaScript", "Swift"]`

- **6. null**
    - 값이 없음을 나타냅니다.
        - 예: `null`

### 4. JSON의 사용 예.

#### 1. 서버와 클라이언트 간 데이터 전송

웹 애플리케이션에서는 서버와 클라이언트 간에 JSON 형식으로 데이터를 주고받는 경우가 많습니다.
예를 들어, 사용자가 로그인할 때 서버로 전송하는 데이터는 다음과 같이 JSON 형식일 수 있습니다.

#### 요청(Request)
```json
{
    "username": "kobe",
    "password": "12345"
}
```

#### 응답(Response)
```json
{
    "status": "success",
    "userId": 101,
    "name": "kobe"
}
```

#### 2. API 응답 형식

많은 RESTful API는 JSON을 응답 형식으로 사용합니다.
예를 들어, 날씨 정보를 제공하는 API는 다음과 같은 JSON 데이터를 반환할 수 있습니다.
```json
{
    "location": "Seoul",
    "temperature": 23,
    "weather": "Sunny"
}
```

#### 3. 설정 파일

JSON은 설정 파일 형식으로도 많이 사용됩니다.
예를 들어, JavaScript 프로젝트의 `package.json` 파일은 프로젝트의 설정 정보를 JSON 형식으로 저장합니다.

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "A sample project",
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

### 5. JSON 파싱과 생성.

각 프로그래밍 언어는 JSON 데이터룰 파싱하고 생성하는 방법을 지원합니다.
예를 들어, JavaScript와 Python 그리고 Java에서 JSON을 처리하는 방법은 다음과 같습니다.

#### JavaScript에서 JSON 처리
```javascript
// JSON 문자열을 객체로 변환 (파싱)
let jsonString = '{"name": "John", "age": 30}';
let obj = JSON.parse(jsonString);

console.log(obj.name); // "John"

// 객체를 JSON 문자열로 변환
let newJsonString = JSON.stringify(obj);
console.log(newJsonString); // '{"name":"John","age":30}'
```

#### Python에서 JSON 처리
```python
import json

# JSON 문자열을 객체로 변환 (파싱)
json_string = '{"name": "John", "age": 30}'
data = json.loads(json_string)

print(data["name"])  # "John"

# 객체를 JSON 문자열로 변환
new_json_string = json.dumps(data)
print(new_json_string)  # '{"name": "John", "age": 30}'
```

#### Java에서 JSON 처리
- Java에서 JSON을 처리하는 방법은 여러 가지 라이브러리를 통해 가능합니다.
- 가장 많이 사용되는 라이브러리로는 **Jackson, Gson 그리고 JSON.simple 등** 이 있습니다.

#### Jackson을 사용한 JSON 처리
- Jackson 라이브러리는 JSON 데이터를 직렬화 및 역직렬화하는 데 강력한 기능을 제공합니다.
- 이를 통해 Java 객체를 JSON 형식으로 변환하거나, JSON 문자열을 Java 객체로 변환할 수 있습니다.

**1. Maven 또는 Gradle에 Jackson 라이브러리 추가.**
```xml
// MAVEN
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.3</version> <!-- 사용하고자 하는 버전 -->
</dependency>
```

```gradle
// GRADLE
implementation 'com.fasterxml.jackson.core:jackson-databind:2.12.3'
```

**2. Jackson을 사용하여 JSON 문자열을 Java 객체로 변환 (파싱)**
```java
import com.fasterxml.jackson.databind.ObjectMapper;

class User {
    private String name;
    private int age;
    
    // 기본 생성자, getter, setter가 필요함.
    public User() {}
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        this.age = age;
    }
}

public class JsonExample {
    public static void main(String[] args) {
        String jsonString = "{\"name\":\"Kobe\", \"age\":30}";
        
        ObjectMapper objectMapper = new ObjectMapper();
        try {
            // JSON 문자열을 Java 객체로 변환.
            User user = objectMapper.readValue(jsonString, User.class);
            
            System.out.println(user.getName()); // Kobe
            System.out.println(user.getAge()); // 30
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

**3. Jackson을 사용하여 Java 객체를 JSON 문자열로 변환.**
```java
import com.fasterxml.jackson.databind.ObjectMapper;

public class JsonExample {
    public static void main(String[] args) {
        User user = new User();
        user.setName("Kobe");
        user.setAge(30);
        
        ObjectMapper objectMapper = new ObjectMapper();
        try {
            // Java 객체를 JSON 문자열로 변환
            String jsonString = objectMapper.writeValueAsString(user);
            
            System.out.println(jsonString); // {"name": "Kobe", "age": 30}
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### Gson을 사용한 JSON 처리.
Gson은 Google에서 개발한 JSON 라이브러리로, 간단하게 Java 객체를 JSON으로 직렬화하거나 JSON 문자열을 Java 객체로 역직렬화할 수 있습니다.

#### 1. Maven 또는 Gradle에 Gson 라이브러리 추가.

```xml
// MAVEN
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.8.8</version> <!-- 사용하고자 하는 버전 -->
</dependency>
```

```gradle
// GRADLE
implementation 'com.google.code.gson:gson:2.8.8'
```

#### 2. Gson을 사용하여 JSON 문자열을 Java 객체로 변환(파싱)
```java
import com.google.gson.Gson;

public class JsonExample {
    public static void main(String[] args) {
        String jsonString = "{\"name\":\"Kobe\", \"age\":30}";
        
        Gson gson = new Gson();
        
        // JSON 문자열을 Java 객체로 변환
        User user = gson.fromJson(jsonString, User.class);
        
        System.out.println(user.getName()); // Kobe
        System.out.println(user.getAge()); // 30
    }
}
```

#### 3. Gson을 사용하여 Java 객체를 JSON 문자열로 변환
```java
import com.google.gson.Gson;

public class JsonExample {
    public static void main(String[] args) {
        User user = new User();
        user.setName("Kobe");
        user.setAge(30);
        
        Gson gson = new Gson();
        
        // Java 객체를 JSON 문자열로 변환.
        String jsonString = gson.toJson(user);
        
        System.out.println(jsonString)l // {"name": "Kobe", "age": 30}
    }
}
```

### 6. JSON과 XML 비교
- JSON과 XML은 모두 데이터를 구조화하는 데 사용되는 포맷입니다.
- 그러나 JSON은 XML에 비해 더 간결하고, 읽기 쉽고 처리 속도가 빠르다는 장점이 있어 현대 웹 애플리케이션에서 더 많이 사용됩니다.



| 비교 항목 | JSON | XML  |
| --------- | ---- | ---- |
|구조|키-값 쌍으로 이루어진 간결한 구조|태그로 둘러싸인 트리 구조|
|가독성|사람이 읽고 쓰기 쉬움|상대적으로 더 복잡함|
|데이터 크기|더 작은 크기|더 큰 크기|
|유연성|객체 및 배열 표현이 직관적|배열 표현이 상대적으로 복잡함|
|지원|대부분의 언어 및 프레임워크에서 지원|오래된 시스템과의 호환성이 좋음|


### 7. 요약.
- Java에서는 JSON 데이터를 처리하기 위해 주로 Jackson과 Gson 라이브러리를 사용합니다.
- 이 라이브러리들은 Java 객체와 JSON 간의 변환을 간단하고 효율적으로 할 수 있게 해줍니다.
- `ObjectMapper`(Jackson) 또는 `Gson` 객체를 사용하여 JSON 데이터를 Java 객체로 변환하거나, 반대로 JSON으로 변환할 수 있습니다.
- JSON은 데이터를 저장하고 교환하는 데 사용되는 경량 텍스트 형식입니다.
- 구조는 객체와 배열로 구성되며, 다양한 데이터 타입(문자열, 숫자, 객체, 배열 등)을 지원합니다.
- 웹 애플리케이션에서 서버와 클라이언트 간의 데이터 전송에 널리 사용됩니다.
- 대부분의 프로그래밍 언어에서 쉽게 파싱하고 생성할 수 있는 라이브러리를 제공합니다.
- 간결한 구조 덕분에 XML보다 많이 사용되며, 특히 RESTful API에서 주로 사용됩니다.

## 5️⃣ `@PostMapping` 애너테이션.
- **`@PostMapping` 애너테이션** 은 Spring Framework에서 HTTP **POST** 요청을 처리하기 위해 사용하는 애너테이션입니다.
- 주로 클라이언트가 서버로 데이터를 전송할 때 사용됩니다.
- 예를 들어, 폼 데이터나 JSON 데이터를 서버로 제출하여 새로운 리소스를 생성하는 경우에 많이 사용됩니다.

### 1. `@PostMapping`의 역할.
- **HTTP POST 요청 처리**
    - `@PostMapping`은 POST 요청을 특정 URL에 매핑하여 처리합니다.
    - 주로 데이터 생성(create) 작업에서 사용되며, 서버로부터 데이터를 전송할 때 사용됩니다.

- **RESTful API에서 데이터 생성**
    - RESTful API에서 리소스를 생성하는 작업을 처리할 때 POST 요청이 사용됩니다.
    - 예를 들어, 사용자를 생성하거나 데이터베이스에 새로운 항목을 추가하는 등의 작업이 있을 수 있습니다.

#### 기본 사용법.

```java
@RestController
public class UserController {
    
    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        // 사용자 생성 로직 처리
        return userService.saveUser(user); // 생성된 사용자 객체 반환
    }
}
```

### 2. 주요 특징.
- **1. 요청 데이터 전송**
    - `@PostMapping`은 주로 요청 본문(Body)에 데이터를 포함하여 전송합니다.
    - 이는 GET 요청과 달리 URL에 데이터를 담지 않고, HTTP 요청의 Body에 데이터를 담아 서버로 전송합니다.

- **2. `@RequestBody`와 함께 사용**
    - 서버로 전달되는 JSON 또는 XML 데이터를 Java 객체로 변환하려면 `@RequestBody` 애너테이션과 함께 사용합니다.
    - `@RequestBody`는 요청 본문에 포함된 데이터를 Java 객체로 매핑하는 역할을 합니다.

- **3. 폼 데이터 처리**
    - HTML 폼을 통해 데이터를 전송할 때도 `@PostMapping`을 사용하여 폼 데이터를 처리할 수 있습니다.
    - 이때는 `@ModelAttribute` 또는 `@RequestParam`을 사용하여 폼 필드를 바인딩합니다.

### 3. 예시 1: JSON 데이터를 POST 요청으로 처리.
```java
@RestController
public class UserController {
    
    // POST 요청을 처리하며, 요청 본문을 Java 객체로 변환
    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        // user 객체는 클라이언트가 보낸 JSON 데이터로부터 매핑됨
        System.out.println("User created: " + user.getName());
        return userService.saveUser(user); // 새로운 사용자 객체 반환
    }
}
```

- `@PostMapping("/users)"`: `/users` URL로 들어오는 POST 요청을 처리합니다.
- `@RequestBody`: 요청 본문에 포함된 JSON 데이터를 `User` 객체로 변환합니다.
- 클라이언트는 아래와 같은 JSON 데이터를 서버로 보낼 수 있습니다.

```json
{
    "name": "Kobe",
    "age": 30
}
```

### 4. 예시 2: HTML 폼 데이터 처리.
```java
@Controller
public class UserController {
    
    // HTML 폼에서 제출된 데이터를 처리
    @PostMapping("/register")
    public String registerUser(@RequestParam String name, @RequestParam int age) {
        // 폼 데이터 처리 로직
        System.out.println("User name: " + name + ", age: " + age);
        return "user_registered"; // 성공 페이지로 이동
    }
}
```

- `@RequestParam`: 폼 필드에서 제출된 데이터를 메서드 파라미터로 바인딩합니다.

#### 클라이언트는 HTML 폼을 통해 데이터를 전송할 수 있습니다.
```html
<form action="/register" method="post">
  <input type="text" name="name" placeholder="Enter your name">
  <input type="number" name="age" placeholder="Enter your age">
  <button type="submit">Register</button>
</form>
```

### 5. `@PostMapping` 과 `@RequestMapping` 비교.

#### `@PostMapping`은 `@RequestMapping(method = RequestMethod.POST)`를 간단하게 대체할 수 있는 방법입니다.
```java
@RequestMapping(value = "/users", method = RequestMethod.POST)
public User createUser(@RequestBody User user) {
    return userService.saveUser(user);
}
```

#### 위 코드를 `@PostMapping`으로 리팩토링.
```java
@PostMapping("/users")
public User createUser(@RequestBody User user) {
    return userService.saveUser(user);
}
```

### 6. 요약.
- `@PostMapping`은 HTTP POST 요청을 처리하기 위한 애너테이션입니다.
- 주로 새로운 리소스를 생성하거나 서버로 데이터를 전송할 때 사용됩니다.
- JSON 데이터를 처리할 때는 `@RequestBody`와 함께 사용되며, 폼 데이터는 `@RequestParam` 또는 `@ModelAttribute`와 함께 처리할 수 있습니다.
- `@PostMapping`은 `@RequestMapping`의 간결한 대안으로 사용됩니다.

## 6️⃣ `@RequestBody` 애너테이션.
- **`@RequestBody` 애너테이션** 은 Spring Framework에서 HTTP 요청의 **본문(바디, Body)** 에 담긴 데이터를 Java 객체로 변환해주는 역할을 합니다.
- 주로 POST, PUT과 같은 요청에서 클라이언트가 JSON, XML 또는 다른 형식의 데이터를 전송할 때 이를 서버에서 처리할 수 있도록 도와줍니다.

### 1. `@RequestBody`의 주요 역할.
- **1. 요청 본문(Request Body)을 Java 객체로 변환**
    - `@RequestBody`는 클라이언트가 요청 본문에 담아 보낸 데이터를 Java 객체로 변환합니다.
    - 이때, Spring은 주로 Jackson 라이브러리를 사용하여 JSON 데이터를 Java 객체로 변환합니다.

- **2. 주로 POST, PUT 요청에서 사용**
    - `@RequestBody`는 POST는 PUT 요청에서 데이터를 서버로 전송할 때 많이 사용됩니다.
    - 예를 들어, 클라이언트가 새로운 리소스를 생성하거나 데이터를 업데이트할 때 JSON 데이터를 본문에 담아 전송할 수 있습니다.

- **3. 자동 역직렬화**
    - 클라이언트가 JSON 형식으로 데이터를 보내면, Spring은 이를 자동으로 Java 객체로 변환(역직렬화) 해줍니다.

### 2. `RequestBody`의 사용법.

#### 예시 1: JSON 데이터를 Java 객체로 변환.

클라이언트가 서버로 JSON 데이터를 전송하고, 서버가 이를 Java 객체로 변환하여 처리하는 예시입니다.
```java
@RestController
public class UserController {
    
    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        // 요청 본문에서 전송된 JSON 데이터를 User 객체로 변환
        System.out.println("User name: " + user.getName());
        System.out.println("User age: " + user.getAge());
        
        // User 객체를 저장하거나 처리한 후 반환
        return userService.saveUser(user);
    }
}
```

- `@PostMApping("/users")`: `/users` 경로로 들어오는 POST 요청을 처리합니다.
- `@RequestBody User user`: 요청 본문에 있는 JSON 데이터를 `User` 객체로 변환합니다.

#### 클라이언트가 전송하는 JSON 데이터 예시
```json
{
    "name": "Kobe",
    "age": 30
}
```

위 JSON 데이터를 서버로 보내면, `@RequestBody`가 이를 `User` 객체로 변환합니다.

#### `User` 클래스
```java
public class User {
    private String name;
    private int age;
    
    // 기본 생성자, getter, setter
    public User() {}
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        this.age = age;
    }
}
```

### 3. 예시 2: 요청 본문에 포함된 JSON 데이터 사용.
```java
@RestController
public class ProductController {
    
    @PutMapping("/products/{id}")
    public Product updateProduct(@PathVariable Long id, @RequestBody Product product) {
        // 요청 본문에서 Product 객체로 변환된 데이터를 사용해 업데이트 처리
        product.setId(id);
        return productService.updateProduct(product);
    }
}
```

- `@RequestBody`: JSON 데이터를 Java 객체인 `Product`로 변환합니다.
- `@PathVariable`: URL 경로에 포함된 값을 메서드 파라미터로 사용합니다. 위 코드에서는 제품 ID를 경로에서 추출합니다.

### 4. 요청 본문과의 관계.

HTTP 요청에서 요청 본문은 실제로 전송되는 데이터가 포함된 부분입니다.

#### 클라이언트가 서버로 데이터를 보내는 방식.
- **1. 요청 URL에 쿼리 파라미터로 데이터를 포함(GET 요청 등에서 사용)**
- **2. 요청 본문에 데이터를 포함(POST, PUT 요청에서 사용)**

`@RequestBody`는 두 번째 경우인 요청 본문에 데이터를 담아 보내는 요청에서 사용됩니다.

### 5. 데이터 형식.

`@RequestBody`는 JSON, XML 등 다양한 형식의 데이터를 처리할 수 있습니다.
기본적으로 Spring은 Jackson을 사용하여 JSON 데이터를 처리하지만, XML 등의 다른 형식도 지원됩니다.

- **JSON**
    - 대부분의 경우 JSON 형식의 데이터가 요청 본문에 담겨 전송되며, Spring은 이를 자동으로 Java 객체로 변환합니다.

- **XML**
    - 필요에 따라 XML 데이터를 사용할 수도 있으며, Spring에서 XML을 처리하기 위한 라이브러리를 추가하면 XML도 처리 가능합니다.

### 6. 추가 속성.

- **`required` 속성**
    - `@RequestBody` 는 기본적으로 요청 본문에 데이터가 반드시 포함되어야 합니다.
    - 하지만, `required = false`로 설정하면 요청 본문이 없어도 예외를 발생시키지 않습니다.

```java
@PostMapping("/users")
public User createUser(@RequestBody(required = false) User user) {
    if (user == null) {
        // 본문이 없을 경우 처리 로직
        return new User("Anonymous", 0);
    }
    return userService.saveUser(user);
}
```

### 7. 요약.
- **`@RequestBody`** 는 클라이언트가 보낸 HTTP 요청의 본문을 Java 객체로 변환하는 데 사용됩니다.
- 주로 POST, PUT 요청에서 JSON, XML 등의 데이터를 서버로 전송할 때 사용됩니다.
- Spring은 기본적으로 Jackson 라이브러리를 사용하여 JSON 데이터를 Java 객체로 변환합니다.
- 요청 본문에 포함된 데이터를 쉽게 처리할 수 있도록 도와주며, RESTful API 개발에 자주 사용됩니다.
