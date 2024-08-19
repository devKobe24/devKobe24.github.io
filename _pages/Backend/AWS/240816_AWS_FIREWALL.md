---
title: "☁️[AWS] 방화벽에서 허용된 통신만 통과."
tags:
    - AWS
    - Network
date: "2024-08-16"
thumbnail: "/assets/img/thumbnail/aws.jpeg"
---

# ☁️[AWS] 방화벽에서 허용된 통신만 통과.
<img src = "https://github.com/devKobe24/images2/blob/main/AWS/aws-1-1.png?raw=true">
- 인터넷을 통해 전 세계의 다양한 웹 사이트나 서비스에 접속할 수 있지만 전 세계와 연결된다는 것은 각종 악성 코드나 해커의 침입에 노출된다는 뜻과도 같습니다.
    - 이런 보안 위협으로부터 시스템을 지키기 위해 사용하는 것이 **방화벽** 입니다.
- 방화벽은 이름에서 알 수 있듯이 화재(보안 위협)가 발생했을 때 화재로부터 자산(시스템)을 지키는 역할을 합니다.
- 하드웨어 형태의 방화벽도 있고 소프트웨어 형태로 서버에 설치돼 동작하는 방화벽도 있습니다.
    - 일반적으로 사용하는 윈도우 PC에도 '보안 센터' 설정을 보면 방화벽이 설치돼 동작하고 있습니다.
    - AWS에서는 보안 그룹이나 네트워크 ACL 같은 방화벽 기능을 가진 서비스가 있습니다.