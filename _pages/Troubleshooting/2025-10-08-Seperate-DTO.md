---
title: "🔍[Troubleshooting] 🚀 DTO 분리: 용도별 전용 DTO 설계"
tags:
    - Troubleshooting
    - Backend Development
    - Spring Boot
date: "2025-10-08"
thumbnail: "/assets/img/thumbnail/troubleshooting.jpg"
---

# 🚀 DTO 분리: 용도별 전용 DTO 설계

---

## 🧐 무엇이 문제인가요?

현재 `ChapterRequestDto`는 여러 가지 역할을 한 번에 수행하려는 **'만능 DTO'** 가 되었습니다. 

이는 **API 설계의 명확성을 심각하게 해치는 안티패턴(Anti-Pattern)** 입니다.

### 1. API의 의도가 불분명해집니다

`searchChapter` 메서드는 이름 그대로 '검색'을 위한 기능입니다. 

그런데 파라미터로 받는 `ChapterRequestDto`에는 검색과 무관한 `chapterNumber`나 `detailChapter` 같은 필드들이 포함되어 있습니다. 

API를 사용하는 개발자는 **"검색하는데 이 필드들은 왜 필요하지? null로 보내도 되나?"** 라는 혼란에 빠지게 됩니다.

### 2. 잘못된 데이터가 전달될 수 있습니다

다른 개발자가 오해하여 검색 시 `chapterTitle`뿐만 아니라 불필요한 `detailChapter` 정보까지 채워서 보낼 수 있습니다. 

이는 네트워크 낭비일 뿐만 아니라, 서버 측에서 의도치 않은 동작을 유발할 수도 있는 잠재적 버그의 원인이 됩니다.

### 3. 단일 책임 원칙(SRP) 위배

현재 `ChapterRequestDto`는 '챕터 생성용 데이터'와 '챕터 검색용 데이터'라는 **두 가지 책임을 동시에** 지고 있습니다. 

좋은 소프트웨어 설계는 각 클래스가 단 하나의 책임만 갖도록 하는 것입니다.

---

## ✅ 해결 방안: 역할에 따라 DTO를 분리하세요

가장 좋은 해결책은 **'용도에 맞는 전용 DTO'** 를 각각 만들어 사용하는 것입니다.

### 1. 검색 전용 DTO 생성

검색에 필요한 `chapterName` 필드만 가진 `ChapterSearchRequestDto`를 새로 만듭니다. 

클래스 이름만 봐도 이 DTO의 역할이 무엇인지 명확하게 알 수 있습니다.

**📄 `dto/request/ChapterSearchRequestDto.java` (신규 생성)**

```java
package com.kobe.koreahistory.dto.request;

import lombok.Getter;
import lombok.NoArgsConstructor;

/**
 * '챕터 검색'이라는 역할만 수행하는 전용 DTO
 */
@Getter
@NoArgsConstructor
public class ChapterSearchRequestDto {
    private String chapterName;
}
```

---

### 2. 생성/수정용 DTO 이름 변경 (선택사항이지만 권장)

기존 `ChapterRequestDto`는 이름만 봐서는 역할을 알기 어렵습니다. 

'챕터 생성'에 사용된다면 `ChapterCreateRequestDto`와 같이 더 명확한 이름으로 변경하는 것이 좋습니다.

**📄 `dto/request/ChapterCreateRequestDto.java` (이름 변경)**

```java
package com.kobe.koreahistory.dto.request;

import lombok.Getter;
import lombok.NoArgsConstructor;
import java.util.List;

/**
 * '챕터 생성'에 필요한 모든 정보를 담는 DTO
 */
@Getter
@NoArgsConstructor
public class ChapterCreateRequestDto {
    private int chapterNumber;
    private String chapterTitle;
    private List<DetailChapterRequestDto> detailChapters;
}
```

---

### 3. Controller에 올바른 DTO 적용

이제 `KoreanHistoryController`의 `searchChapter` 메서드는 **검색 전용 DTO**인 `ChapterSearchRequestDto`를 사용하도록 수정합니다.

**📄 수정된 `KoreanHistoryController.java`**

```java
package com.kobe.koreahistory.controller;

import com.kobe.koreahistory.dto.request.ChapterSearchRequestDto;
import com.kobe.koreahistory.dto.response.ChapterResponseDto;
import com.kobe.koreahistory.service.ChapterService;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/v1")
@RequiredArgsConstructor
public class KoreanHistoryController {

    private final ChapterService chapterService;

    @PostMapping("/search/chapters")
    public ResponseEntity<ChapterResponseDto> searchChapters(
        @RequestBody ChapterSearchRequestDto requestDto
    ) {
        return ResponseEntity.ok(
            chapterService.findChapterWithDetails(requestDto.getChapterName())
        );
    }
}
```

---

## ✨ 개선 후의 장점

### API 명세의 명확성
이제 `searchChapters` API를 보는 개발자는 요청 Body에 `chapterName` 하나만 넣으면 된다는 사실을 DTO의 이름과 필드만 보고도 **즉시** 알 수 있습니다. 

혼동의 여지가 사라집니다.

### 유지보수 용이성
나중에 검색 조건이 추가되면 `ChapterSearchRequestDto`만 수정하면 됩니다. 

생성 로직에 아무런 영향을 주지 않습니다.

### 안정성
불필요한 데이터가 서버로 전달될 가능성을 원천적으로 차단하여 더 견고한 코드가 됩니다.

---

## 💡 결론

> **"하나의 DTO를 여러 API에서 돌려쓰지 말고, 각 API의 역할에 맞는 전용 DTO를 만들라"**

### DTO 설계 원칙

- ✅ **용도별 분리**: 생성용, 수정용, 검색용 DTO를 각각 만들기
- ✅ **명확한 네이밍**: DTO 이름만 봐도 용도를 알 수 있게
- ✅ **단일 책임**: 하나의 DTO는 하나의 역할만
- ✅ **필요한 필드만**: 불필요한 필드는 포함하지 않기
