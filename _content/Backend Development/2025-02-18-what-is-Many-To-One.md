---
title: "📚[Backend Development] @ManyToOne이란 무엇일까요?"
tags:
    - Backend Ddevelopment
date: "2025-02-18"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] @ManyToOne이란 무엇일까요?"
## 🍎 Intro.
- @ManyToOne은 **다대일(N:1) 관계**를 매핑할 때 사용합니다.
- 즉, **여러 개(Many)의 엔티티가 하나(One)의 엔티티를 참조**하는 구조입니다.

## ✅1️⃣ @ManyToOne 예제.
- 게시글(Article)과 댓글(Comment) 관계를 예로 들어보겠습니다.
- **하나의 게시글(Article)에 여러 개의 댓글(Comment)이 달릴 수 있습니다.**

### 1️⃣ Article 엔티티 (게시글)
```java
@Entity
public class Article {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String title;
    private String content;
    
    // Getter, Setter
}
```

### 2️⃣ Comment 엔티티 (댓글)
```java
@Entity
public class Comment {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String content;
    
    @ManyToOne
    @JoinColumn(name = "article_id") // 외래 키 컬럼명 설정
    private Article article;
    
    // Getter, Setter
}
```

## ✅2️⃣ @ManyToOne 설명.
- 1. @ManyToOne을 사용하여 **여러 개의 댓글(Comment)이 하나의 게시글(Article)을 참조**하도록 설정합니다.
- 2. @JoinColumn(name = "article_id")를 통해 **comment 테이블에 article_id 외래 키(FK)를 생성**합니다.

## ✅3️⃣ 데이터베이스 테이블 구조.
- 위 코드를 실행하면 데이터베이스는 다음과 같은 테이블이 생성됩니다.

#### 📌 article 테이블

|id|title|content|
| -------- | -------- | -------- |
|1|"Hello JPA"|"JPA 배우기"|
|2|"Spring Boot"|"Spring 공부"|

#### 📌 comment 테이블 (article_id FK 포함)

|id|content|article_id(FK)|
| -------- | -------- | -------- |
|1|"좋은 글이네요!"|1|
|2|"유익한 정보 감사합니다."|1|
|3|"Spring 최고!"|2|

- 📌 article_id 컬럼이 **게시글(Article)을 참조하는 외래 키(FK)입니다.**
    - 즉, **comment 테이블의 여러 행이 article_id를 통해 같은 article을 가리킬 수 있습니다.**

## ✅4️⃣ 데이터 조회
### ✅ 특정 게시글에 속한 댓글 가져오기.
- 게시글 ID(articleId)가 1번인 댓글을 가져오려면:

```java
List<Comment> comments = entityManager.createQuery(
        "SELECT c FROM c WHERE c.article.id = :articleId", Comment.class)
    .setParameter("articleId", 1L)
    .getResultList();
```
