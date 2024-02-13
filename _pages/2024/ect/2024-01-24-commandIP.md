---
title: 🤔[Command] macOS의 terminal에서 현재 IP 주소를 확인하는 명령어.
date: 2024-01-24 07:46:00
modified: 2024-01-24 08:20:00
tags: [command]
description: ip 주소 확인하는 명령어.
---

# 🤔[Command] macOS의 terminal에서 현재 IP 주소를 확인하는 명령어.

<p>macOS의 터미널에서 현재 IP 주소를 확인하려면 다음과 같은 명령어를 사용할 수 있습니다:</p>

<p><strong>1. 로컬 네트워크 내에서의 IP 주소를 확인하는 경우:</strong></p>

<p><pre>ifcongif | grep "inet " | grep -v 127.0.0.1</pre></p>

<p>이 명령어는 <strong>'ifconfig'</strong>를 사용하여 네트워크 인터페이스의 정보를 가져오고 <strong>'grep'</strong>을 이용하여 IPv4 주소만을 필터링합니다.<br>
<br><strong>'127.0.0.1'</strong>(로컬호스트 주소)는 제외됩니다.</p>

<p><strong>2. 외부 인터넷에 대한 공용 IP 주소를 확인하는 경우:</strong></p>
<p>
<pre>curl ifconfig.me</pre>
<p>또는</p>
<pre>curl icanhazip.com</pre>
</p>
<p>
    이 명령어들은 외부 서비스(<strong>'ifconfig.me'</strong> 또는 <strong>'icanhazip.com'</strong>)을 사용하여 공용 IP 주소를 가져옵니다.<br>
</p>
<p>
이 방법은 인터넷을 통해 접속하는 다른 사용자들에서 보이는 IP 주소를 확인할 때 유용합니다.<br>    
</p>
