---
layout: post
comments: true
title:  "프론트엔드 개발환경 이해(김정환님 세미나)"
date:   2020-01-22 17:00:00
author: Gongdel
categories: Seminar
tags:	 Front Environment NPM Babel Webpack
cover:  "/assets/instacode.png"
---
# 프론트엔드 개발 환경의 이해 2(Lint, 웹팩 심화)
## 개발환경을 구성하는데 필요한 도구
+ Lint(린트)
	- 코드에서 발견된 문제 패턴을 식별하기 위한 정적 코드 분석 도구
## 1. ESLint
### 1.1 기본 개념
ESLint는 ECMAScript 코드에서 문제점을 검사하고 일부는 더 나은 코드로 정정하는 린트 도구 중의 하나다.  
`코드의 가독성을 높이고, 잠재적인 오류와 버그를 제거해 단단한 코드를 만드는 것이 목적이다.`  
코드에서 검사하는 항목을 크게 분류하면
+ 포맷팅
	- 일관된 코드 스타일을 유지하도록 하고, '들여쓰기 규칙', '코드라인 최대 너비 규칙' 등을 통해 코드의 가독성을 높여준다.
+ 코드 품질
	- 어플리케이션의 잠재적인 오류나 버그를 예방하기 위함
		- 사용하지 않는 변수 제거, 글로벌 스코프 함수로 다루지 않기 등

### 1.2. 규칙(Rules)
먼저 노드 패키지로 제공되는 ESLint 도구를 다운로드한다.
~~~
❯ npm i -D eslint
~~~

ESLint는 검사 규칙을 미리 정해 놓았다. [규칙 목록](https://eslint.org/docs/rules/)
설정 파일의 rules 객체에 규칙을 추가해 보자.  
##### .eslintrc.js
~~~
module.exports = {
  rules: {
    "no-unexpected-multiline": "error"
  }
}
~~~
규칙에 설정하는 값은 세 가지이다.  
+ off 또는 0 - 끔
+ warn 또는 1 - 경고
+ error 또는 2 - 오류

### 1.2.1 Extensible Config
추천하는 규칙을 여러 개 미리 정해 놓은 것이 eslint:recommended 설정이다. [규칙 목록](https://eslint.org/docs/rules/)에서 왼쪽에 체크 표시되어 있는 것이 이 설정에서 활성화되어 있는 규칙이다.  

이것을 사용하려면 extends 설정을 추가한다.
##### .eslintrc.js
~~~
module.exports = {
  extends: [
    "eslint:recommended", // 미리 설정된 규칙 세트을 사용한다
  ], 
}
~~~
만약 이 설정 외에 규칙이 더 필요하다면 rules 속성에 추가해서 확장할 수 있다.  
ESLint에서 기본으로 제공하는 설정 외에 자주 사용하는 두 가지가 있다.
+ airbnb
	- [airbnb 스타일 가이드](https://github.com/airbnb/javascript)를 따르는 규칙 모음
	- [eslint-config-airbnb-base](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb-base) 패키지로 제공
+ standard
	- [자바스크립트 스탠다드 스타일](https://standardjs.com/)를 따르는 규칙 모음
  - [eslint-config-standard](https://github.com/standard/eslint-config-standard) 패키지로 제공

### 1.3 초기화
이러한 설정은 --init 옵션을 추가하면 손쉽게 구성할 수 있다.
~~~
❯ npx eslint --init
? How would you like to use ESLint? To check syntax, find problems, and enforce code style
? What type of modules does your project use? JavaScript modules (import/export)
? Which framework does your project use? React
? Does your project use TypeScript? No
~~~
대화식 명령어로 진행되며, 답변에 따라 .eslintrc 파일을 자동으로 생성한다.

## 3. Prettier 
프리티어는 코드를 "더" 예쁘게 다듬는다. ESLint의 역할 중 포매팅과 겹치는 부분이 있지만, 프리티어는 좀 더 일관적인 스타일로 코드를 다듬는다.  
또한 코드 품질과 관련된 기능을 하지 않는 것이 ESLint와 다른 점이다.

## 3.1 설치 및 사용
프리티어 패키지를 다운로드 하고
~~~
❯ npm i -D prettier
~~~
코드를 아래처럼 작성 후, 
##### app.js
~~~
console.log('hello world')
~~~
Pretiier로 검사를 해보자. --write 옵션을 추가하면 파일을 재작성한다. 그렇지 않을 경우 결과를 터미널에 출력한다.
~~~
npx prettier app.js --write

// app.js
console.log("hello world");
~~~
작은 따옴표 큰 따옴표로 변환됐으며, 세미콜론도 추가됐다. 프리티어는 ESLint와 달리 규칙이 미리 세팅되어 있기 때문에 설정없이도 바로 사용할 수 있다.  
또한 ESLint는 규칙에 의해 코드를 검사하고 결과만 알려주면, 수정하는 것은 개발자의 몫이다. 반면 프리티어는 어떻게 수정해야할지 알고 있기 때문에 코드를 다시 작성해 준다.  

## 3.2 통합방법
여전히 ESLint를 사용해야 하는 이유를 남아 있다. 포맷팅은 프리티어에 맡기더라도, 코드 품질과 관련된 검사는 ESLint의 몫이기 때문이다.  
따라서 이 둘을 같이 사용하는 것이 최선이다. 프리티어는 이러한 ESLint와 통합 방법을 제공한다.  
eslint-config-prettier 는 프리티어와 충돌하는 ESLint 규칙을 끄는 역할을 한다. 둘 다 사용하는 경우 규칙이 충돌하기 때문이다.
