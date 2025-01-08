---
title: 🛠️[개발 도구 및 환경] DataGrip으로 생성한 MySQL DB를 Docker 이미지로 만들기.
tags:
    - Development tools
    - Enviroments
date: "2025-01-08"
thumbnail: "/assets/img/thumbnail/tools.jpg"
---

# 🛠️[개발 도구 및 환경] DataGrip으로 생성한 MySQL DB를 Docker 이미지로 만들기.
## ✅1️⃣ MySQL 데이터 덤프(Export)
- ↘︎ `mysqldump` 명령어를 사용하여 MySQL 데이터를 덤프한다.
```shell
mysqldump -u root -p --databases article > article.sql
```
- ↘︎ **`-u root` :** MySQL 사용자명.
- ↘︎ **`-p` :** 비밀번호 입력 요청.
- ↘︎ **`--databasese` :** 특정 데이터베이스 선택.
- ↘︎ **`article.sql` :** 데이터 덤프 파일명.

## ✅2️⃣ Dockerfile 생성.
- ↘︎ MySQL을 기반으로 한 Docker 이미지를 만들기 위한 Dockerfile을 생성.
    - ↘︎ **Dockerfile 예시:**
    ```dockerfile
    FROM mysql:8.0.38
    
    # 환경 변수 설정
    ENV MYSQL_DATABASE=article
    
    # 덤프된 SQL 파일 복사
    COPY article.sql /docker-entrypoint-initdb.d/
    
    # MySQL 포트 노출
    EXPOSE 3306
    ```
- ↘︎ **설명:**
    - ↘︎ `FROM mysql:8.0.38` : MySQL 8.0.38 이미지 사용.
    - ↘︎ `ENV` : 환경 변수 설정.
    - ↘︎ `COPY` : 덤프 파일을 MySQL 초기화 디렉토리에 복사.
    - ↘︎ `/docker-enrtypoint-initdb.d/` : 디렉터리는 컨테이너가 시작될 때 SQL 스크립트를 자동으로 실행.

## ✅3️⃣ Docker 이미지 빌드.
- ↘︎ Dockerfile이 있는 디렉터리로 이동한 후, Docker 이미지를 빌드.
```shell
docker build .
```

## ✅4️⃣ Dockerfile의 위치.
### 📌 설명.
- ↘︎ Dockerfile은 사용자가 직접 **생성해야 함.**
    - ↘︎ MySQL 공식 이미지(mysql:8.0.38)에는 Dockerfile이 포함되어 있지 않으며, 사용자의 요구사항에 맞게 커스터마이징해야 함.

### 📌 Dockerfile 생성 위치.
- ↘︎ Dockerfile은 주로 다음과 같은 위치에 생성됨.
    - ↘︎ **프로젝트 루트 디렉터리**
    - ↘︎ **Docker 관련 설정 디렉터리 (예: docker/)**
        - ↘︎ **예시 프로젝트 구조:**
        ```shell
        kobe-board/
        ├── docker/
        │   ├── Dockerfile       ← Dockerfile 생성
        │   ├── backup.sql       ← MySQL 덤프 파일 (선택 사항)
        │   ├── docker-compose.yml (선택 사항)
        ├── service/
        │   ├── article/
        │   ├── comment/
        └── common/
        ```

## ✅5️⃣ Dockerfile 생성.
### 📌 프로젝트 루트 디렉터리로 이동.
- ↘︎ 
```shell
cd /Users/kobe/Desktop/kobe-board
```

### 📌 docker 디렉터리 생성(선택 사항).
- ↘︎ 
```shell
mkdir docker
cd docker
```

### 📌 Dockerfile 생성.
- ↘︎ 
```shell
vi Dockerfile
```

### 📌 Dockerfile 내용 작성.
- ↘︎ **예제 Dockerfile:** 
```dockerfile
# MySQL 공식 이미지 사용
FROM mysql:8.0.38

# 환경 변수 설정
ENV MYSQL_DATABASE=article

# SQL 덤프 파일 복사 (선택 사항)
COPY article.sql /docker-entrypoint-initdb.d/

# MySQL 포트 노출
EXPOSE 3306
```

## ✅6️⃣ 주의사항
- ↘︎ **1. Dockerfile 위치**
    - ↘︎ Dockerfile은 `docker build` 명령어를 실행하는 디렉터리에 존재해야 함.
- ↘︎ **2. SQL 덤프 파일**
    - ↘︎ article.sql 파일이 존재하지 않는다면 해당부분(COPY article.sql)을 생략하거나 수정해야 함.
- ↘︎ **3. 디렉터리 권한**
    - ↘︎ MySQL 데이터가 저장될 디렉터리는 Docker가 접근 가능해야 함.
