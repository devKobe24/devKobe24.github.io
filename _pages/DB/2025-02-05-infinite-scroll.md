---
title: "💾[Database] 무한 스크롤 방식(Infinite Scroll)이란 무엇일까요?"
tags:
    - Database
    - Spring Boot
    - JPA
    - REST API
date: "2025-02-05"
thumbnail: "/assets/img/thumbnail/database.jpeg"
---

# "💾[Database] 무한 스크롤 방식(Infinite Scroll)이란 무엇일까요?"
## 🍎 Intro.
- 사용자가 스크롤을 내릴 때마다 새로운 데이터를 동적으로 로드하는 방식입니다.
- Java에서는 주로 **Spring Boot + JPA + REST API**를 활용하여 무한 스크롤 방식의 백엔드를 구현합니다.

## ✅1️⃣ 무한 스크롤 방식의 핵심 개념.
### 1️⃣ 클라이언트가 서버에 데이터 요청.
- 스크롤 이벤트 발생 시, 클라이언트가 서버에 데이터를 요청.
- 요청 시 cursor(마지막 게시물 ID) 또는 page(페이지 번호)를 포함.

### 2️⃣ 서버가 요청받은 데이터만 반환.
- LIMIT 또는 OFFSET을 사용하여 지정된 개수만큼의 데이터 반환.
- **커서 기반 페이징(Cursor Pagination)이** 가장 효율적.

### 3️⃣ 클라이언트가 받은 데이터를 추가로 렌더링.
- 기존 데이터에 새로운 데이터를 추가하여 UI 업데이트.

## ✅2️⃣ Java에서 무한 스크롤을 구현하는 2가지 방식.
### 1️⃣ 페이지 번호 기반 페이징(Offset Pagination)
- 요청 시 **페이지 번호(Page Number)와 페이지 크기(Page Size)를 전달**하여 특정 페이지 데이터를 조회.
- SQL에서 LIMIT과 OFFSET을 활용.

### 2️⃣ 커서 기반 페이징(Cursor Pagination)
- 마지막으로 조회한 데이터의 ID 또는 Timestamp를 기준으로 다음 데이터를 요청.
- **대규모 데이터 처리에 최적화됨.**

## ✅3️⃣ 무한 스크롤 방식의 장단점.
### ✅ 장점.
- **1. 사용자 경험(UX) 개선 ➞** 사용자가 자견스럽게 데이터를 탐색 가능.
- **2. 성능 최적화 ➞** 필요한 만큼만 데이터를 요청하여 서버 부담 감소.
- **3. 네트워크 최적화 ➞** 페이지 전체를 로드하지 않고 필요한 데이터만 전송.

### ❌ 단점.
- **1. 특정 데이터 검색 어려움 ➞** 사용자가 원하는 데이터를 다시 찾기 어려움.
- **2. SEO 문제 ➞** 검색 엔진이 전체 콘텐츠를 크롤링하기 어려움.
- **3. 메모리 사용 증가 ➞** 클라이언트 측에서 데이터가 계속 추가되면 성능 저하 가능.

## 🚀 결론.
- Java에서 무한 스크롤을 구현할 때는 **커서 기반 페이징(Cursor Pagination)이** 성능상 유리.
- Spring Boot + JPA에서 REST API를 제공하고, 클라이언트에서 요청을 보내는 방식으로 구현.
- **대규모 데이터를 처리할 때는 커서 기반 페이징이 적합.**
