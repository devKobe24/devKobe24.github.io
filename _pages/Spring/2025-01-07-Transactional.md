---
title: 🍃[Spring] `@Transactional`이 read 메서드에 붙어있지 않은 이유는 무엇일까요?
tags:
    - Spring
    - Framework
date: "2025-01-07"
thumbnail: "/assets/img/thumbnail/spring.jpeg"
---

# 🍃[Spring] `@Transactional`이 read 메서드에 붙어있지 않은 이유는 무엇일까요?
## 📌 Intro 및 코드 소개.
```java
package kobe.board.article.service;

import kobe.board.article.entity.Article;
import kobe.board.article.repository.ArticleRepository;
import kobe.board.article.service.request.ArticleCreateRequest;
import kobe.board.article.service.request.ArticleUpdateRequest;
import kobe.board.article.service.response.ArticleResponse;
import kobe.board.common.exception.CustomException;
import kobe.board.common.responsecode.ResponseCode;
import kuke.board.common.snowflake.Snowflake;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
@RequiredArgsConstructor
public class ArticleService {
	private final Snowflake snowflake = new Snowflake();
	private final ArticleRepository articleRepository;

	@Transactional
	public ArticleResponse create(ArticleCreateRequest request) {
		ArticleResponse article = createArticle(request);
		return article;
	}

	private ArticleResponse createArticle(ArticleCreateRequest request) {
		// 1. 게시글 생성 및 저장
		articleRepository.save(
			Article.create(snowflake.nextId(), request.getTitle(), request.getContent(), request.getBoardId(), request.getWriterId())
		);

		// 2. 게시물 여/부 검증.
		Article article1 = articleRepository.findById(snowflake.nextId()).orElseThrow(() -> new CustomException(ResponseCode.NOT_FOUND_ARTICLE));

		// 3. ArticleResponse 리턴
		return ArticleResponse.from(article1, ResponseCode.SUCCESS_CREATE_ARTICLE);
	}

	@Transactional
	public ArticleResponse update(Long articleId, ArticleUpdateRequest request) {
		// 1. 게시물 여/부 검증.
		Article article = articleRepository.findById(articleId).orElseThrow(() -> new CustomException(ResponseCode.NOT_FOUND_ARTICLE));
		
		// 2. 게시물 업데이트.
		article.update(request.getTitle(), request.getContent());
		
		// 3. ArticleResponse 리턴.
		return ArticleResponse.from(article, ResponseCode.SUCCESS_UPDATE_ARTICLE);
	}


	public ArticleResponse read(Long articleId) {
		return ArticleResponse.from(articleRepository.findById(articleId).orElseThrow(), ResponseCode.SUCCESS_READ_ARTICLE);
	}

	@Transactional
	public void delete(Long articleId) {
		articleRepository.deleteById(articleId);
	}
}
```

- ↘︎ 위 클래스는 게시물 서비스 클래스임.
- ↘︎ 게시물 생성, 업데이트, 삭제 관련 메서드에는 `@Transactional`이 붙어있는데 `read` 메서드에는 붙어있지 않음.

## ✅1️⃣ `@Transactional`이 `read` 메서드에 붙어있지 않은 이유.
### ✅1️⃣ `@Transactional` 기본 개념.
- ↘︎ `@Transactional` : 트랜잭션 관리를 위해 사용되며, 주로 데이터베이스의 일관성을 유지하기 위해 사용됨.
- ↘︎ **읽기 전용(read-only) 트랜잭션 :** `readOnly = true`로 설정하면 데이터 변경 없이 조회만 수행하게 됨.

### ✅2️⃣ `read` 메서드에 `@Transactional`이 필요하지 않은 이유
#### 1️⃣ 데이터 변경 없음.
- ↘︎ read 메서드는 단순히 **데이터를 읽기만 하고 변경하지 않는다.**
    - ↘︎ 데이터베이스에 쓰기 작업(INSERT, UPDATE, DELETE)이 없기 때문에 트랜잭션이 필요하지 않다.
#### 2️⃣ 성능 최적화
- ↘︎ 트랜잭션은 리소스를 소모함.
    - ↘︎ 불필요한 트랜잭션을 사용하면 오버헤드가 발생할 수 있음.
- ↘︎ read 메서드는 트랜잭션을 사용할 필요가 없어, 더 가볍고 빠르게 작동함.
#### 3️⃣ 기본적으로 JPA는 읽기 작업에서 트랜잭션이 필요 없음
- ↘︎ JPA는 읽기 작업만 수행할 경우 기본적으로 트랜잭션을 필요로 하지 않음.
- ↘︎ `findById`는 읽기 전용 작업이기 때문에 별도의 트랜잭션 설정이 필요 없음.

### ✅3️⃣ `@Transactional(readOnly = true)`를 사용할 경우.
- ↘︎ `@Transactional(readOnly = true)`를 사용하면 **영속성 컨텍스트는 읽기 전용으로 설정된다.**
- ↘︎ read 메서드는 명시적으로 `@Transactional(readOnly = true)`를 추가할 경우, 다음과 같은 이점이 있다.
    - ↘︎ **성능 최적화 :** 쓰기 연산을 차단하고, 읽기 전용으로 최적화된다.
    - ↘︎ **안전성 :** 잘못된 쓰기 연산이 발생하는 것을 방지한다.

#### 🎯 예시: read 메서드에 `@Transactional(readOnly = true)` 적용
```java
@Transactional(readOnly = true)
public ArticleResponse read(Long articleId) {
    return Articleresponse.from(
        articleRepository.findById(articleId)
        .orElseThrow(
            () -> new CustomException(ResponseCode.NOT_FOUND_ARTICLE)
        ),
        ResponseCode.SUCCESS_READ_ARTICLE
    );
}
```

### ✅4️⃣ 상황별 `@Transactional` 사용 여부.

|메서드 유형|@Transactional 필요 여부|이유|
| -------- | -------- | -------- |
|Create|필요함|데이터 변경 발생(INSERT)|
|Update|필요함|데이터 변경 발생(UPDATE)|
|Delete|필요함|데이터 변경 발생(DELETE)|
|Read(조회만)|불필요|데이터 변경 없음(SELECT)|
|Read(복잡한 조회)|@Transactional(readOnly = true) 권장|읽기 전용 최적화 및 안전성|

## 🚀 결론.
- ↘︎ read 메서드에는 기본적으로 `@Transactional`이 필요하지 않음.
    - ↘︎ 하지만 읽기 전용으로 안전성을 높이고 최적화를 원한다면 **`@Transactional(readOnly = true)`를 사용해도 됨.**

### 👉 권장 접근법.
- ↘︎ 단순 조회라면 `@Transactional`을 생략해도 무방함.
- ↘︎ 읽기 최적화나 추가 안전성이 필요하다면 `@Transactional(readOnly = true)`를 사용하는 것이 좋음.
