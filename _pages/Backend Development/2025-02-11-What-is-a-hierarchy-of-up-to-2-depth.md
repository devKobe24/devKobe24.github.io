---
title: "📚[Backend Development] 최대 2 Depth의 계층형 대댓글이란?"
tags:
    - Backend Ddevelopment
    - Database Design
    - API Design
    - Performance Optimization
date: "2025-02-11"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 최대 2 Depth의 계층형 대댓글이란?"
## 🍎 Intro.
- **댓글(Parent) ➞ 대댓굴(Child)까지만 허용하며, 대댓글의 대댓글(Child of Child)은 허용하지 않는 구조입니다.**
    - 즉, **댓글의 깊이가 최대 2단계까지만 유지 되며,**
        - **1 Depth (최상위 댓글)**
        - **2 Depth (대댓글)**
            - 이후에는 **더 이상 하위 대댓글을 추가할 수 없는 방식**입니다.

## ✅1️⃣ 최대 2 Depth 계층형 대댓글이 필요한 이유
### ❌1️⃣ 일반적인 계층형 댓글 방식의 문제점.
- 댓글이 무한히 중첩될 경우, **데이터 조회 및 정렬이 복잡해지고 성능 저하 가능성.**
- UI에서 **너무 깊은 계층 구조는 사용자 경험(UX)에 좋지 않음.**
- **무한 재귀 호출 방지를 위해 계층을 제한하는 것이 일반적.**

### ✅2️⃣ 최대 2 Depth 계층형 대댓글의 장점.
- **UI/UX 개선 ➞** 대댓글이 많아도 가독성이 유지됨.
- **SQL 성능 최적화 가능 ➞** 복잡한 재귀 쿼리 없이 간단한 JOIN으로 해결 가능.
- **프론트엔드에서 구현이 쉬움 ➞** 2 Depth까지만 유지하므로 댓글 정렬이 단순함.

## ✅2️⃣ 최대 2 Depth의 계층형 대댓글 테이블 구조.

```sql
CREATE TABLE comments (
    comment_id BIGINT AUTO_INCREMENT PRIMARY KEY,
    article_id BIGINT NOT NULL,    -- 게시글 ID
    parent_id BIGINT NULL,        -- 부모 댓글 ID (NULL이면 최상위 댓글)
    depth INT NOT NULL DEFAULT 1,    -- 댓글의 깊이 (1: 일반 댓글, 2: 대댓글)
    author VARCHAR(255) NOT NULL,
    content TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (parent_id) REFERENCES comments(comment_id) ON DELETE CASCADE
);
```

### ✅ 테이블 주요 컬럼
- **comment_id ➞** 댓글의 고유 ID.
- **article_id ➞** 댓글이 속한 게시글 ID.
- **parent_id ➞** 부모 댓글 ID (NULL이면 최상위 댓글).
- **depth ➞** 댓글의 깊이 (1이면 일반 댓글, 2이면 대댓글).
- **author  ➞** 작성자.
- **content ➞** 댓글 내용.
- **created_at ➞** 댓글 작성 시간.

## ✅3️⃣ 최대 2 Depth 계층형 대댓글의 규칙.

|**Depth**|**설명**|
| -------- | -------- |
|**1 Depth**|일반 댓글 (Parent)|
|**2 Depth**|대댓글 (Child)|
|**3 Depth 이상**|❌ 허용하지 않음|

## ✅4️⃣ 최대 2 Depth의 계층형 대댓글 목록 조회 API 구현.
### 🛠️1️⃣ Entity (JPA 기반 계층형 댓글)
```java
import jakarta.persistence.*;
import lombok.Getter;
import lombok.NoArgsConstructor;
import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

@Getter
@NoArgsConstructor
@Table(name = "comments")
public class Comment {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long commentId;
    
    @ManyToOne
    @JoinColumn(name = "article_id", nullable = false)
    private Article article;
    
    @ManyToOne
    @JoinColumn(name = "parent_id")
    private Comment parent; // 부모 댓글
    
    @OneToMany(mappedBy = "parent", cascade = CascadeType.ALL)
    private List<Comment> replies = new ArrayList<>(); // 대댓글 리스트
    
    private Integer depth; // 댓글 깊이 (1: 일반 댓글, 2: 대댓글)
    private String author;
    private String content;
    private LocalDateTime createdAt = LocalDateTime.now();
}
```

- ✅ **댓글의 깊이를 depth 필드로 저장하여 최대 2 Depth까지만 허용.**

### 🛠️2️⃣ Repository (2 Depth까지 댓글 조회)
```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.List;

@Repository
public interface CommentRepository extends JpaRepository<Comment, Long> {
    List<Comment> findByArticle_ArticleIdDepthOrderByCreatedAtAsc(Long articleId, Integer depth);
}
```

- ✅ **최대 depth = 2까지만 조회하도록 설정.**

### 🛠️3️⃣ Service (비즈니스 로직)
```java
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
@RequiredArgsConstructor
public class CommentService {
    private final CommentRepository commentRepository;
    
    public List<CommentResponse> getCommentsByArticle(Long articleId) {
        return commentRepository.findByArticle_ArticleIdAndDepthOrderByCreatedAtAsc(articleId, 1)
            .stream()
            .map(CommentResponse::fromEntity)
            .toList();
    }
    
    public List<CommentResponse> getRepliesByComment(Long parentId) {
        return commentRepository.findByArticle_ArticleIdAndDepthOrderByCreatedAtAsc(parentId, 2)
            .stream()
            .map(CommentResponse::fromEntity)
            .toList();
    }
}
```

- ✅ **최대 2 Depth까지만 조회하는 API 로직 구현.**

### 🛠️4️⃣ Controller (API 요청 처리)
```java
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/articles/{articleId}/comments")
@RequiredArgsConstructor
public class CommentController {
    private final CommentService commentService;
    
    @GetMapping
    public ResponseEntity<List<CommentResponse>> getComments(@PathVariable Long articleId) {
        return ResponseEntity.ok(commentService.getCommentsByArticle(articleId));
    }
    
    @GetMapping("/{commentId}/replies")
    public RespinseEntity<List<CommentResponse>> getReplies(@PathVariable Long commentId) {
        return ResponseEntity.ok(commentService.getRepliesByComment(commentId));
    }
}
```

- ✅ **RESTful API로 최대 2 Depth까지 댓글 및 대댓글 조회 가능.**

## ✅5️⃣ API 응답 예시.
```json
[
  {
    "commentId": 1,
    "author": "John Doe",
    "content": "좋은 글이네요!",
    "createdAt": "2025-02-01T12:00:00Z",
    "replies": [
      {
        "commentId": 2,
        "author": "Alice",
        "content": "저도 그렇게 생각해요!",
        "createdAt": "2025-02-01T12:05:00Z"
      }
    ]
  },
  {
    "commentId": 3,
    "author": "Bob",
    "content": "궁금한 점이 있어요!",
    "createdAt": "2025-02-01T12:10:00Z",
    "replies": []
  }
]
```

- ✅ **최대 2 Depth까지만 유지되며, replies 필드를 이용하여 대댓글을 표현.**

## ✅6️⃣ SQL 쿼리 방식.
```sql
-- 최상위 댓글(1 Depth) 조회
SELECT * FROM comments WHERE article_id = 123 AND depth = 1 ORDER BY created_at ASC;

-- 특정 댓글(2 Depth)의 대댓글 조회
SELECT * FROM comments WHERE parent_id = 1 AND depth = 2 ORDER BY created_at ASC;
```

- ✅ **SQL을 활용하여 2 Depth까지만 조회 가능하도록 제한.**

## ✅7️⃣ 최대 2 Depth 계층형 대댓글의 활용 사례

| 서비스   | 설명                                             |
| -------- | ------------------------------------------------ |
| 블로그   | 블로그 게시글의 댓글 + 대댓글(ex: 네이버 블로그) |
| 커뮤니티 | 게시판 댓글 + 대댓글 (ex: Reddit)|
|SNS|SNS 댓글 + 대댓글 (ex: 인스타그램, 트위터)|
|이커머스|상품 리뷰의 대댓글 (ex: 아마존)|

## ✅8️⃣ 결론.
- **최대 2 Depth의 계층형 대댓글은 댓글 ➞ 대댓글까지만 허용하고, 더 이상 하위 댓글을 허용하지 않는 방식.**
- **데이터 정렬이 단순해지고 성능 최적화 가능.**
- **Spring Boot + JPA 기반으로 쉽게 구현 가능.**
- **대규모 트래픽에서도 적절한 성능을 유지하면서 안정적인 댓글 시스템 제공 가능.**
