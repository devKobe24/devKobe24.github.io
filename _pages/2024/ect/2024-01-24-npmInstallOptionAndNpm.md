---
title: 📦[Package] npm install 옵션과 npm에 관하여.
date: 2024-01-24 05:34:00
modified: 2024-01-24 07:41:00
tags: [npm, package]
description: npm install option
---

# 📦[Package] npm install 옵션에 관하여
<p>
0. npm install은 현재 작업 중인 디렉토리의 package.json 파일을 기반으로 패키지를 설치합니다.<br>
<ul>
	<li>현재 디렉토리에 package.json 파일이 없다면, npm은 패키지를 설치하지만 package.json 파일에는 반영되지 않습니다.</li>
</ul>
</p>

<p>
1. npm install {package name} --global<br>
<ul>
	<li>npm 패키지 install 시 --global 옵션을 사용하면 전역으로 패키지가 전역적으로 설치가 됩니다.</li>
</ul>
</p>

<p>
2. npm install {package name} --save<br>
<ul>
	<li>
		npm install 명령어를 사용할 때 --save 옵션을 추가하면 설치하는 패키지가 package.json의 dependencies 섹션에 자동으로 추가됩니다.
	</li>
		<ul>
			<li>
				예를 들어 express 패키지를 설치하고자 한다면 다음과 같이 명령합니다:
			</li>
    </ul>
</ul>
<pre>
npm install express --save
</pre>
<ul>
	<li>이 명령을 실행하면 express 패키지가 package.json 파일의 dependencies 리스트에 추가됩니다.</li>
</ul>
</p>

<p>
3. npm install {package name} --save-dev<br>
<ul>
	<li>개발 중에만 필요한 패키지를 설치할 때는 --save-dev 옵션을 사용합니다.</li>
	<li>이 옵션을 사용하면 패키지가 package.json의 devDependencies 섹션에 추가됩니다.</li>
	<li>예를 들어, nodemon을 개발용 의존성으로 설치하려면 다음과 같이 명령합니다:</li>
</ul>
<pre>
npm install nodemon --save-dev
</pre>
</p>

# 📦 NPM(Node Package Manager)란?
<p>
<ul>
	<li>NPM(Node Package Manager)은 Node.js를 설치하면 자동으로 설치가 되는 패키지 매니저입니다.</li>
	<li>NPM은 명령어로 자바스크립트 라이브러리를 설치하고 관리할 수 있는 패키지 매니저로, 전 세계 자바스크립트 개발자들이 모두 자바스크립트 라이브러리를 공개된 저장소에 올려놓고 npm 명령어로 편하게 다운로드가 가능합니다.</li>
			<ul>
				<li>(cocoaPods이랑 비슷한건가?? ㅇㅅㅇ)</li>
			</ul>
</ul>
</p>
