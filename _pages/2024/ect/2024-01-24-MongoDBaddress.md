---
title: 💾[DataBase] MongoDB에서 데이터베이스의 주소를 확인하는 방법.
date: 2024-01-24 15:55:00
modified: 2024-01-24 16:14:00
tags: [Database]
description: 데이터베이스 주소 확인.
---

# 💾[DataBase] MongoDB에서 데이터베이스의 주소를 확인하는 방법.

<p>
    MongoDB에서 데이터베이스의 주소를 확인하려면, MongoDB 인스턴스에 연결된 상태에서 몇 가지 정보를 알아야 합니다.
</p>

<p>
    일반적으로 MongoDB 데이터베이스의 주소는 다음과 같은 형식으로 구성됩니다.<br>
    <pre>
    mongodb://[username:password@]host:port/database
    </pre>
</p>

<p>
    <h4>여기서 각 부분은 다음을 의미합니다:</h4>
    <ul>
        <li><strong>'username:password@'</strong>: 인증이 필요한 경우 사용되는 사용자 이름과 비밀번호입니다. 인증이 필요하지 않은 경우 이 부분은 생략될 수 있습니다.</li>
        <li><strong>'host'</strong>: MongoDB 서버가 실행 중인 호스트의 주소입니다. 로컬에서 실행 중인 경우 <strong>'localhost'</strong>가 될 수 있고, 원격 서버인 경우 IP 주소나 도메인 이름이 될 수 있습니다.</li>
        <li><strong>'port'</strong>: MongoDB 서비스가 사용하는 포트입니다. 기본값은 <strong>'27017'</strong>입니다.</li>
        <li><strong>'database'</strong>: 연결하고자 하는 데이터베이스의 이름입니다.</li>
    </ul>
</p>

<p>
    예를 들어, 로컬 호스트에서 실행 중인 MongoDB에 <strong>'mydatabase'</strong>라는 이름의 데이터베이스에 접근하려면, 주소는 다음과 같을 것입니다:<br>
    <pre>mongodb://localhost:27017/mydatabase</pre>
</p>

<p>
    이 정보는 MongoDB 서버를 설정할 때 정의되며, MongoDB의 설정 파일이나 실행 명령에서 찾을 수 있습니다.<br>
    또한, MongoDB를 실행하는 클라이언트 애플리케이션 또는 개발환경에서 데이터베이스에 연결할 때 이 주소를 사용합니다.
</p>

<p>
    만약 MongoDB를 클라우드 서비스(예: MongoDB Atlas)를 통해 사용하고 있다면, 해당 서비스의 관리 콘솔에서 연결 주소를 찾을 수 있습니다.
</p>
