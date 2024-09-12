---
title: 🌐 [AWS] IAM이란?
date: 2024-02-02 06:45:00
modified: 2024-02-02 06:59:00
tags: [AWS, Cloud platform]
description: AWS에서 IAM에 대하여.
---

# 🌐 [AWS] IAM이란?

<p>
    <strong>IAM(Identity and Access Management)</strong>은 누가, 어떤 리소스나 서비스를 사용할 수 있는지 접근 레벨이나 권한(permission) 관리 기능을 제공합니다.<br>
    대부분의 AWS 리소스는 지역(region)에 따라 제공되는 서비스와 기능이 다르지만, IAM은 유니버셜(Universal)합니다.<br>
    따라서 IAM 사용 시 따로 지역을 설정해줄 필요가 없습니다.<br>
    IAM에서 일어나는 모든 것은 전 지역에 적용되기 때문입니다.
</p>
<p>
    <strong>IAM</strong>은 유저 X에 대한 엑세스 키(access key)와 비밀 키(secret key)를 부여합니다.<br>
    두 가지 키를 가지고 유저 X는 AWS 내의 다양한 서비스를 사용할 수 있습니다.<br>
    AWS 메인 페이지에서 로그인할 때 제공하는 유저 아이디 및 비밀번호와는 다른 개념입니다.<br>
    엑세스 키와 비밀 키는 사람이 이해할 수 없는 긴 문자열로 이루어져 있고, API 혹은 콘솔에서 이 키를 가지고 AWS 리소스를 사용합니다.
</p>
<p>
    <strong>IAM</strong>은 세밀한 접근 권한 관리를 가능하게 합니다.<br>
    데이터베이스를 생각해봅시다.<br>
    테이블을 생성하고 데이터를 삽입하거나 읽어올 수 있고, 수정하고, 삭제하는 등 다양한 액션을 취할 수도 있습니다.<br>
    앞서 언급한 유저 X에게 모든 권한을 부여할 수 있고, 데이터만 읽을 수 있는 읽기 전용(view only) 권한도 부여할 수도 있습니다.<br>
    <strong>IAM</strong>은 리소스에 대한 권한만 관리하는 것이 아니라 리소스 안에서 이루어지는 다양한 행동에 대한 세밀한 접근 원한 관리도 할 수 있습니다.
</p>
<p>
    루트 유저, 혹은 루트 유저가 만들어낸 유저는 AWS에 접속할 때 아이디와 비밀번호를 입력하면 로그인할 수 있습니다.<br>
    아마존에서는 <strong>다요소 인증(multi-factor authentication, MFA)</strong>을 활성화시킬 것을 적극 권장하고 있습니다.<br>
    MFA는 사용자의 다른 계정(Facebook, Google 등)을 통한 2차 인증을 거치는 추가적인 과정입니다.
</p>
<img src="https://github.com/devKobe24/images/blob/main/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202024-02-02%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%206.58.09.png?raw=true"><br>
