---
title: "📝[Post] OpenVPN 클라이언트의 IP 주소와 OpenVPN 서버에서 할당된 서브넷 정보 가져오는 방법."
tags:
    - Post
    - Backend
date: "2024-08-18"
thumbnail: "/assets/img/thumbnail/blog.jpeg"
---

# 🙋‍♂️ OpenVPN 클라이언트의 IP 주소와 OpenVPN 서버에서 할당된 서브넷 정보 가져오는 방법.
- 1. 먼저 OpenVPN Access Server에 접속합니다.
> 🎯 OpenVPN 클라이언트는 꺼진 상태여야 합니다.
> 만약 OpenVPN 클라이언트가 연결되어 있다면 연결을 끊어주세요.
- 2. 로그 파일을 살펴봅니다.
```bash
sudo tail -f /var/log/openvpnas.log
```
- 3. **`'SENT CONTROL [client_name]'` 또는 `'PUSH_REPLY...'`** 와 같은 메시지를 찾아봅니다.
    - 로그에서 위와 같은 메시지에는 클라이언트가 할당받은 IP 주소 및 서브넷 정보가 포함 되어 있을 수 있습니다.
<img src = "https://github.com/devKobe24/images2/blob/main/openvpn-client-ip.png?raw=true">
