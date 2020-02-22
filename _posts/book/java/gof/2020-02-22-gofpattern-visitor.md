---
layout: post
comments: true
title:  "Gof 디자인패턴_방문자(Visitor)"
date:   2020-02-22 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 방문자(Visitor)
### 의도
객체 구조를 이루는 원소에 대해 수행할 연산을 표현한다. 연산을 적용할 원소의 클래스를 변경하지 않고 새로운 연산을 정의할 수 있게 한다.

### 활용성
방문자 패턴은 다음의 경우에 사용한다.
+ 다른 인터페이스를 가진 클래스가 객체 구조에 포함되어 있으며, 구체 클래스에 따라 달라진 연산을 이들 클래스의 객체에 대해 수행하고자 할 때

### 구조
![alt](/assets/gof/images/gof-design-patterns-visitor.png)

+ Visitor
	- 객체 구조 내에 있는 각 ConcreteElement 클래스를 위한 Visit() 연산을 선언한다.  
	연산의 이름과 인터페이스 형태는 Visit() 요청을 방문자에게 보내는 클래스를 식별한다. 이로써 방문자는 방문된 원소의 구체 클래스를 결정할 수 있다.  
	그러고 나서 방문자는 그 원소가 제공하는 인터페이스를 통해 원소에 직접 접근할 수 있다.
+ ConcreteVisitor
	- Visitor 클래스에 선언된 연산을 구현한다. 각 연산은 구조 내에 있는 객체의 대응 클래스에 정의된 일부 알고리즘을 구현한다.  
	ConcreteVisitor 클래스는 알고리즘이 운영될 수 있는 상황 정보를 제공하며 자체 상태를 저장한다. 이 상태는 객체 구조를 순회하는 도중 순회 결과를 누적할 때가 많다.
+ Element
	- 방문자를 인자로 받아들이는 Accept() 연산을 정의한다.
+ ConcreteElement
	- 인자로 방문자 객체를 받아들이는 Accept() 연산을 구현한다.
  
---

- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%EB%B9%84%EC%A7%80%ED%84%B0_%ED%8C%A8%ED%84%B4>
