---
title: 🛠️[개발 도구 및 환경] Docker에서 네트워크를 사용하는 컨테이너를 확인, 실행 상태 점검, 정지하는 전체 흐름.
tags:
    - Development tools
    - Enviroments
date: "2025-05-22"
thumbnail: "/assets/img/thumbnail/tools.jpg"
---

# 🛠️[개발 도구 및 환경] Docker에서 네트워크를 사용하는 컨테이너를 확인, 실행 상태 점검, 정지하는 전체 흐름.

아래는 Docker에서 ecommerce_be_ecommerce-network 네트워크를 사용하는 컨테이너를 **확인하고, 실행 상태를 점검하며, 정지**하는 전체 흐름입니다.

## ✅ 1. 해당 네트워크를 사용하는 컨테이너 확인 방법.

```bash
docker network inspect ecommerce_be_ecommerce-network
```
- 출력 결과 중 "Containers" 항목에 해당 네트워크에 연결된 컨테이너들의 정보가 나옵니다.
- 예시:
```json
"Containers": {
  "c5d45c33e1bd1d7d8423...": {
    "Name": "user-service",
    "EndpointID": "...",
    "MacAddress": "...",
    "IPv4Address": "172.20.0.2/16",
    ...
  },
  ...
}
```
👉 여기서 "Name"이 컨테이너 이름입니다. (user-service, product-service 등).

## ✅ 2. 해당 컨테이너가 실행 중인지 확인하는 방법.

컨테이너 이름(예:user-service)을 확인한 후:
```bash
docker ps -a --filter name=user-service
```
- 실행 중이면 STATUS가 Up으로 표시됩니다.
- 정지 상태면 Exited가 나옵니다.
> 여러 컨테이너가 있다면 반복해서 확인.

## ✅ 3. 실행 중인 컨테이너 정지 방법.

실행 중인 컨테이너를 멈추려면 다음 명령어 사용:
```bash
docker stop user-service
```

여러 컨테이너를 동시에 멈추려면 공백으로 구분하여 나열:
```bash
docker stop user-service product-service gateway-service
```

## 💡 요약 쉘 스크립트 예시.

```bash
# 네트워크를 사용하는 컨테이너 목록 조회
docker network inspect ecommerce_be_ecommerce-network | jq -r '.Containers[].Name'

# 예: user-service, product-service 가 나온다고 가정

# 실행 여부 확인
docker ps -a --filter name=user-service
docker ps -a --filter name=product-service

# 실행 중이면 정지
docker stop user-service product-service
```

> jq가 없다면 .Container 부분을 grep/awk로 파싱해도 됩니다.
