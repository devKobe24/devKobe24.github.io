---
title: "📚[Backend Development] 계층형 댓글 목록 조회란 무엇일까요?"
tags:
    - Backend Ddevelopment
    - Database Design
    - API Design
    - Performance Optimization
date: "2025-02-08"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] 계층형 댓글 목록 조회란 무엇일까요?"
## 🍎 Intro.
- **계층형 댓글(Hierachical Commmeents)이란, 댓글과 대댓글(답글)을 계층적으로 표시하는 방식입니다.**
- 이는 일반적인 **1차원 목록 형태의 댓글 조회와 달리, 부모-자식 관계를 유지하는 댓글 시스템**입니다.

## ✅1️⃣ 계층형 댓글 목록 조회가 필요한 이유.
### ❌1️⃣ 일반적인 평면 댓글(flat comments) 방식의 한계.
- 일반적으로 **게시글에 대한 댓글을 단순 목록(List) 형태로 조회.**
    - 하지만 **답글(대댓글)이 많아질 경우, 계층 구조가 필요.**

### ✅2️⃣ 계층형 댓글의 장점.
- 댓글에 대한 **대댓글(답글)을 구조적으로 표현 가능.**
- **유저가 대화 흐름을 쉽게 파악할 수 있음.**
- **재귀적인 구조를 활용하여 대댓글이 몇 단계까지 달려도 정렬 가능.**

## ✅2️⃣ 계층형 댓글의 데이터 구조.
- 계층형 댓글을 저장하는 방법은 여러 가지가 있지만, 일반적으로 **"부모 댓글(parentId)"를** 저장하여 댓글 간 관계를 유지합니다.

### 📌1️⃣ 테이블 구조
```sql
CREATE TABLE comments (
    comment_id BIGINT AUTO_INCREMENT PRIMARY KEY,
    article_id BIGINT NOT NULl, -- 게시글 ID
    parent_id BIGINT NULL,      -- 부모 댓글 ID (NULL이면 최상위 댓글)
    author VARCHAR(255) NOT NULL,
    content TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (parentt_id) REFERENCES comments(comment_id) ON DELETE CASCADE
);
```

### ✅ 주요 컬럼.
- **comment_id ➞** 댓글의 고유 ID.
- **article_id ➞** 해당 댓글이 속한 게시글 ID.
- **parent_id ➞** 부모 댓글 ID (최상위 댓글이면 NULL).
- **author ➞** 댓글 작성자.
- **content ➞** 댓글 내용.
- **created_at ➞** 댓글 작성 시간.

## ✅3️⃣ 계층형 댓글 목록 조회 API 구현 (Spring Boot + JPA)
### 🛠️1️⃣ Entity (계층형 구조)
```java
import jakarta.persistence.*;
import lombok.Getter;
import lombok.NoArgsConstructor;

import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

@Getter
@NoArgsConstructor
@Entity
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
    private Comment parent;  // 부모 댓글

    @OneToMany(mappedBy = "parent", cascade = CascadeType.ALL)
    private List<Comment> replies = new ArrayList<>(); // 대댓글 리스트

    private String author;
    private String content;
    private LocalDateTime createdAt = LocalDateTime.now();
}
```

- ✅ **부모 댓글(parent)과 대댓글(replies) 관계를 유지하여 계층 구조 형성.**

### 🛠️2️⃣ Repository (계층형 댓글 조회)
```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public interface CommentRepository extends JpaRepository<Comment, Long> {
    List<Comment> findByArticle_ArticleIdAndParentIsNullOrderByCreatedAtDesc(Long articleId);
}
```

- ✅ **최상위 댓글(부모가 없는 댓글)만 가져옴 ➞** 이후 replies 필드에서 대댓글을 가져옴.

### 🛠️3️⃣ Service (계층형 댓글 로직 구현)
```java
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
@RequireArgsConstructor
public class CommentService {
    private final CommentService {
        private final CommentRepository commentRepository;
        
        public List<CommentResponse> getCommentsByArticle(Long articleId) {
            List<Comment> comments = commentRepository.findByArticle_ArticleIdAndParentIsNullOrderByCreatedAtDesc(articleId);
            return comments.stream().map(CommentResponse::fromEntity).toList();
        }
    }
}
```

- ✅ **최상위 댓글만 조회하고, 대댓글은 replies 필드를 통해 재귀적으로 가져옴.**

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
}
```

- ✅ **RESTful API 형식으로 GET /api/articles/{articleId}comments 요청을 처리.**

## ✅4️⃣ 계층형 댓글 조회 API의 응답 예시
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
        "createdAt": "2025-02-01T12:05:00Z",
        "replies": []
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

- ✅ **replies 필드를 이용해 대댓글을 계층적으로 표현.**

## ✅5️⃣ 계층형 댓글 조회의 SQL 방식.
### 📌1️⃣ 재귀 쿼리(CTE)
```sql
WITH RECURSIVE comment_tree AS (
    SELECT comment_id, parent_id, content, author, created_at
    FROM comments
    WHERE article_id = 123 AND parent_id IS NULL
    UNION ALL
    SELECT c.comment_id, c.parent_id, c.content, c.author, c.created_at
    FROM comments c
    INNER JOIN comment_tree ct ON c.parent_id = ct.comment_id
)
SELECT * FROM comment_tree ORDER BY created_at;
```

- ✅ **재귀적으로 부모-자식 관계를 조회하여 계층적 데이터를 정렬.**

## ✅6️⃣ 계층형 댓글 조회 API의 성능 최적화.
### 1️⃣ JPA의 @BatchSize 또는 JOIN FETCH 활용 ➞ N + 1 문제 해결.
```java
@Query("SELECT c FROM Comment c JOIN FETCH c c.replies WHERE c.article.articleId = :articleId")
List<Comment> findCommentsWithReplies(@Param("articleId") Long articleId);
```

### 2️⃣ Redis 캐싱 적용 ➞ 자주 조회되는 댓글 목록을 캐싱하여 성능 향상 가능.
## ✅7️⃣ 계층형 댓글 조회 API의 활용 사례.

| 서비스 | 설명                                                      |
| ------ | --------------------------------------------------------- |
|**블로그**|블로그 게시글의 계층형 댓글 (ex: 네이버 블로그, 티스토리)|
|**커뮤니티**|게시판 댓글 + 대댓글 (ex: 디시인사이드, Reddit)|
|**SNS**|소셜 미디어 댓글 (ex: 페이스북, 인스타그램)|
|**이커머스**|상품 리뷰 대댓글 (ex: 아마존, 쿠팡)|

## ✅8️⃣ 결론.
- **계층형 댓글 목록 조회는 댓글과 대댓글(답글)을 계층적으로 표시하는 방식.**
- **부모 댓글(parentId)을 저장하여 관계를 유지.**
- **Spring Boot + JPA 기반으로 쉽게 구현 가능.**
- **재귀 쿼리(CTE) 또는 JOIN FETCH를 활용하여 최적화 가능.**
