---
title: "📚[Backend Development] 🚀 환경 변수(Environment Variable) 치환 기능"
tags:
  - Backend Development
  - Server
  - Java
  - Security

date: "2025-12-11"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 🚀 GitHub에 비밀번호가 올라갔다면? 환경 변수로 해결하기

## 😱 이런 경험 있으신가요?

```yaml
# application.yml
spring:
  datasource:
    password: myRealPassword123!  # 😨 이거 그대로 GitHub에...?
```

코드를 작성하다 보면 **DB 비밀번호, API 키, 인증서 비밀번호** 등 민감한 정보를 설정 파일에 넣어야 할 때가 있습니다.

하지만 이대로 GitHub에 올리면? 🔥 **보안 사고 직행입니다.**

---

## 💡 해결책: 환경 변수(Environment Variable) 치환

Spring Boot는 `${환경변수명:기본값}` 문법으로 **환경 변수를 자동으로 치환**해줍니다.

### 작동 원리

```yaml
password: ${MY_SECRET_PASSWORD:defaultPassword}
```

1. **운영체제 환경변수 `MY_SECRET_PASSWORD`가 있으면** → 그 값 사용
2. **없으면** → 기본값 `defaultPassword` 사용

이렇게 하면:
- ✅ **소스코드에는 진짜 비밀번호가 없음**
- ✅ **GitHub에 올려도 안전**
- ✅ **서버마다 다른 비밀번호 사용 가능**

---

## 📝 Step 1: application.yml 설정

IRC 서버의 SSL 인증서 비밀번호를 환경 변수로 관리해봅시다.

```yaml
irc:
  ssl:
    keystore-path: classpath:keystone.p12
    keystore-password: ${IRC_KEYSTORE_PASSWORD:password}  # 👈 핵심!
    keystore-type: PKCS12
```

### 🔍 코드 설명
- `IRC_KEYSTORE_PASSWORD`: 운영체제 환경변수 이름
- `password`: 로컬 개발용 기본값 (운영 환경에서는 절대 사용 안 됨)

---

## 📝 Step 2: Java 코드 작성 (SslConfig.java)

환경 변수로 받은 값을 실제로 사용하는 코드입니다.

```java
package com.ircproject.config;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.io.Resource;
import org.springframework.core.io.ResourceLoader;

import javax.net.ssl.KeyManagerFactory;
import javax.net.ssl.SSLContext;
import javax.net.ssl.TrustManagerFactory;
import java.io.InputStream;
import java.security.KeyStore;

@Configuration
public class SslConfig {

    @Value("${irc.ssl.keystore-path}")
    private String keystorePath;

    @Value("${irc.ssl.keystore-password}")  // 👈 환경변수 자동 주입
    private String keystorePassword;

    @Value("${irc.ssl.keystore-type}")
    private String keystoreType;

    private final ResourceLoader resourceLoader;

    public SslConfig(ResourceLoader resourceLoader) {
        this.resourceLoader = resourceLoader;
    }

    @Bean
    public SSLContext sslContext() throws Exception {
        // 1. Keystore 로드 (JAR 내부 파일도 읽을 수 있도록 ResourceLoader 사용)
        KeyStore keyStore = KeyStore.getInstance(keystoreType);
        Resource resource = resourceLoader.getResource(keystorePath);

        try (InputStream stream = resource.getInputStream()) {
            keyStore.load(stream, keystorePassword.toCharArray());
        }

        // 2. KeyManager 초기화 (서버 신분증)
        KeyManagerFactory kmf = KeyManagerFactory.getInstance(
            KeyManagerFactory.getDefaultAlgorithm()
        );
        kmf.init(keyStore, keystorePassword.toCharArray());

        // 3. TrustManager 초기화 (클라이언트 검증)
        TrustManagerFactory tmf = TrustManagerFactory.getInstance(
            TrustManagerFactory.getDefaultAlgorithm()
        );
        tmf.init(keyStore);

        // 4. SSLContext 생성
        SSLContext sslContext = SSLContext.getInstance("TLS");
        sslContext.init(kmf.getKeyManagers(), tmf.getTrustManagers(), null);

        return sslContext;
    }
}
```

### 💡 왜 ResourceLoader를 사용하나요?

일반 `File` 클래스는 JAR로 패키징하면 작동하지 않습니다.  
`ResourceLoader`를 사용하면 **JAR 내부의 파일도 정상적으로 읽을 수 있습니다.**

---

## 🖥️ Step 3: 로컬에서 실행하기

### 방법 A: IntelliJ IDEA

1. **Run/Debug Configurations** 클릭
2. **Environment variables** 입력창에 추가:
   ```
   IRC_KEYSTORE_PASSWORD=mySecretPassword123
   ```
3. **Apply** → **Run**

![IntelliJ 환경변수 설정 예시]

### 방법 B: 터미널 (Mac/Linux)

```bash
# 환경변수 설정 후 실행
export IRC_KEYSTORE_PASSWORD=mySecretPassword123
./gradlew bootRun
```

또는 한 줄로:

```bash
# 일회성 환경변수 설정
IRC_KEYSTORE_PASSWORD=mySecretPassword123 ./gradlew bootRun
```

### 방법 C: 터미널 (Windows)

```cmd
set IRC_KEYSTORE_PASSWORD=mySecretPassword123
gradlew.bat bootRun
```

---

## ☁️ Step 4: AWS/운영 환경에서 실행하기

### AWS EC2 예시

```bash
# ~/.bashrc 또는 ~/.profile에 추가
export IRC_KEYSTORE_PASSWORD=운영환경_진짜_비밀번호

# 서버 실행
java -jar irc-server.jar
```

### Docker 예시

```dockerfile
# Dockerfile
FROM openjdk:17-jdk-slim
COPY build/libs/irc-server.jar app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

```bash
# 실행 시 환경변수 주입
docker run -e IRC_KEYSTORE_PASSWORD=운영비밀번호 my-irc-server
```

### Kubernetes 예시

```yaml
# deployment.yaml
apiVersion: v1
kind: Secret
metadata:
  name: irc-secrets
type: Opaque
stringData:
  keystorePassword: "운영비밀번호"
---
apiVersion: apps/v1
kind: Deployment
spec:
  template:
    spec:
      containers:
      - name: irc-server
        env:
        - name: IRC_KEYSTORE_PASSWORD
          valueFrom:
            secretKeyRef:
              name: irc-secrets
              key: keystorePassword
```

---

## ✅ 이렇게 하면 얻는 3가지 이점

| 항목 | 설명 |
|------|------|
| 🔒 **보안** | GitHub에 비밀번호가 절대 노출되지 않음 |
| 🛠️ **유지보수성** | 환경별로 다른 설정 쉽게 적용 가능 |
| 🚀 **배포 안정성** | JAR 패키징 시에도 정상 작동 |

---

## 💡 추가 팁

### 1️⃣ .gitignore에 인증서 파일 추가

```gitignore
# Security / Secrets
*.p12
*.jks
*.key
*.pem
src/main/resources/keystone.p12
```

### 2️⃣ 여러 환경변수 한번에 설정

```bash
# .env 파일 생성 (Git에는 올리지 말 것!)
IRC_KEYSTORE_PASSWORD=myPassword
DB_PASSWORD=dbPassword
API_KEY=myApiKey

# 환경변수 로드 후 실행
export $(cat .env | xargs) && ./gradlew bootRun
```

### 3️⃣ Spring Profile별로 다른 환경변수 사용

```yaml
# application-dev.yml
irc:
  ssl:
    keystore-password: ${IRC_KEYSTORE_PASSWORD:devPassword}

# application-prod.yml
irc:
  ssl:
    keystore-password: ${IRC_KEYSTORE_PASSWORD}  # 기본값 없음 (필수!)
```

---

## ⚠️ 주의사항

1. **기본값을 너무 쉽게 만들지 마세요**
   - `password`, `1234` 같은 기본값은 위험합니다
   - 개발 환경용으로만 사용하고, 운영에서는 반드시 환경변수 설정

2. **환경변수 이름은 명확하게**
   - ❌ `PASSWORD` (너무 일반적)
   - ✅ `IRC_KEYSTORE_PASSWORD` (명확함)

3. **민감한 정보는 절대 로그에 남기지 마세요**
   ```java
   // ❌ 위험
   log.info("Password: {}", keystorePassword);
   
   // ✅ 안전
   log.info("SSL Context initialized successfully");
   ```

---

## 🎯 마무리

환경 변수 치환 기능은 **필수 보안 패턴**입니다.

지금 당장 프로젝트의 `application.yml`을 확인해보세요.  
비밀번호가 하드코딩되어 있다면, 이 글을 참고해서 바로 수정하시길 바랍니다! 🚀

---
