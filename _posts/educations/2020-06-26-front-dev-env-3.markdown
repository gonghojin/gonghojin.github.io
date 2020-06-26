---
layout: post
comments: true
title:  "프론트엔드 개발환경 실습 답안"
date:   2020-06-21 17:00:00
author: Gongdel
categories: Seminar
tags:	 Front Environment NPM Babel Webpack
cover:  "/assets/instacode.png"
---
# 1-webpack/1-entry: Lab1.웹팩 엔트리/아웃풋 실습
---
## 1. Lab1 브런치로 이동하기
* /lecture-frontend-dev-env
~~~
git checkout -f 1-webpack/1-entry
~~~

### 2. 프로젝트 초기화 및 package.josn 파일 생성
* /lecture-frontend-dev-env
~~~
❯ npm init -y
~~~

### 3. webpack, webpack-cli 패키지 설치
* /lecture-frontend-dev-env
~~~
❯ npm install -D webpack webpack-cli
~~~

### 4. webpack.config.js 생성 후, 설정 
* /lecture-frontend-dev-env/webpack.config.js  
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

### 5. 웹팩 실행을 위한 NPM 커스텀 명렁어 추가
* /lecture-frontend-dev-env/package.json
~~~
  "scripts": {
    "build": "./node_modules/.bin/webpack"
  },
~~~

### 6. 웹팩으로 빌드하기
* /lecture-frontend-dev-env
~~~
❯ npm run build
~~~

### 7. 웹팩으로 빌드한 js파일을, index.html에서 로딩하기
* /Users/gonghojin/Documents/company/edu/lecture-frontend-dev-env/index.html
~~~
<!-- TODO: 웹팩으로 빌드한 자바스크립트를 여기에 로딩하세요 -->
<script type="text/javascript" src="dist/main.js"></script>
~~~

### 8. index.html 실행시킨 후 결과 보기
* /Users/gonghojin/Documents/company/edu/lecture-frontend-dev-env/index.html
    *  VSCOODE에서 `open in brower` 플러그인 설치 후,   
    index.html 오른쪽 클릭 -> `Open In Default Browser`  
    
![https://webpack.js.org/](/assets/educations/images/lab1.png)  
