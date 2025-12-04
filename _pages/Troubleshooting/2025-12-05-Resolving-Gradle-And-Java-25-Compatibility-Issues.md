---
title: "🔍 [Troubleshooting] 🚀 Gradle & Java 25 호환성 문제 해결하기"
tags:
  - Troubleshooting
  - Backend Development
  - Gradle
  - JVM
date: "2025-12-05"
thumbnail: "/assets/img/thumbnail/troubleshooting.jpg"
---

# 🚨 문제 상황

프로젝트 생성 직후, Gradle 빌드 실행 시 다음과 같은 오류가 발생했습니다.
```shell
FAILURE: Build failed with an exception.

* What went wrong:
BUG! exception in phase 'semantic analysis' in source unit '_BuildScript_' 
Unsupported class file major version 69

Your build is currently configured to use incompatible Java 25 and Gradle 8.14.3. 
Cannot sync the project.
The maximum compatible Gradle JVM version is 24.
```

---

## 🔎 원인 분석

### 핵심 오류 메시지 해석

| 오류 메시지 | 의미 |
|-----------|------|
| `Unsupported class file major version 69` | Java 25의 클래스 파일 버전 |
| `The maximum compatible Gradle JVM version is 24` | Gradle 8.14.3은 최대 Java 24까지만 지원 |

### 문제의 본질

**Gradle을 실행하는 JVM 버전이 Gradle이 지원하는 최대 버전을 초과**했기 때문입니다.

- ❌ 현재 설정: Java 25로 Gradle 실행 시도
- ✅ Gradle 8.14.3 지원 범위: Java 24 이하

> 💡 **참고**: Java의 빠른 릴리스 주기(6개월마다 새 버전)로 인해, 빌드 도구들은 최신 버전을 즉시 지원하지 않는 경우가 많습니다.

---

## ✅ 해결 방법

### 📍 IntelliJ IDEA 설정 변경 (macOS 기준)

#### **Step 1️⃣ 설정 메뉴 열기**

- 단축키: `⌘` + `,` (Command + Comma)
- 또는 메뉴: **IntelliJ IDEA > Preferences**

#### **Step 2️⃣ Gradle 설정으로 이동**
```
Build, Execution, Deployment 
  └─ Build Tools 
      └─ Gradle
```

#### **Step 3️⃣ Gradle JVM 버전 변경**

1. 화면 하단의 **Gradle JVM** 드롭다운 찾기
2. 현재 `Java 25` 또는 `Project SDK 25`로 되어 있는 것을 확인
3. **Java 21 (LTS)** 또는 **Java 17 (LTS)**로 변경

**🎯 권장 버전 선택 가이드**

| Java 버전 | 권장 여부 | 사유 |
|----------|---------|------|
| **Java 21 (LTS)** | ⭐️ 최우선 권장 | 최신 LTS, Spring Boot 3.x 최적화 |
| **Java 17 (LTS)** | ✅ 권장 | Spring Boot 3.x 최소 요구사항 |
| Java 25 | ❌ 비권장 | Non-LTS, 안정성 미검증 |

**💾 JDK가 없는 경우**

드롭다운 메뉴에서 `Download JDK...` 선택 후:
- ✅ Eclipse Temurin 21
- ✅ Amazon Corretto 21

#### **Step 4️⃣ 변경사항 적용**

1. `Apply` → `OK` 클릭
2. **Gradle 프로젝트 새로고침**
   - Gradle 탭의 🔄 아이콘 클릭
   - 또는 터미널에서 `./gradlew clean build` 실행

---

## 🎓 추가 설명

### Class File Major Version 이해하기

| Java 버전 | Class File Major Version |
|----------|-------------------------|
| Java 17 | 61 |
| Java 21 | 65 |
| Java 24 | 68 |
| **Java 25** | **69** |

### 왜 LTS 버전을 사용해야 할까요?

**LTS(Long Term Support) 버전의 장점:**

- ✅ **장기 보안 업데이트 제공** (3년 이상)
- ✅ **프레임워크 및 라이브러리 우선 지원**
- ✅ **프로덕션 환경 안정성 보장**
- ✅ **커뮤니티 레퍼런스 풍부**

**현재 권장 조합:**
```
Spring Boot 3.x + Java 21 (LTS)
```

---

## 🛠️ 트러블슈팅 체크리스트

해결이 안 되는 경우 아래 항목들을 확인해보세요:

- [ ] IntelliJ IDEA를 재시작했나요?
- [ ] `~/.gradle/` 폴더의 캐시를 삭제했나요?
- [ ] `JAVA_HOME` 환경변수가 올바르게 설정되어 있나요?
- [ ] 프로젝트의 `build.gradle`에서 `sourceCompatibility`가 올바르게 설정되어 있나요?

### 환경변수 확인 (macOS/Linux)
```bash
# 현재 JAVA_HOME 확인
echo $JAVA_HOME

# 설치된 Java 버전 확인
java -version

# 사용 가능한 Java 버전 목록 (macOS)
/usr/libexec/java_home -V
```

---

## 📚 참고 자료

- [Gradle Compatibility Matrix](https://docs.gradle.org/current/userguide/compatibility.html)
- [Spring Boot System Requirements](https://docs.spring.io/spring-boot/system-requirements.html)
- [Java SE Support Roadmap](https://www.oracle.com/java/technologies/java-se-support-roadmap.html)

---

> 💡 **Tip**: 새 프로젝트를 시작할 때는 항상 최신 LTS 버전의 Java를 사용하고, IDE의 프로젝트 설정과 Gradle JVM 설정이 일치하는지 확인하는 습관을 들이면 이런 문제를 예방할 수 있습니다!
