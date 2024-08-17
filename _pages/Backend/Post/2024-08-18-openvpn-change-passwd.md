---
title: "📝[Post] OpenVPN Password 변경방법."
tags:
    - Post
    - Backend
date: "2024-08-18"
thumbnail: "/assets/img/thumbnail/blog.jpeg"
---

# 🙋‍♂️ OpenVPN Password 변경방법.
- OpenVPN Access Server에서 사용자의 비빌번호를 변경하는 방법은 다음과 같습니다.
- ✏️ **사용자 비밀번호 변경.**
    - 다음 명령어를 사용하여 **`'openvpn'`** 사용자의 비밀번호를 변경할 수 있습니다.
```bash
sudo /usr/local/openvpn_as/scripts/sacli --user openvpn --new_pass "your-new-password" SetLocalPassword
```
> 🎯 **`'your-new-password'`** 를 원하는 새 비밀번호로 바꿔서 입력하세요.

# ✏️ 참고.
- **`'sacil'`** 명령어를 사용할 때, 정확한 옵션과 명령어 구문을 확인하기 위해 다음과 같은 명령어로 도움말을 볼 수 있습니다. 🤩
```bash
sudo /usr/local/openvpn_as/scripts/sacli --help
```
> 🎯 이 명령어를 통해 **`'sacil'`** 에서 사용 가능한 모든 옵션과 명령을 확인할 수 있습니다. :)
