---
title: "🔍[Troubleshooting] 🚀 Spring Boot 페이지네이션"
tags:
    - Troubleshooting
    - Backend Development
    - Spring Boot
    - 페이지네이션
    - Pagination
date: "2025-09-16"
thumbnail: "/assets/img/thumbnail/troubleshooting.jpg"
---

# 🚀 Spring Boot 페이지네이션 Troubleshooting

## 📋 목차
1. [문제 상황](#문제-상황)
2. [원인 분석](#원인-분석)
3. [해결 방안](#해결-방안)
4. [코드 예시](#코드-예시)
5. [베스트 프랙티스](#베스트-프랙티스)

---

## 🚨 문제 상황

**증상**: 게시글이 존재함에도 불구하고 페이지네이션 UI가 화면에 표시되지 않음

**환경**: 
- Spring Boot + JPA
- Thymeleaf 템플릿 엔진
- `@PageableDefault` 어노테이션 사용

---

## 🔍 원인 분석

### 1. Thymeleaf 조건문 확인

```html
<ul class="pagination" th:if="${posts.totalPages > 1}">
    <!-- 페이지네이션 UI -->
</ul>
```

**분석**: `th:if="${posts.totalPages > 1}"` 조건문이 핵심
- **전체 페이지 수가 1을 초과**할 때만 페이지네이션 UI 표시
- UX 관점에서 올바른 처리 방식 (페이지가 1개뿐이면 불필요)

### 2. Controller 설정 확인

```java
@GetMapping
public String index(Model model, 
    @PageableDefault(size = 5, sort = "id", direction = Sort.Direction.DESC) 
    Pageable pageable) {
    
    Page<PostResponseDto> posts = postService.findAll(pageable);
    model.addAttribute("posts", posts);
    return "index";
}
```

**분석**: `@PageableDefault(size = 5)` 설정
- 한 페이지당 **5개의 게시글** 표시
- 총 게시글이 5개 이하 → `totalPages = 1` → 페이지네이션 숨김

### 3. 데이터 상태 확인

| 총 게시글 수 | 페이지 크기 | 총 페이지 수 | 페이지네이션 표시 |
|-------------|------------|-------------|------------------|
| 1-5개       | 5          | 1           | ❌ 숨김          |
| 6-10개      | 5          | 2           | ✅ 표시          |
| 11-15개     | 5          | 3           | ✅ 표시          |

---

## ✅ 해결 방안

### 방법 1: 데이터 추가 (권장)

**실제 운영 환경에 적합한 방법**

```java
// 테스트 데이터 추가 (예시)
@PostConstruct
public void initTestData() {
    for (int i = 1; i <= 10; i++) {
        Post post = Post.builder()
            .title("테스트 게시글 " + i)
            .content("테스트 내용 " + i)
            .author("작성자" + i)
            .build();
        postRepository.save(post);
    }
}
```

**장점**:
- 실제 사용 환경과 동일
- 다양한 페이지네이션 시나리오 테스트 가능

### 방법 2: 페이지 크기 조절 (개발/테스트용)

**개발 단계에서 빠른 확인용**

#### Before
```java
@PageableDefault(size = 5, sort = "id", direction = Sort.Direction.DESC)
```

#### After
```java
@PageableDefault(size = 2, sort = "id", direction = Sort.Direction.DESC)
```

**장점**:
- 코드 수정만으로 빠른 확인 가능
- 적은 데이터로도 페이지네이션 테스트

**주의사항**: 운영 배포 시 원래 값으로 복구 필요

---

## 💻 코드 예시

### 1. Controller 완전한 예시

```java
@Controller
@RequestMapping("/posts")
@RequiredArgsConstructor
public class PostController {
    
    private final PostService postService;
    
    @GetMapping
    public String index(
        Model model,
        @PageableDefault(size = 5, sort = "id", direction = Sort.Direction.DESC) 
        Pageable pageable) {
        
        Page<PostResponseDto> posts = postService.findAll(pageable);
        
        // 디버깅용 로그
        log.info("총 게시글 수: {}", posts.getTotalElements());
        log.info("총 페이지 수: {}", posts.getTotalPages());
        log.info("현재 페이지: {}", posts.getNumber() + 1);
        
        model.addAttribute("posts", posts);
        return "index";
    }
}
```

### 2. Thymeleaf 템플릿 개선

```html
<!-- 디버깅 정보 표시 (개발 시에만) -->
<div th:if="${#profiles.active == 'dev'}" class="debug-info">
    <p>총 게시글: <span th:text="${posts.totalElements}"></span></p>
    <p>총 페이지: <span th:text="${posts.totalPages}"></span></p>
    <p>현재 페이지: <span th:text="${posts.number + 1}"></span></p>
</div>

<!-- 게시글 목록 -->
<div class="post-list">
    <div th:each="post : ${posts.content}" class="post-item">
        <h3 th:text="${post.title}"></h3>
        <p th:text="${post.content}"></p>
    </div>
</div>

<!-- 페이지네이션 -->
<nav th:if="${posts.totalPages > 1}" aria-label="페이지 네비게이션">
    <ul class="pagination justify-content-center">
        <!-- 이전 페이지 -->
        <li class="page-item" th:classappend="${posts.first} ? 'disabled'">
            <a class="page-link" 
               th:href="@{/posts(page=${posts.number - 1})}"
               th:unless="${posts.first}">이전</a>
            <span class="page-link" th:if="${posts.first}">이전</span>
        </li>
        
        <!-- 페이지 번호 -->
        <li class="page-item" 
            th:each="pageNum : ${#numbers.sequence(0, posts.totalPages - 1)}"
            th:classappend="${pageNum == posts.number} ? 'active'">
            <a class="page-link" 
               th:href="@{/posts(page=${pageNum})}"
               th:text="${pageNum + 1}"
               th:unless="${pageNum == posts.number}"></a>
            <span class="page-link" 
                  th:if="${pageNum == posts.number}"
                  th:text="${pageNum + 1}"></span>
        </li>
        
        <!-- 다음 페이지 -->
        <li class="page-item" th:classappend="${posts.last} ? 'disabled'">
            <a class="page-link" 
               th:href="@{/posts(page=${posts.number + 1})}"
               th:unless="${posts.last}">다음</a>
            <span class="page-link" th:if="${posts.last}">다음</span>
        </li>
    </ul>
</nav>

<!-- 페이지네이션이 없을 때 안내 메시지 -->
<div th:if="${posts.totalPages <= 1}" class="text-center text-muted">
    <p>전체 <span th:text="${posts.totalElements}"></span>개의 게시글</p>
</div>
```

### 3. Service Layer 예시

```java
@Service
@RequiredArgsConstructor
@Transactional(readOnly = true)
public class PostService {
    
    private final PostRepository postRepository;
    
    public Page<PostResponseDto> findAll(Pageable pageable) {
        Page<Post> posts = postRepository.findAll(pageable);
        
        // Entity → DTO 변환
        return posts.map(post -> PostResponseDto.builder()
            .id(post.getId())
            .title(post.getTitle())
            .content(post.getContent())
            .author(post.getAuthor())
            .createdAt(post.getCreatedAt())
            .build());
    }
}
```

---

## 💡 베스트 프랙티스

### 1. 환경별 설정 분리

```yaml
# application-dev.yml (개발환경)
spring:
  data:
    web:
      pageable:
        default-page-size: 2  # 개발 시 작은 값으로 테스트

# application-prod.yml (운영환경)
spring:
  data:
    web:
      pageable:
        default-page-size: 10  # 운영 시 적절한 값
```

### 2. 커스텀 Pageable Configuration

```java
@Configuration
public class PaginationConfig {
    
    @Bean
    @Primary
    public PageableHandlerMethodArgumentResolver pageableResolver() {
        PageableHandlerMethodArgumentResolver resolver = 
            new PageableHandlerMethodArgumentResolver();
        resolver.setMaxPageSize(100);  // 최대 페이지 크기 제한
        resolver.setOneIndexedParameters(true);  // 1부터 시작하는 페이지 번호
        return resolver;
    }
}
```

### 3. 디버깅을 위한 로깅

```java
@Slf4j
@Service
public class PostService {
    
    public Page<PostResponseDto> findAll(Pageable pageable) {
        log.debug("페이지 요청 - 페이지: {}, 크기: {}, 정렬: {}", 
            pageable.getPageNumber(), 
            pageable.getPageSize(), 
            pageable.getSort());
            
        Page<Post> posts = postRepository.findAll(pageable);
        
        log.debug("페이지 결과 - 총 요소: {}, 총 페이지: {}, 현재 페이지 요소 수: {}", 
            posts.getTotalElements(), 
            posts.getTotalPages(), 
            posts.getNumberOfElements());
            
        return posts.map(this::convertToDto);
    }
}
```

### 4. 테스트 코드 작성

```java
@SpringBootTest
@Transactional
class PostControllerTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @Autowired
    private PostRepository postRepository;
    
    @Test
    @DisplayName("게시글이 5개 이하일 때 페이지네이션이 표시되지 않음")
    void pagination_not_shown_when_posts_less_than_page_size() throws Exception {
        // Given: 3개의 게시글 생성
        createTestPosts(3);
        
        // When & Then
        mockMvc.perform(get("/posts"))
            .andExpect(status().isOk())
            .andExpect(model().attributeExists("posts"))
            .andExpect(xpath("//ul[@class='pagination']").doesNotExist());
    }
    
    @Test
    @DisplayName("게시글이 페이지 크기를 초과할 때 페이지네이션 표시됨")
    void pagination_shown_when_posts_exceed_page_size() throws Exception {
        // Given: 7개의 게시글 생성 (페이지 크기 5 초과)
        createTestPosts(7);
        
        // When & Then
        mockMvc.perform(get("/posts"))
            .andExpect(status().isOk())
            .andExpect(model().attributeExists("posts"))
            .andExpect(xpath("//ul[@class='pagination']").exists());
    }
    
    private void createTestPosts(int count) {
        for (int i = 1; i <= count; i++) {
            Post post = Post.builder()
                .title("테스트 게시글 " + i)
                .content("테스트 내용 " + i)
                .build();
            postRepository.save(post);
        }
    }
}
```

---

## 📝 요약

### ✅ 정상 동작 확인
현재 코드는 **정상적으로 동작**하고 있습니다. 페이지네이션이 보이지 않는 것은 설계된 동작입니다.

### 🎯 해결 체크리스트
- [ ] 총 게시글 수 확인 (페이지 크기보다 많은가?)
- [ ] `@PageableDefault` 설정 확인
- [ ] Thymeleaf 조건문 `th:if="${posts.totalPages > 1}"` 확인
- [ ] 로그를 통한 페이지 정보 디버깅
- [ ] 테스트 데이터 추가 또는 페이지 크기 조절

### 🚀 운영 고려사항
- **개발 환경**: 페이지 크기를 작게 설정하여 테스트 용이성 확보
- **운영 환경**: 적절한 페이지 크기로 사용자 경험 최적화
- **성능**: 페이지 크기가 너무 크면 성능 저하 가능성 있음
