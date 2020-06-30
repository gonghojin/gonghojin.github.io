---
layout: post
comments: true
title:  "프론트엔드 개발환경 이해2"
date:   2020-05-28 17:00:00
author: Gongdel
categories: Seminar
tags:	 Front Environment NPM Babel Webpack
cover:  "/assets/instacode.png"
---
# 프론트엔드 개발 환경의 이해 2(Lint, 웹팩 심화)
## 개발환경을 구성하는데 필요한 도구
+ Lint(린트)
	- 코드에서 발견된 문제 패턴을 식별하기 위한 정적 코드 분석 도구

---

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
    "parserOptions": {
        "ecmaVersion": 2018,
        "sourceType": "module"
    },
    "rules": {
        "no-extra-semi": "error", // 연속 세미콜론 제거
    }
};
~~~
규칙에 설정하는 값은 세 가지이다.  
+ off 또는 0 - 끔
+ warn 또는 1 - 경고
+ error 또는 2 - 오류

사용법  
규칙을 위반 코드 위치와 규칙명을 제공
~~~
❯ npx eslint app.js
~~~
규칙을 위반한 코드를 자동으로 수정
~~~
❯ npx eslint app.js --fix
~~~
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

---

## 2. Prettier 
프리티어는 코드를 "더" 예쁘게 다듬는다. ESLint의 역할 중 포매팅과 겹치는 부분이 있지만, 프리티어는 좀 더 일관적인 스타일로 코드를 다듬는다.  
또한 코드 품질과 관련된 기능을 하지 않는 것이 ESLint와 다른 점이다.

## 2.1 설치 및 사용
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

## 2.2 통합방법
여전히 ESLint를 사용해야 하는 이유를 남아 있다. 포맷팅은 프리티어에 맡기더라도, 코드 품질과 관련된 검사는 ESLint의 몫이기 때문이다.  
따라서 이 둘을 같이 사용하는 것이 최선이다. 프리티어는 이러한 ESLint와 통합 방법을 제공한다.  
eslint-config-prettier 는 프리티어와 충돌하는 ESLint 규칙을 끄는 역할을 한다. 둘 다 사용하는 경우 규칙이 충돌하기 때문이다.
패키지를 설치한 뒤,
~~~
❯ npm i -D eslint-config-prettier
~~~
##### eslintrc.js
설정파일의 extends 배열에 추가한다.
~~~
module.exports = {
    extends: [
        "eslint:recommended", // 미리 설정된 규칙 세트을 사용한다
        "eslint-config-prettier"
    ],
}
~~~
+ eslint-config-prettier
	- ESLint는 중복 세미콜론 사용을 검사하고, 프리티어도 마찬가지다.  
	따라서 어느 한쪽에서는 규칙을 꺼야 하는데, eslint-config-prettier를 extends하면 중복되는 ESLint 규칙을 비활성화한다.  

ESLint는 중복된 포매팅 규칙을 프리티어에게 맞기고, 나머지 코드 품질에 관한 검사만 한다.  
따라서 아래처럼 두 개를 동시에 실행해서 코드를 검사한다.
~~~
❯ npx prettier app.js --write && npx eslint app.js --fix

1:5  error  'foo' is assigned a value but never used  no-unused-vars

✖ 1 problem (1 error, 0 warnings)
~~~
프리티어에 의해 코드가 아래와 같이 포매팅 되고, ESlint에 의해 코드 품질과 관련된 오류(no-unused-vars)를 리포팅한다.
~~~
var foo = '' // 사용하지 않은 변수. ESLint가 검사
console.log();;;;;;; // 중복 세미콜론 사용. 프리티어가 자동 수정

=============

var foo = "";
console.log();
~~~

한편, [eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier)는 프리티어 규칙을 ESLint 규칙으로 추가하는 플러그인이다.  
프리티어의 모든 규칙이 ESLint로 들어오기 때문에 ESLint만 실행하면 된다.  

패키지를 설치하고,
~~~
❯ npm i -D eslint-plugin-prettier
~~~
설정 파일에서 plugins와 rules에 설정을 추가한다.
##### eslintrc.js
~~~
{
  plugins: [
    "prettier"
  ],
  rules: {
    "prettier/prettier": "error"
  },
}
~~~
프리티어의 모든 규칙을 ESLint 규칙으로 가져온 설정이다. 이제는 ESLint만 실행해도 프리티어 포매팅 기능을 가져올 수 있다.  
~~~
❯ npx eslint app.js --fix
~~~
프리티어는 이 두 패키지를 함게 사용하는 [단순한 설정](https://prettier.io/docs/en/integrating-with-linters.html)을 제공하는데 아래 설정을 추가하면 된다.  
##### eslintrc.js
~~~
{
  "extends": [
    "eslint:recommended",
    "plugin:prettier/recommended"
  ]
}
~~~

---
## 4. 웹팩(심화)
이전 글에서는 웹팩의 개념과 간단한 사용법에 대해 살펴보았다. 웹팩은 프론트엔드 개발 서버를 제공하고, 몇 가지 방법으로 빌드 결과를 최적화할 수 있는데 알아보자.  
## 4.1. 웹팩 개발 서버
인터넷에 웹사이트를 게시하려면 서버 프로그램으로 파일을 읽고 요청한 클라이언트에게 제공해야 한다.  
개발 환경에서도 이와 유사한 환경을 갖추는 것이 좋다. 운영 환경과 맞춤으로써 배포 시 잠재적 문제를 미리 확인할 수 있다.  
게다가 ajax 방식의 api 연동은 cors 정책 때문에 반드시 서버가 필요하다.  

프론트엔드 개발환경에서 이러한 개발용 서버를 제공해 주는 것이 [webpack-dev-server](https://webpack.js.org/configuration/dev-server/)

## 4.1.1. 설치 및 사용
먼저 webpack-dev-server 패키지를 설치한다.  
~~~
❯ npm i -D webpack-dev-server
~~~
node_modules/.bin에 있는 webpack-dev-servr 명령어를 바로 실행해도 되지만, npm 스크립트를 등록해서 사용해 보자.
##### package.json
~~~
{
	"scripts": {
		"start": "webpack-dev-server"
	}
}
~~~
npm start 명령어로 실행하면 다음과 같이 서버가 구동되었다는 메시지를 확인할 수 있다.
~~~
> npm start

> webpack-dev-server

ℹ ｢wds｣: Project is running at http://localhost:8080/
ℹ ｢wds｣: webpack output is served from /
ℹ ｢wds｣: Content not from webpack is served from 
~~~
로컬 호스트의 8080 포트에 서버가 구동되어서 접속을 대기하고 있다. 웹팩 아웃풋인 dist 폴더는 루트 경로를 통해 접속할 수 있다.  
브라우저 주소창에 http://localhost:8080/으로 접속해 보면 결과물을 확인할 수 있다.  

소스 코드를 수정하고 저장하면, 웹팩 서버가 파일 변화를 감지하여 웹팩 빌드를 다시 수행하고 브라우저를 리플레시하여 변경된 결과물을 보여준다. 이것만으로도 개발 환경이 무척 편리해 질 것이다.  

## 4.1.2. 기본 설정
웹팩 설정 파일의 devServer 객체에 개발 서버 옵션을 설정할 수 있다.  
##### webpack.config.js
~~~
module.exports = {
  devServer: {
      contentBase: path.join(__dirname, "dist"), // 정적파일을 제공할 경로, 기본값은 웹팩 아웃풋이다.
      publicPath: "/", // 브라우저를 통해 접근하는 경로, 기본값은 ‘/’이다.
      host: "dev.domain.com",
      overlay: true, // 빌드 시 에러나 경고를 부라우저 화면에 표시한다.
      port: 8080, // 개발 서버 포트 번호를 설정, 기본값은 8080
      starts: "error-only", // 메시지 수준을 정할 수 있다
      historyApiFallback: true,
    }
}
~~~
+ contentBase
	- 정적파일을 제공할 경로, 기본값은 웹팩 아웃풋이다.
+ publicPath
	- 브라우저를 통해 접근하는 경로, 기본값은 '/'이다.
+ host
	- 개발 환경에서 도메인을 맞춰야 하는 상황에서 사용한다.  
	예를들어, 쿠기 기반의 인증은 인증 서버와 동일한 도메인으로 개발 환경을 맞춰야 한다.  
+ overlay
	- 빌드 시 에러나 경고를 부라우저 화면에 표시한다.  
+ port
	- 개발 서버 포트 번호를 설정, 기본값은 8080
+ starts
	- 메시지 수준을 정할 수 있다.
		- 'none’, ‘errors-only’, ‘minimal’, ‘normal’, ‘verbose’ 로 메세지 수준을 조절한다.
+ historyApiFallBack
	- 히스토리 API를 사용하는 SPA 개발 시 설정한다.  
	404가 발생하면 index.html로 리다이렉트한다.  
	
이 외에도 개발 서버를 실행할 때 명령어 인자로 --progress를 추가하면 빌드 진행률을 보여준다.  
빌드 시간이 길어질 경우 사용하면 좋다.  
그밖의 옵션은 [여기](https://webpack.js.org/configuration/dev-server/)를 참조하자

## 4.2. API 연동
## 4.2.1 실제 API 연동: devServer.proxy
localhost:8080에서 localhost:8081 호출할 경우, 즉 다른 서버로 호출할 경우 CORS(Cross Origin Resource Sharing) 정책에 위반된다.  
CORS 브라우저와 서버간의 보안상의 정책인데 브라우저가 최초로 접속한 서버에서만 요청을 할 수 있다.  

이 문제를 해결하는 방법은 두 가지인데, 그중 프론트엔드 측에서 해결하는 방법을 알아보자.  
웹팩 개발 서버에서 api 서버로 `프록싱`하는 방법으로, 웹팩 개발 서버는 proxy 속성으로 이를 지원한다.
#####
~~~
// webpack.config.js
module.exports = {
  devServer: {
    proxy: {
      '/api': 'http://localhost:8081', // 프록시
    }
  }
}
~~~
개발 서버에 들어온 모든 http 요청 중 '/api'로 시작되는 것은 'http://localhost:8081'로 요청하는 설정이다.
