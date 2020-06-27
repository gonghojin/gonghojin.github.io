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
### 1. Lab1 브런치로 이동하기
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

----
# 1-webpack/2-loader: 웹팩 로더 실습(Lab 2)
---
### 2. Lab2 브런치로 이동하기
* /lecture-frontend-dev-env
~~~
git checkout -f 1-webpack/2-loader
~~~

### 2. CSS와 파일을 로딩하기 위한 패키지 추가
* /lecture-frontend-dev-env  
~~~
❯ npm install -D css-loader style-loader file-loader
~~~

### 3. "TODO: CSS 파일을 엔트리포인트(app.js)에서 로딩하세요" 해결하기
#### 3.1 css loader & style loader 설정
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
      module: {
        rules: [{
          test: /\.css$/,
          use: ['style-loader', 'css-loader'], // style-loader를 앞에 추가한다
        }
        ]
      }
    }
~~~
#### 3.2 css 파일 import
* /lecture-frontend-dev-env/src/app.js
~~~
    import MainController from "./controllers/MainController.js";
    import "./main.css";
    document.addEventListener("DOMContentLoaded", () => {
      new MainController();
    });
~~~

### 4. "TODO: 파일을 로딩할수 있도록 웹팩 로더 설정을 추가하세요 (file-loader)" 해결하기
#### 4.1 file-loader 설정

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
  module: {
    rules: [{
      test: /\.css$/,
      use: ['style-loader', 'css-loader'], // style-loader를 앞에 추가한다
    },
    {
      test: /\.(png|jpg)$/, // .png 또는 .jpg 확장자로 마치는 모든 파일
      loader: 'file-loader',
      options: {
        publicPath: './dist/', // prefix를 아웃풋 경로로 지정
        name: '[name].[ext]?[hash]', // 파일명 형식
      }
    }
    ]
  }
}
~~~

### 5. 웹팩으로 빌드하기
* /lecture-frontend-dev-env
~~~
❯ npm run build
~~~

### 6. index.html 실행시킨 후 결과 보기
* /Users/gonghojin/Documents/company/edu/lecture-frontend-dev-env/index.html

----
![https://webpack.js.org/](/assets/educations/images/lab1.png)  
----

![https://webpack.js.org/](/assets/educations/images/lab2.png)  
