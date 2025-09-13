---
title: 🛠️[개발 도구 및 환경] .gitignore가 동작하지 않아요!
tags:
    - Development tools
    - Enviroments
date: "2024-11-24"
thumbnail: "/assets/img/thumbnail/tools.jpg"
---

# 🛠️[개발 도구 및 환경] .gitignore가 동작하지 않아요!
## 1️⃣ .gitignore가 제대로 동작하지 않을 때.
- `.gitignore`를 생성한 후 내부에 `.DS_Store`를 추가하고 나서 언젠가부터 **ignore 처리된 파일이 changes에 나오기 시작했습니다.**
    - 그래서 그 이유를 찾아보니 **"git의 캐시가 문제가 되는 것"이였습니다.**
        - 아래의 명령어로 캐시 내용을 **전부 삭제후 다시 add All 해서 커밋하니 제대로 동작했습니다.**
```bash
git rm -r --cached .
git add .
git commit -m "fixed untracked files"
```

### 📝 REFERENCE
- [기억보단 기록을](https://jojoldu.tistory.com/307)
