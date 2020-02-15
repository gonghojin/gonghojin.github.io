---
layout: post
comments: true
title:  "Gof 디자인패턴_상태(State)"
date:   2020-02-12 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 상태(State) 패턴
### 의도
객체의 내부 상태에 따라 스스로 행동을 변경할 수 있게 허가하는 패턴으로, 이렇게 하면 객체는 마치 자신의 클래스를 바꾸는 것처럼 보인다.

### 활용성
다음 상황 가운데 하나에 속하면 상태 패턴을 사용할 수 있다.
+ 객체의 행동이 상태에 따라 달라질 수 있고, 객체의 상태에 따라서 런타임에 행동이 바뀌어야 한다.
+ 어떤 연산에 그 객체의 `상태에 따라 달라지는 다중 분기 조건 처리`가 많이 들어 있을 때. 객체의 상태를 표현하기 위해 상태를 하나 이상의 열거형 상수(enumerated constant)로 정의해야 한다.  
	동일한 조건 문장들을 하나 이상의 연산에 중복 정의해야 할 떄도 잦다. 이때, 객체의 상태를 별도의 객체로 정의하면, 다른 객체들과 상관없이 그 객체의 상태를 다양화시킬 수 있다.  
	
### 구조
![alt](/assets/gof/images/gof-design-patterns-state.png)  

+ Context
	- 사용자가 관심있는 인터페이스를 정의하며, 객체의 현재 상태를 정의한 ConcreteState 서브클래스의 인스턴스를 유지,관리한다.
+ State
	- Context의 각 상태별로 필요한 행동을 캡슐화하여 인터페이스로 정의
+ ConcreteState 서브클래스
	- 각 서브클래스들은 Context의 상태에 따라 처리되어야 할 실제 행동을 구현한다.
~~~

---

- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%EC%83%81%ED%83%9C_%ED%8C%A8%ED%84%B4>

