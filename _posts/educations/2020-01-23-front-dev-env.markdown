---
layout: post
comments: true
title:  "프론트엔드 개발환경 이해1"
date:   2020-05-21 17:00:00
author: Gongdel
categories: Seminar
tags:	 Front Environment NPM Babel Webpack
cover:  "/assets/instacode.png"
---
# 프론트엔드 개발 환경의 이해 1(NPM, 웹팩, Babel)
## 사전 셋팅
1. 실습 베이스 소스 Fork하기
    - https://github.com/gonghojin/lecture-frontend-dev-env
    
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

---

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
~~~
❯ node_modules/.bin/webpack --mode development --entry ./src/app.js --output dist/main.js
~~~
+ --mode
	+ 웹팩 실행 모드는 의미하는데, 개발 버전인 development를 지정한다.
+ --entry
	+ 시작점 경로를 지정하는 옵션
+ --output
	+ 번들링 결과물을 위치할 경로

또한 옵션 중 --config 통해 웹팩 설정 파일의 경로를 지정할 수 있다.  
기본 파일명은 webpack.config.js 혹은 webpackfile.js이며, 다음과 같이 사용할 수 있다.
##### webpack.config.js
다음과 같이 webpack.config.js 설정을 통해, 위의 터미널에서 사용한 옵션을 대체할 수 있다.
~~~
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    main: './src/app.js'
  },
  output: {
    filename: '[name].js',
    path: path.resolve('./dist'),
  },
}
~~~
+ output에 설정한 '[name]'은 entry에 추가한 main이 문자열로 들어오는 방식이다.  
	+ output.path에는 절대 경로를 사용하기 떄문에 path  모듈의 resolve() 함수를 사용해서 계산했다. 		
	(path는 노드 코어 모듈 중 하나로 경로를 처리하는 기능을 제공)  
	
웹팩 실행을 위한 NPM 커스텀 명령어를 추가한다.
##### package.json
~~~
{
  "scripts": {
    "build": "./node_modules/.bin/webpack"
  }
}
~~~
모든 옵션을 웹팩 설정 파일로 옮겼기 때문에 단순히 webpack 명령어만 실행한다. 이제부터는 npm run build로 웹팩 작업을 지시할 수 있다.

---
* LAB 1 - 웹팩 엔트리 / 아웃풋 실습:   
 <https://github.com/gonghojin/lecture-frontend-dev-env/tree/1-webpack/1-entry>
 
### 3. 로더
#### 3.1 로더의 역할
웹팩은 자바스크립트로 만든 모듈뿐만 아니라, 스타일시트, 이미지, 폰트 등 `모든 파일을 모듈로 바라본다.`  
따라서 import 구문을 사용하면 자바스크립트 코드 안으로 가져올 수 있다.  

이것을 가능하게 해주는 것이 바로 `웹팩의 로더`이다.  
로더는 타입스크립트 같은 다른 언어를 자바스크립트 문법으로 변환해 주거나 이미지를 data Url 형식의 문자열로 변환한다.  
뿐만 아니라 CSS 파일을 자바스크립에서 직접 로딩할 수 있도록 해준다.

#### 3.2 자주 사용하는 로더
#### 3.2.1 css-loader
웹팩은 모든 것을 모듈로 바라보기 때문에 스타일시트도 import 구분으로 불러올 수 있다.  
##### app.js
~~~
import './style.css'
~~~
##### style.css
~~~
body {
  background-color: green;
}
~~~
하지만 CSS 파일을 자바스크립트에서 불러와 사용하려면 CSS를 모듈로 변환하는 작업이 필요하다.  
그러한 역할을 해주 것이 `css-loader`이다. 먼저 로더를 설치하자.  
~~~
❯ npm install -D css-loader
~~~
웹팩 설정에 로더를 추가하자.  
##### webpack.config.js
~~~
module.exports = {
  module: {
    rules: [{
      test: /\.css$/, // .css 확장자로 끝나는 모든 파일 
      use: ['css-loader'], // css-loader를 적용한다, 경로대신 배열에 문자열로 전달해도 가능
    }]
  }
}
~~~
웹팩은 엔트리 포인트부터 시작해서 모듈을 검색하다 CSS 파일을 찾으면 css-loader로 처리한다.    

빌드한 결과, CSS코드가 자바스크립토 변환된 것을 확인할 수 있다.  
##### main.js
~~~
....."body {\r\n\tbackground-color: green;\r
~~~

#### 3.2.1 style-loader
모듈로 변경된 스타일시트는 DOM에 추가되어야만 브라우저가 해석할 수 있다.  
css-loader로 처리하면 자바스크립트 코드로만 변경되었을 뿐 DOM에 적용되진 않는다.  
따라서 `style-loader`를 통해 자바스크립트로 변경된 스타일시트를 동적으로 DOM에 추가한다.  
즉, CSS를 번들링하기 위해서는 `css-loader`와 `style-loader`를 함께 사용해야 한다.  

먼저 스타일 로더를 다운로드 한 후, 웹팩 설정에 로더를 추가한다.
~~~
❯ npm install -D style-loader
~~~
##### webpack.config.js
~~~
module.exports = {
  module: {
    rules: [{
      test: /\.css$/,
      use: ['style-loader', 'css-loader'], // style-loader를 앞에 추가한다 
    }]
  }
}
~~~

#### 3.2.2 file-loader
소스 코드에서 사용하는 모든 파일을 모듈로 사용하게끔 할 수 있다.  
파일을 모듈 형태로 지원하고 웹팩 아웃풋에 파일을 옮겨주는 것이 `file-loader`가 하는 일이다.  
예를들어, CSS에서 url() 함수에 이미지 파일 경로를 지정할 수 있는데, 웹팩은 file-loader를 사용해서 이 파일을 처리한다.
##### style.css
~~~
body {
  background-image: url(bg.png);
}
~~~

웹팩은 엔트리 포인트인 app.js가 로딩하는 style.css 파일을 읽는다. 그리고 이 스타일시트는 url() 함수로 bg.png를 사용하는데 이때 로더를 동작시킨다.  
##### webpack.config.js
~~~
module.exports = {
  module: {
    rules: [{
      test: /\.png$/, // .png 확장자로 마치는 모든 파일
      loader: 'file-loader', // 파일 로더를 적용한다
    }]
  }
}
~~~ 
웹팩이 .png 파일을 발견하면 file-loader를 실행한다. 로더가 동작하고 나면 아웃풋에 설정한 경로로 이 파일이 복사된다.  
(아래 그림처럼 파일명이 해쉬코드로 변경되는데, 캐쉬 갱신을 위한 처리로 보인다.)  
![https://webpack.js.org/](/assets/educations/images/file_loader_1.png)  

하지만 이대로 index.html 파일을 브라우저에 로딩하면 이미지를 제대로 로딩하지 못한다.  
CSS를 로딩하면 background-image: url(bg.png) 코드에 의해 동일 폴더에서 이미지를 찾으려고 시도하지만, 웹팩으로 빌드한 이미지 파일은 output인 dist 폴더 아래로 이동했기 때문이입니다.
따라서 file-loader 옵션을 조정해서 경로를 잡아주어야 한다.

##### webpack.config.js
~~~
module.exports = {
  module: {
    rules: [{
      test: /\.png$/, // .png 확장자로 마치는 모든 파일
      loader: 'file-loader',
      options: {
        publicPath: './dist/', // prefix를 아웃풋 경로로 지정 
        name: '[name].[ext]?[hash]', // 파일명 형식 
      }
    }]
  }
}
~~~

+ publicPath
	+ file-loader가 처리하는 파일을 모듈로 사용할 때 경로 앞에 추가되는 문자열  
		+ output에 설정한 'dist' 폴더에 이미지 파일을 옮기므로, 해당 값을 지정
		+ 'bg.png'를 'dist/bg.png'로 변경하여 사용
+ name
	+ loader가 파일을 아웃풋에 복사할 때 사용할 파일 이름
	+ 기본적으로 설정된 해쉬값을 쿼리스트링으로 옮겨서 'bg.png?070980c4b3edb776872e80d29760490b' 형식으로 파일을 요청

#### 3.2.4 url-loader
사용하는 이미지 갯수가 많다면 네트웍 리소스를 사용하는 부담이 있고, 사이트 성능에 영향을 줄 수도 있다.  
만약 한 페이지에서 작 이미지를 여러 개 사용한다면 `Data URI Scheme`를 사용하는 것이 더 낫다.  
이미지를 Base64로 인코딩하여 문자열 형태로 소스코드에 넣는 형식이. url-loader는 이러한 처리를 자동화 해준다.  

##### webpack.config.js
~~~
{
  test: /\.png$/,
  use: {
    loader: 'url-loader', // url 로더를 설정한다
    options: {
      publicPath: './dist/', // file-loader와 동일
      name: '[name].[ext]?[hash]', // file-loader와 동일
      limit: 5000 // 5kb 미만 파일만 data url로 처리 
    }
  }
}
~~~

+ limit
	+ 모듈로 사용한 파일 중 크기가 '5kb 미만'인 파일만 url-loader를 적용하는 설정
		+ 만약 이보다 크면 file-loader가 처리하는데 옵션 중 'fallback' 기본 값이 file-loader이기 때문  

아이콘처럼 `용량이 작거나 사용 빈도가 높은 이미지는` 파일을 그대로 사용하기 보다는 Data URI Scheme를 적용하기 위해 url-loader를 사용하면 좋다!!

* LAB 2 - 웹팩 로더:   
<https://github.com/jeonghwan-kim/lecture-frontend-dev-env/tree/1-webpack/2-loader>

### 4. 플러그인
### 4.1 플러그인의 역할
로더가 `파일 단위`로 처리하는 반면, 플러그인은 `번들된 결과물`을 처리한다.  
번들된 자바스크립트를 난독화한다거나 특정 테스트를 추출하는 용도로 사용한다.  

### 4.2 자주 사용하는 플러그인
### 4.2.1 BannerPlugin
BannerPlugin은 결과물에 빌드 정보나 커밋 버전 같은 걸 추가할 수 있다.

##### webpack.config.js
~~~
const webpack = require('webpack');

module.exports = {
  plugins: [
    new webpack.BannerPlugin({
      banner: '이것은 배너 입니다',
    })
  ]
}
~~~
생성자 함수에 전달하는 옵션 객체의 banner 속성에 문자열을 전달한다.  
웹팩 컴파일 타임에 얻을 수 있는 정보(예: 빌드시간, 커밋정보)를 전달하기 위해 함수로 전달할 수도 있다.  
~~~
new webpack.BannerPlugin({
  banner: () => `빌드 날짜: ${new Date().toLocaleString()}`
})
~~~
만약 배너 정보가 많다면 별도 파일로 분리해서 사용하자.
~~~
const banner = require('./banner.js');

new webpack.BannerPlugin(banner);
~~~

빌드 날짜 외에 커밋 해쉬와 빌드한 유저 정보까지 추가해보자
##### banner.js
~~~
const childProcess = require('child_process');

module.exports = function banner() {
  const commit = childProcess.execSync('git rev-parse --short HEAD')
  const user = childProcess.execSync('git config user.name')
  const date = new Date().toLocaleString();
  
  return (
    `commitVersion: ${commit}` +
    `Build Date: ${date}\n` +
    `Author: ${user}`
  );
}
~~~

##### 빌드한 뒤 플로그인이 처리한 결과
![https://webpack.js.org/](/assets/educations/images/plugin_1.png)  

### 4.2.2 DefinePlugin
어플리케이션은 개발환경과 운영환경으로 나눠서 운영되는데, 환경에 따라 API 서버 주소가 다를 수도 있다.  
같은 소스 코드를 두 환경에 배포하기 위해서는 이러한 환경 의존적인 정보를 소스가 아닌 곳에서 관리하는 것이 좋다.  
배포할 때마다 코드를 수정하는 것은 곤란하기 때문이다.  

웹팩은 이러한 환경 정보를 제공하기 위해 DefinePlugin을 제공한다.
##### webpack.config.js
~~~
const webpack = require('webpack');

export default {
  plugins: [
    new webpack.DefinePlugin({}),
  ]
}
~~~
빈 객체를 전달해도 기본적으로 넣어주는 값이 있다. 노드 환경 정보인 process.env.NODE_ENV인데 웹팩 설정의 mode에 설정한 값이 여기에 들어간다.  
'development'를 설정했기 때문 어플리케이션 코드에서 process.env.NODE_ENV 변수로 접근하면 'development' 값을 얻을 수 있다.

##### app.js
~~~
console.log(process.env.NODE_ENV) // "development"
~~~
이 외에도 웹팩 컴파일 시간에 결정되는 값을 전역 상수 문자열로 어플리케이션에 주입할 수 있다.  
~~~
new webpack.DefinePlugin({
  TWO: '1+1',
})
~~~
TWO라는 전역 변수에 1+1 이란 코드 조각을 넣었다. 실제 어플리케이션 코드에서 이것을 출력해보면 2가 나올 것이다.  

코드가 아닌 값을 입력하려면 문자열화한 뒤 넘기자.
~~~
new webpack.DefinePlugin({
  VERSION: JSON.stringify('v.1.2.3'),
  PRODUCTION: JSON.stringify(false),
  MAX_COUNT: JSON.stringify(999),
  'api.domain': JSON.stringify('http://dev.api.domain.com'),
})
~~~

##### app.js
~~~
console.log(VERSION) // 'v.1.2.3'
console.log(PRODUCTION) // false
console.log(MAX_COUNT) // 999
console.log(api.domain) // 'http://dev.api.domain.com'
~~~
빌드 타임에 결정된 값을 어플리이션에 전달할 때는 이 플러그인을 사용하자.

### 4.2.3 HtmlTemplatePlugin
HtmlTemplatePlugin은 HTML 파일을 후처리하는 데 사용한다. 빌드타임의 값을 넣거나 코드를 압축할 수 있다.  

index.html 파일을 src/index.html로 옮긴 뒤, 다음과 같이 작성해 보자.
##### src/index.html
~~~
<!DOCTYPE html>
<html>
  <head>
    <title>타이틀<%= env %></title>
  </head>
  <body>
    <!-- 로딩 스크립트 제거 -->
    <!-- <script src="dist/main.js"></script> -->
  </body>
</html>
~~~
 
타이틀 부분에 EJS 문법을 사용하여 전달받은 env 변수 값을 출력한다.  
HtmlTemplatePlugin은 이 변수에 데이터를 주입시켜 동적으로 HTML 코드를 생성한다.  
뿐만 아니라 웹팩으로 빌드한 결과물을 자동으로 로딩하는 코드를 주입해 준다.  
따라서 로딩 스크립트 코드는 제거해도 무방하다.  

##### webpack.config.js
~~~
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports {
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html', // 템플릿 경로를 지정 
      templateParameters: { // 템플리셍 주입할 파라매터 변수 지정
        env: process.env.NODE_ENV === 'development' ? '(개발용)' : '', 
      },
    })
  ]
}
~~~

### 4.2.4 CleanWebpackPlugin
CleanWebpackPlugin은  `빌드 이전 결과물`을 제거하는 플러그인이다.  
빌드 결과물은 아웃풋 경로에 모이는데, 과거 파일이 남아 있을 수 있다. 
- 덮여 씌여지는 빌드 내용은 상관없지만, 그렇지 않은 결과물은 불필요하게 남아 있게 된다.  

사용을 위해선 먼저 패키지를 설치한다.
~~~
❯ npm install -D clean-webpack-plugin
~~~

그후 웹팩에 설정을 추가한다.
~~~
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
  plugins: [
    new CleanWebpackPlugin(),
  ]
}
~~~

### 4.2.5 MiniCssExtractPlugin
스타일시트가 점점 많아지면 하나의 자바스크립트 결과물로 만드는 것은 부담일 수 있다. 이때 번들 결과에서 스타일시트 코드만 추출해서 별도의 CSS 파일로 만들어 역할에 따라 파일을 분리하는 것이 좋다.  
브라우저에선 큰 파일 하나를 내려 받는 것보다, 여러 개의 작은 파일을 동시에 다운로드하는 것이 더 빠르다.

개발 환경에서는 CSS를 하나의 모듈로 처리해도 상관없지만, 프로덕션 환경에서는 분리하는 것이 효과적이다.   
이런한 역할을 `MiniCssExtractPlugin`이 해준다. CSS 별로 파일로 뽑아내 보자.  

먼저 패키지를 설치한다.  
~~~
❯ npm install -D mini-css-extract-plugin
~~~

그후 웹팩에 설정을 추가한다.
~~~
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  plugins: [
    ...(
      process.env.NODE_ENV === 'production' 
      ? [ new MiniCssExtractPlugin({filename: `[name].css`}) ] // 프로덕션 환경에서만 추가
      : []
    ),
  ],
}
~~~
프로덕션 환경일 경우에만 플러그인을 추가했기 때문에, filename에 설정한 값으로 아웃풋 경로에 CSS 파일이 생성된다.  

개발 환경에서는 css-loader에 의해 자바스크립트 모듈로 변경된 스타일시트를 적용하기 위해 `style-loader`를 사용했다.  
반면 프로덕션 환경에서는 별도의 CSS 파일로 추출하는 플러그인을 적용했으므로 다른 로더가 필요하다.
~~~
module.exports = {
  module: {
    rules: [{
      test: /\.css$/,
      use: [
        process.env.NODE_ENV === 'production' 
        ? MiniCssExtractPlugin.loader  // 프로덕션 환경
        : 'style-loader', 'css-loader' // 개발 환경
      ],
    }]
  }
}
~~~

---
### 5. Babel(바벨)
#### 5.1. 배경
#### 5.1.1 크로스 브라우징
브라우저마다 사용하는 언어가 달라서 프론트엔드 코드는 일관적이지 못할 때가 많다.  
스팩과 브라우저가 개선되고 있지만 여전히 인터넷 익스플로러는 Promise(프로미스)를 이해하지 못한다.   
프론트엔드 개발에서 크로스 브라우징 이슈는 코드의 일관성을 해치고 초심자를 불안하게 만든다.   
이러한 크로스 브라우징의 혼란을 해결해 줄 수 있는 것이 '바벨'이다.   
바벨은 ECMAScript2015+로 작성한 코드를 모든 브라우저에서 동작할 수 있도록 호환성을 지켜준다. (타입스크립트, JSX와 같은 다른 언어도 포함)

#### 5.2. 바벨의 기본 동작
바벨은 `ECMAScript2015 이상의 코드를 적당한 하위 버전으로 바꾸는 것`이 주된 역할이다. 이렇게 바뀐 코드는 인터넷 익스플로러나 구버전 브라우저처럼 최신 자바스크립트 코드를 이해하지 못하는 환경에서도 잘 동작한다.  

바벨은 세 단계로 빌드를 진행한다.  
1. 파싱(Parsing)
	- 코드를 읽고 추상 구문 트리(AST)로 변환하는 단계
		- 이것은 빌드 작업을 처리하기에 적합한 자료구조로, 컴파일러 이론에 사용되는 개념
2. 변환(Tranforming)
	- 추상 구문 트리를 변경하는 단계로, 실제로 코드를 변경하는 작업을 한다.
3. 출력(Printing)
	- 변경된 결과물을 출력

### 5.3 플러그인
바벨은 파싱과 출력만 담당하고, 변환 작업은 `플러그인`에 의해서 처리된다.
### 5.3.1 플로그인 사용하기  
기본적으로 ECMASCript2015로 작성한 코드를 인터넷 익스플로러에서 돌리기 위해, `block-scoping` 플러그인을 사용한다.  
( const, let처럼 블록 스코핑을 따르는 예약어를 함수 스코핑을 사용하는 var로 변경한다. )  

NPM 패키지로 제공하는 플러그인을 설치
~~~
❯ npm install -D @babel/plugin-transform-block-scoping
~~~
설치한 플러그인을 사용 시
~~~
// app.js
const alert = msg => window.alert(msg);
~~~
~~~
❯ npx babel app.js --plugins @babel/plugin-transform-block-scoping

var alert = msg => window.alert(msg);
~~~

또한 인터넷 익스플로러는 화살표 함수도 지원하지 않는데, `arrow-functions` 플러그인을 사용해서 일반 함수로 변경할 수 있다.
~~~
❯ npm install -D @babel/plugin-transform-arrow-functions

npx babel app.js \
  --plugins @babel/plugin-transform-block-scoping \
  --plugins @babel/plugin-transform-arrow-functions
  
var alert = function (msg) {
  return window.alert(msg);
};
~~~

ECMAScript5에서부터 지원하는 엄격 모드를 사용하는 것이 안전하기 때문에, `strict-mode 플러그인`을 사용하여 "use strict" 구문을 추가하자.  
그전에 커맨드라인 명령어가 점점 길어지기 떄문에 설정 파일로 분리하는 것이 낫다.  
웹팩이 webpack.config.js를 기본 설정파일로 사용하듯, 바벨도 babel.config.js를 사용한다.  

프로젝트 루트에 babel.config.js 파일을 아래와 같이 작성하자.
##### babel.config.js
~~~
module.exports = {
	 plugins: [
		"@babel/plugin-transform-block-scoping",
		"@babel/plugin-transform-arrow-functions",
		"@babel/plugin-transform-strict-mode", 
	]
}
~~~
커맨드라인에서 사용한 block-scoping, arrow-functions 플러그인을 설정 파일로 옮겨 plugins 배열에 추가하는 방식이다.  
~~~
❯ npx babel app.js
~~~
비로소 인터넷 익스폴로러에서 안전하게 동작하는 코드로 트랜스파일되었다.

### 5.4 프리셋
ECMAScript2015+으로 코딩할 때 필요한 플러그인을 일일이 설정하는 일은 번거로운 일이다.  
따라서 목적에 맞게 여러가지 플러그인을 세트로 모아놓은 것을 "프리셋"이라 한다.  
### 5.4.1 프리셋 사용하기  
바벨은 목적에 따라 몇 가지 프리셋을 제공한다.  
+ preset-env
	- ECMAScript2015+를 변환할 때 사용
+ preset-flow
+ preset-react
+ preset-typescript  

인터넷 익스플로러 지원을 위해 env 프리셋을 사용해 보자. 
~~~
❯ npm install -D @babel/preset-env
~~~
##### babel.config.js
~~~
module.exports = {
  presets: [
    '@babel/preset-env'
  ]
}
// 위에서 사용한 플러그인(3개)를 위의 설정으로 대체할 수 있다.
~~~

### 5.4.2 타깃 브라우저
우리 코드가 크롬 최신 버전(2019년 12월 기준)만 지원한다면, 인터넷 익스플로러를 위한 코드 변환은 불필요하다.  
target 옵션에 브라우저 버전명만 지정하면, env 프리셋은 이에 맞는 플러그인들을 찾아 최적의 코드를 출력한다.  
##### babel.config.js 
~~~
module.exports = {
  presets: [
    [
      '@babel/preset-env',
      {
        targets: {
          chrome: '79', // 크롬 79까지 지원하는 코드를 만든다
        }
      }
    ]
  ]
}
~~~
크롬은 블록 스코핑과 화살표 함수를 지원하기 때문에 코드를 변환하지 않고 이러한 결과물을 만들었다.  
만약 인터넷 익스플로러도 지원해야 한다면, 바벨 설정에 브라우저 정보만 하나 더 추가하면 된다.  
##### babel.config.js 
~~~
module.exports = {
  presets: [
    [
      '@babel/preset-env',
      {
        targets: {
          chrome: '79',
          ie: '11' // ie 11까지 지원하는 코드를 만든다
        }
      }
    ]
  ]
}
~~~

### 5.4.2 폴리필
바벨은 ECMAScript2015+를 ECMAScript5 버전으로 변환할 수 있는 것만 빌드한다.  
그렇지 못한 것들은 `폴리필`이 라고 부르는 코드 조각을 추가해서 해결해야 한다.  

예를들면, ECMAScript2015의 블록 스코핑은 ECMASCript5의 함수 스코핑으로 대체할 수 있다. 화살표 함수도 일반 함수로 대체할 수 있다.  
이런 것들은 바벨이 변환해서 ECMAScript5 버전으로 결과물을 만들기 때문이다.  
하지만 Promise는 ECMAScript5 버전으로 대체할 수 없다. 다만 ECMAScript5 버전으로 구현할 수는 있다.  

env 프리셋은 폴리필을 지정할 수 있는 옵션을 제공한다.
##### babel.config.js
~~~
module.exports = {
  presets: [
    [
      '@babel/preset-env',
      {
        useBuiltIns: 'usage', // 폴리필 사용 방식 지정
        corejs: { // 폴리필 버전 지정
          version: 2
        }
      },
    ],
  ],
};
~~~
+ useBuiltIns
	- 어떤 방식으로 폴리필을 사용할지 설정하는 옵션
	- "usage", "entry", false(기본값) 세 가지 값을 사용
		- usage나 entry 설정 시 폴리필 패키지 중 core-js를 모듈로 가져온다.  
	
core-js 패키지에서 Promise 모듈을 가져오는 임포트 구문이 상단에 추가되며, 인터넷 익스폴러에서도 안전하게 돌아간다.
~~~
npx babel src/app.js
"use strict";

require("core-js/modules/es6.promise");
require("core-js/modules/es6.object.to-string");

new Promise();
~~~

## 6. 웹팩으로 통합
실무 환경에서는 바벨을 직접 사용하는 것보다는, 로더 형태로 제공되는 'babel-loader'를 통해 `웹팩으로 통합해서` 사용하는 것이 일반적이다. 
먼저 패키지를 설치하고
~~~
❯ npm install -D babel-loader
~~~ 
웹팩 설정에 로더를 추가한다.  
##### webpack.config.js
~~~
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader', // 바벨 로더를 추가한다 
      },
    ]
  },
}
~~~
.js 확장자로 끝나는 파일은 babel-loader가 처리하도록 설정했다.  
사용하는 써드파티 라이브러리가 많을수록 바벨 로더가 느리게 동작할 수 있기 때문에, node_modules 폴더를 로더가 처리하지 않도록 예외처리 했다.  

폴리필 사용 설정을 했다면 core-js도 설치해야 한다. 웹팩은 바벨 로더가 만든 아래 코드를 만나면 core-js를 찾을 것이기 때문이다.  
~~~
require("core-js/modules/es6.promise");
require("core-js/modules/es6.object.to-string");
~~~
버전 2로 패키지를 추가하자.
~~~
❯ npm i core-js@2
~~~
그리고 웹팩으로 빌드하면 끝~
