---
title: ☕️[JAVA] Packaing 옵션.
date: 2024-02-06 13:46:00
modified: 2024-02-06 14:27:00
tags: [JAVA, Programming language]
description: Java의 Jar 파일과 War 파일에 대하여.
---

<h1>☕️[JAVA] Packaing 옵션.</h1>
<p>
    <a href="https://start.spring.io">Spring Initializr</a>에서 <strong>"Packaing"</strong> 옵션을 선택할 때 <strong>'Jar'</strong>와 <strong>'War'</strong> 중에 선택해야 합니다.
</p>
<p>
    어떤 것을 선택해야 할지는 개발하려는 어플리케이션의 유형과 배포 환경에 따라 달라집니다.
</p>
<p>
    <h2>각 포맷에 대한 설명.</h2>
</p>
<p>
    <h3>1️⃣ Jar (Java Archive)</h3>
</p>
<p>
    <ul>
        <li><strong>Jar</strong> 파일은 Java 클래스 파일, 메타데이터, 리소스 파일을 하나의 파일로 압축한 포맷입니다.</li>
        <li><strong>스탠드얼론(Spring Boot 어플리케이션 권장 포맷): </strong> Jar 포맷은 내장된 서버(예: Tomcat, Jetty)를 사용하여 스프링 부트 어플리케이션을 스탠드얼론 어플리케이션으로 실행할 수 있게 합니다. 이는 별도의 웹 서버 설치 없이도 실행 가능하며, 마이크로서비스, 클라우드 어플리케이션 개발에 적합합니다.</li>
        <li><strong>간편한 배포와 실행:</strong> Jar 파일은 <strong>'java -jar'</strong> 명령어로 쉽게 실행할 수 있으며, 도커 컨테이너와 같은 환경에 배포하기도 용이합니다.</li>
</ul>
</p>
<p>
    <h3>2️⃣ War (Web Application Archive)</h3>
</p>
<p>
    <ul>
        <li><strong>War</strong> 파일은 웹 어플리케이션에 필요한 Java 클래스 파일, JSP(JavaServer Pages), 서블릿, 리소스 파일, 메타데이터 등을 포함한 포맷입니다.</li>
        <li><strong>전통적인 웹 어플리케이션:</strong> War 포맷은 서블릿 컨테이너나 어플리케이션 서버(예: Tomcat, Jetty, WebLogic, WildFly)에 배포될 전통적인 웹 어플리케이션 개발에 사용됩니다. 이 경우, 어플리게이션 서버가 웹 어플리케이션을 실행하는데 필요한 환경을 제공합니다.</li>
        <li><strong>엔터프라이즈 환경: </strong>복잡한 엔터프라이즈 환경에서는 여러 어플리케이션을 하나의 서버에 배포해야 할 필요가 있을 수 있으며, War 포맷이 이러한 요구 사항을 충족시킬 수 있습니다.</li>
</ul>
</p>
<p>
    <h3>🙌 선택 기준</h3>
</p>
<p>
    <ul>
        <li><strong>스탠드얼론 어플리케이션 개발 및 마이크로 아키텍처를 선호한다면 'Jar'를 선택</strong>하세요.</li>
        <li><strong>기존의 엔터프라이즈 환경에서 어플리케이션 서버를 사용해야 한다면 'War'를 선택</strong>하세요.</li>
</ul>
</p>
<p>
    Spring Boot는 두 가지 포맷 모두를 지원하므로, 프로젝트 요구 사항과 배포 환경에 맞게 최적의 옵션을 선택할 수 있습니다.
</p>
