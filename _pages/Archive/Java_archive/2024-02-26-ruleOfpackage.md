---
title: "☕️[Java] 패키지 규칙"
tags:
    - Java
    - Programming Language
date: "2024-02-26"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 패키지 규칙.

## 패키지 규칙.
* **"패키지의 이름과 위치는 폴더(디렉토리) 위치와 같아야 합니다.(필수)"**
* 패키지 이름은 모두 소문자를 사용합니다.(관례)
* 패키지의 앞 부분에는 일반적으로 회사의 도메인 이름을 거꾸로 사용합니다.
    * 예를 들어, `com.company.myapp`과 같이 사용합니다(관례)
        * 이 부분은 필수는 아닙니다. 하지만 수 많은 외부 라이브러리가 함께 사용되면 같은 패키지에 같은 클래스 이름이 존재할 수도 있습니다. 이렇게 도메인 이름을 거꾸로 사용하면 이런 문제를 방지할 수 있습니다.
        * 내가 오픈소스나 라이브러리를 만들어서 외부에 제공한다면 꼭 지키는 것이 좋습니다.
        * 내가 만들 애플리케이션을 다른 곳에 공유하지 않고, 직접 배포한다면 보통 문제가 되지 않습니다.

## 패키지와 계층 구조.
패키지는 보통 다음과 같이 계층 구조를 이룹니다.

* `a`
    * `b`
    * `c`

* 이렇게 하면 다음과 같이 총 3개의 패키지가 존재하게 됩니다.
    * `a`, `a.b`, `a.c`

* 계층 구조상 `a` 패키지 하위에 `a.b` 패키지와 `a.c` 패키지가 있습니다.
    * 그런데 이것은 우리 눈에 보기에 계층 구조를 이룰 뿐입니다.
        * `a` 패키지와 `a.b`,`a.c` 패키지는 서로 완전히 다른 패키지입니다.
            * 따라서 `a` 패키지의 클래스에서 `a.b` 패키지의 클래스가 필요하다면 `import` 해서 사용해야 합니다. 반대도 물론 마찬가지 입니다.

## 정리
* 패키지가 계층 구조를 이루더라도 모든 패키지는 서로 다른 패키지입니다.
    * 물론 사람이 이해하기 쉽게 계층 구조를 잘 활용해서 패키지를 분류하는 것은 좋습니다.
        * 참고로 카테고리는 보통 큰 분류에서 세세한 분류로 점점 나누어집니다.
            * 패키지도 마찬가지입니다.