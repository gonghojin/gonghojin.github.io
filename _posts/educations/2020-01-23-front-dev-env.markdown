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
# 프론트엔드 개발 환경의 이해
## 개발환경을 구성하는데 필요한 도구
+ Node.js
	- 프로젝트 전반에 사용되는 `자바스크립트` 기반 플랫폼
+ Webpack
	- `모듈`로 분리하여 코딩할 수 있게 도와주는 모듈 번들
+ Babel
	- 구형 브라우저에서도 최신 자바스크립트 문법을 사용할 수 있도록 도와주는 도구

### 1. NPM(Node Package Manage)
#### 1.1 왜 프론트엔드 개발에 Node.js가 필요한가?
##### 1.1.1 최신 스펙으로 개발할 수 있다!!
자바스크립트 스펙의 빠른 발전에 비해 브라우저의 지원 속도는 항상 뒤쳐 진다.  
따라서 이것을 구현해 주는 징검다리 역할, 즉 바벨과 같은 도구의 도움없이는 부족하다.  
더불어 웹펙, NPM 같은 Node 기술로 만들어진 환경에서 사용할 때 비로소 자동화된 프론트엔드 개발환경을 갖출 수 있다.

##### 1.1.2 빌드 자동화!!
과거처럼 코딩 결과물을 브라우저에 바로 올리는 경우는 흔치 않다.  
파일을 압축하고, 코드를 난독화하고, 폴리필을 추가하는 등 개발 이외의 작업을 거친 후 배포한다.  
Node.js는 이러한 빌드 과정을 자동화하는데 도와주는 역할을 할뿐만 아니라, 라이브러리 의존성을 해결하고, 각종 테스트를 자동화하는데도 사용된다.

##### 1.1.3 개발 환경 커스터마이징
각 프레임워크에서 제공하는 도구를 사용하면 손쉽게 개발환경을 갖출 수 있다.(React에서 CRA(create-react-app)  
그러나 개발 프로젝트에 따라 툴을 그래도 사용할 수 없는 경우도 빈번하기 때문에, 커스터마이징하거나 또는 자체적으로 환경을 구축해야 한다.  
이러한 배경하에 Node.js는 프론트엔드 개발에서 필수 기술로 자리매김하고 있다.

### 1.2 프로젝트 초기화
개발 프로젝트는 외부 라이브러리를 다운로드 받고 빌드하는 등 `일련의 명령어를 자동화`하여 프로젝트를 관리하는 도구가 존재한다.  
자바의 Gradle, 자바스크립의 NPM이 같은 툴이 이에 해당한다.  

#### 1.2.1 Init
NPM은 다양한 하위 명령어를 제공하며, 그 중 Init 명령어를 통해 프로젝트를 생성한다.
~~~
$ npm init

package name: (front)
version: (1.0.0)
description:
entry point: (index.js)
test command:
git repository:
keywords:
author:
license: (ISC)
~~~
프로젝트 설정에 대한 몇 가지 질문이 이루어지며, 빈 칸으로 남겨둘 시 기본값 설정으로 `package.json` 파일이 생성된다.  
##### Package.json
~~~
{
  "name": "front",
  "version": "1.0.0",
  "description": "",
  "main": "index.js", // 노드 어플리케이션일 경우 진입점 경로, 프론트엔드 프로젝트일 경우 사용하지 않는다.
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}

~~~

#### 1.2.2 프로젝트 명령어
생성한 프로젝트는 package.json에 등록한 `스크립트(script)`를 사용해 실행된다.  
어플리케이션 빌드, 테스트, 배포, 실행 따위의 명령어를 등록하여 사용한다.
~~~
❯ npm test

> echo "Error: no test specified" && exit 1

Error: no test specified
npm ERR! Test failed.  See above for more details.
~~~
Package.json에 등록한 쉘 스크립트 코드가 실행된다.  
또한 NPM에서 사용할 수 있는 명령어 외에 명령어를 추가할 수 있으며,
~~~
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "사용자 정의 명령어"
  },
~~~ 
커스텀으로 등록한 명령어는 아래 형식으로 run을 추가해서 실행한다.
~~~
❯ npm run build

> 사용자 정의 명령어
~~~

### 1.3 패키지 설치
외부 라이브러리를 가져다 쓰는 것은 무척 자연스러운 일이며, CDN(컨텐츠 전송 네트워크), 직접 다운로드 등 다양한 수단이 있다.  
하지만 위와 같은 방법은 단점들이 존재하며, 이를 보완한 방법이 `NPM을 사용하여 패키지를 관리`하는 방식이다.  
이 방식은 라이브러리를 어느 한 곳에서 업데이트하고 하위 호환되는 안전한 버전만 다운받아서 사용할 수 있다.    
NPM은 다음과 같은 방식으로 패키지를 관리하며, 이는 최신 버전의 react를 NPM 저장소에서 찾아 우리 프로젝트로 다운하는 명령어이다.
~~~
❯ npm install react
~~~
package.json에는 설치한 패키지 정보를 기록한다.
~~~
 "dependencies": {
    "react": "^16.12.0"
  }
~~~

### 2. WebPack(웹팩)
#### 2.1 왜 프론트엔드 개발에 WebPack이 필요한가?
최근 들어 프론트 엔드 프로젝트의 규모가 커짐에 따라, Javascript 코드를 `여러 파일과 폴더에 나누어 작성`하고 서로가 서로를 효율적으로 불러올 수 있도록 해주는 모듈화된 환경으로 개발이 이루어진다.  
하지만 `모든 브라우져에서 모듈 시스템을 지원하지는 않는다.` 따라서 브라우져에 무관하게 모듈을 사용하기 위해 나온 것이 `웹팩이다.`

#### 2.2 엔트리 / 아웃풋
웹팩은 여러 개 파일을 하나의 파일로 합쳐주는 번들러(Bundler)다.  
하나의 시작점(Entry point)에서 의존적인 모듈을 전부 찾아내서 하나의 결과물을 만들어 낸다.
![https://webpack.js.org/](/assets/educations/images/babel.png)  

간단히 웹팩으로 번들링 작업을 해보자.
번들 작업을 하는 `webpack` 패키지와 웹팩 터미널 도구인 `webpack-cli`를 설치한다.  
~~~
❯ yarn add -D webpack webpack-cli
~~~
설치가 완료되면 `node_modules/.bin` 폴더에 실행 가능한 명령어가 몇 개 생긴다.  
(webpack 또는 webpack-cli를 실행하면 된다.) --help 옵션으로 사용 방법을 확인해 보자.  
~~~
❯ ./node_modules/.bin/webpack --help
~~~
이 중 --mode, --entry, --output 세 개 옵션만으로 번들링을 할 수 있다.  
+ --mode
	+ 웹팩 실행 모드는 의미하는데, 개발 버전인 development를 지정한다.
+ --entry
	+ 시작점 경로를 지정하는 옵션
+ --output
	+ 번들링 결과물을 위치할 경로
