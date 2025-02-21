---
title: "📚[Backend Development] Path Enumeration 방식에서 댓글의 경로(Path) 결정 과정."
tags:
    - Backend Ddevelopment
date: "2025-02-21"
thumbnail: "/assets/img/thumbnail/BackendDevelopment.jpg"
---

# "📚[Backend Development] Path Enumeration 방식에서 댓글의 경로(Path) 결정 과정."
## 📌1️⃣ 신규 댓글의 경로를 결정하는 과정
- Path Enumeration(경로 열거) 방식을 사용할 때, **새로운 댓글이 추가될 경우** 해당 댓글의 path를 어떻게 결정할 것인지가 중요합니다.
- 이 글에서는 **이미 존재하는 계층형 댓글 트리에서 새로운 댓글이 추가될 때, path를 어떻게 생성하는지**에 대해 설명합니다.

## 🏗️2️⃣ 기존 댓글 구조 확인
### ✅ 기존 댓글 트리

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/infinite-depth-example_3.png?raw=true">

- 초기 댓글 구조는 위와 같습니다.
    - 최상위 댓글 00a0z 아래에 여러 개의 하위 댓글이 존재합니다.
    - 각 댓글의 path 계층 구조를 따라 **부모 댓글의 path를 상속받으며,** 새로운 댓글이 추가될 때마다 숫자가 증가하는 방식으로 정렬됩니다.
    - 가장 최근의 하위 댓글은 00a0z 00002이며, 00a0z 00002의 하위 댓글로 00a0z 00002 00000이 존재합니다.

## 🏗️3️⃣ 신규 댓글 추가 요청.
### ✅ 새로운 댓글 요청

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/infinite-depth-example_4.png?raw=true">

- 어떤 사용자가 00a0z 댓글의 하위에 새로운 댓글을 작성하려고 합니다.
- 하지만, 현재 00a0z의 하위 댓글들은 이미 존재하고 있으므로, **새로운 댓글이 들어갈 올바른 path를 결정해야 합니다.**

## 🏗️4️⃣ 현재 존재하는 하위 댓글 중 가장 큰 path 찾기
### ✅ childrenTopPath 찾기

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/infinite-depth-example_5.png?raw=true">

- 새로운 댓글을 추가할 때는, **현재 존재하는 하위 댓글 중 가장 큰 path(childrenTopPath)를 찾아서** 그 값에 +1을 하여 새로운 댓글의 path를 생성합니다.
    - 현재 00a0z의 하위 댓글 중에서 가장 큰 path는 00a0z 00002입니다.
    - 따라서, 새로운 댓글의 path는 00a0z 00003이 됩니다.

## 🏗️5️⃣ 모든 자식 댓글을 고려한 descendantsTopPath 찾기

- 하지만, **단순히 childrenTopPath만 고려하면 안됩니다.** 자식 댓글이 있는 경우, **가장 깊은 depth에 있는 자식 댓글까지 고려하여 path를 결정해야 합니다.**
    - descendantsTopPath는 **부모 댓글을 포함한 모든 자식 댓글 중 가장 큰 path를** 의미합니다.
    - 즉, 00a0z의 모든 하위 댓글 중 **가장 깊은 depth를 가지면서도 가장 큰 path를 찾습니다.**

## 🏗️6️⃣ descendantsTopPath에서 신규 댓글의 depth에 맞는 childrenTopPath 계산
### ✅ descendantsTopPath를 기반으로 path 생성

<img src = "https://github.com/devKobe24/images2/blob/main/this_is_backend_img/infinite-depth-example_6.png?raw=true">

- 기존 댓글 중 **가장 깊은 depth를 가지는 descendantsTopPath를 찾고, 신규 댓글이 들어갈 depth만큼의 path를 남기고 나머지는 잘라냅니다.**
    - descendantsTopPath = 00a0z 00002 00000
    - 하지만 신규 댓글이 들어갈 depth는 2이므로, (depth * 5)만큼의 문자만 남깁니다.
    - 결과적으로 **childrenTopPath = 00a0z 00002**가 됩니다.

## 🏗️7️⃣ 최종적으로 childrenTopPath를 찾아 신규 댓글의 path 생성
### ✅ 최종 path 결정
- **1. parentPath를 가지는 모든 자식 댓글을 조회**
- **2. 가장 큰 descendantsTopPath를 찾음**
- **3. 신규 댓글이 들어갈 depth만큼 path를 남기고 자름 → childrenTopPath 생성**
- **4. 마지막 숫자에 +1을 하여 최종 path 결정**
- 📌 **결과적으로, 새로운 댓글의 path는 00a0z 00003이 됩니다.**

## 🚀8️⃣ 결론.

- ✅ **Path Enumeration 방식을 사용하면, 댓글의 계층 구조를 명확하게 유지하면서도 정렬 및 조회를 빠르게 수행할 수 있습니다.**
- ✅ **신규 댓글이 추가될 때는, 현재 존재하는 하위 댓글 중 가장 큰 path(descendantsTopPath)를 찾아서 새로운 path를 결정합니다.**
- ✅ **이 방식은 무한 Depth 댓글에서도 정렬 순서를 유지하면서 빠르게 댓글을 추가할 수 있도록 도와줍니다.**
