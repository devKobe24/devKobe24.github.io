---
title: "📚[Backend Development] @OneToMany란 무엇일까요?"
tags:
    - Backend Ddevelopment
date: "2025-02-18"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] @OneToMany란 무엇일까요?"
## 🍎 Intro.
- @OneToMany는 **일대다(1:N) 관계**를 매핑할 때 사용하는 어노테이션입니다.
- 즉, **하나(One)의 엔티티가 여러 개(Many)의 엔티티를 참조**하는 구조입니다.

## ✅1️⃣ @OneToMany 예제.
- 게시글(Article)과 댓글(Comment) 관계를 예로 들어보겠습니다.
- **하나의 게시글(Article)에는 여러 개의 댓글(Comment)이 달릴 수 있습니다.**

### 1️⃣ Article 엔티티(게시글)
```java
@Entity
public class Article {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String title;
    private String content;
    
    @OneToMany(mappedBy = "article") // Comment 엔티티의 article 필드가 관계의 주인
    private List<Comment> comments = new ArrayList<>();
    
    // Getter, Setter
}
```

### 2️⃣ Comment 엔티티(댓글)
```java
@Entity
public class Comment {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String content;
    
    @ManyToOne
    @JoinColumn(name = "article_id") // comment 테이블에 article_id FK 생성
    private Article article;
    
    // Getter, Setter
}
```

## ✅2️⃣ @OneToMany(mappedBy = "article")의 의미
- Comment 엔티티의 article 필드를 참조하여 **양방향 관계를 설정**합니다.
- **외래 키를 관리하는 주인은 Comment.article 필드**이며, Article 엔티티는 mappedBy를 통해 **읽기 전용**입니다.
- 즉, **Comment 엔티티가 관계의 주인이고, Article 엔티티에서는 직접 FK를 관리하지 않습니다.** (➞ @JoinColumn이 Comment 쪽에만 있는 이유)

## ✅3️⃣ 데이터베이스 테이블 구조.
- 위 코드를 실행하면 다음과 같은 테이블이 생성됩니다.

### 📌 article 테이블 (게시글)

|id|title|content|
| -------- | -------- | -------- |
|1|"Hello JPA"|"JPA 배우기"|
|2|"Spring Boot"|"Spring 공부"|

### 📌 comment 테이블 (게시글에 연결된 댓글, article_id FK 포함)

|id|content|article_id(FK)|
| -------- | -------- | -------- |
|1|"좋은 글이네요!"|1|
|2|"유익한 정보 감사합니다."|1|
|3|"Spring 최고!"|2|

- 📌 article_id 컬럼이 **게시글(Article)을 참조하는 외래 키(FK)입니다.**
    - 즉, **하나의 Article에는 여러 개의 Comment가 연결될 수 있습니다.**

## ✅4️⃣ 데이터 조회.
### ✅ 특정 게시글에 속한 댓글 가져오기.
- 양방향 관계가 설정되어 있으므로, 특정 게시글에 달린 댓글을 쉽게 가져올 수 있습니다.

```java
Article article = entityManager.find(Article.class, 1L);
List<Comment> comments = article.getComments(); // 해당 게시글의 모든 댓글 가져오기
```

## 🚀5️⃣ 단방향 @OneToMany vs 양방향 @OneToMany
### 1️⃣ 단방향 @OneToMany
```java
@Entity
public class Article {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String title;
    private String content;
    
    @OneToMany
    @JoinColumn(name = "article_id") // FK를 직접 관리 (주인 역할)
    private List<Comment> comments = new ArrayList<>();
    
    // Getter, Setter
}
```

### ✅ 장점.
- 단순한 구조.
- 불필요한 mappedBy 없이 @JoinColumn을 통해 FK 직접 관리 가능

### ❌ 단점.
- **데이터 삽입 시 추가적인 SQL 실행 발생**
- @OneToMany 단방향 관계에서 @JoinColumn을 사용하면 **INSERT 쿼리가 두 번 실행됨** (➞ 댓글 삽입 후, 게시글 ID 업데이트)

### 2️⃣ 양방향 @OneToMany + @ManyToOne
```java
@Entity
public class Article {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String title;
    private String content;
    
    @OneToMany(mappedBy = "article") // Comment의 article 필드를 주인으로 설정
    private List<Comment> comments = new ArrayList<>();
    
    // Getter, Setter
}
```

### ✅ 장점.
- **성능 최적화 가능 (FK는 Comment.article이 관리).**
- INSERT 쿼리 실행이 한 번만 발생.
- 객체 그래프 탐색이 편리함 (article.getComments() 가능).

### ❌ 단점
- mappedBy로 인해 데이터 저장이 Comment 쪽에서 이루어져야 함.

## ✅6️⃣ 정리
- ✔️ **@OneToMany는 하나(One)의 엔티티가 여러 개(Many)의 엔티티를 참조할 때 사용.**
- ✔️ **양방향 관계에서는 @OneToMany(mappedBy = "필드명") + @ManyToOne 조합 사용**
- ✔️ **단방향 @OneToMany보다는 양방향을 사용하는 것이 일반적**
- ✔️ **외래 키(FK)는 @ManyToOne 쪽에서 관리하며, @OneToMany는 읽기 전용**
- 📌 **게시글-댓글 관계처럼 1:N 관계가 필요할 때 @OneToMany를 사용하면 됩니다.**
