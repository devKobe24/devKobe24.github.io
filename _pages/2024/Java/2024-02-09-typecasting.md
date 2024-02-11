---
title: ☕️[Java] 형변환 정리.
date: 2024-02-09 11:23:00
modified: 2024-02-09 11:29:00
tags: [Java, Programming Language]
description: Java의 Type Casting에 대하여.
---

<h1>☕️[Java] 형변환 정리.</h1>
<p>
    <h3>형변환</h3>
</p>
<p>
    <strong>int</strong> => <strong>long</strong> => <strong>double</strong>
    <ul>
        <li>작은 범위에서 큰 범위로는 대입할 수 있습니다.</li>
        <ul>
            <li>이것을 묵시적 형변환 또는 자동 형변환이라 합니다.</li>
        </ul>
</ul>
</p>
<p>
    <ul>
        <li>큰 범위에서 작은 범위의 대입은 다음과 같은 문제가 방생할 수 있습니다. 이때는 명식적 형변환을 사용해야 합니다.</li>
        <ul>
            <li>소수점 버림</li>
            <li>오버플로우</li>
        </ul>
</ul>
</p>
<p>
    <ul>
        <li>연산과 형변환</li>
        <ul>
            <li>같은 타입은 같은 결과를 냅니다.</li>
            <li>서로 다른 타입의 계산은 큰 범위로 자동 형변환이 일어납니다.</li>
        </ul>
</ul>
</p>
