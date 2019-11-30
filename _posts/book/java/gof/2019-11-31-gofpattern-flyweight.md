---
layout: post
comments: true
title:  "Gof 디자인패턴_플라이웨이트(FLYWEIGHT) 패턴"
date:   2019-11-30 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 플라이웨이트(FlyWeight) 패턴
### 의도
공유(sharing)를 통해 많은 수로 쪼개진(fine-grained) 객체들을 효과적으로 지원한다.

### 활용성
플라이웨이트 패턴은 언제 사용하는가에 따라서 그 효과가 달라지며, 다음과 같은 경우에 사용할 수 있다.  
+ 응용프로그램이 대량의 객체를 사용해야 할 때
+ 객체의 수가 너무 많아져 저장 비용이 너무 높아질 때

### 구조
![alt](/assets/gof/images/gof-design-patterns-flyweight.png)

+ Flyweight  
	- Flyweight가 받아들일 수 있고, `부가적 상태에서 동작해야 하는 인터페이스를` 선언  
~~~
플라이웨이트 패턴에서 중요한 개념은 본질적(intinsic) 상태와 부가적(extrinsic) 상태의 구분이다.  
본질적 상태는 플라이웨이트 객체에 저장되어야 하며, 이것이 적용되는 상황과 상관없는 본질적 특성 정보들이 객체를 구성한다.
본질적이지 않은 부가적 상태는 플라이웨이트 객체가 사용될 상황에 따라 달라질 수 있고, 그 상황에 종속적이다.  
그러므로 공유될 수 없다. 사용자 객체는 이런 부가적 상태를 그것이 필요한 플라이웨이트 객체에 전달해야하는 책임을 갖는다. 
~~~
+ ConcreteFlyweight   
	- Flyweight 인터페이스를 구현하고 내부적으로 갖고 있어야 하는 `본질적 상태에 대한 저장소를` 정의한다.  
		ConcreteFlyweight 객체는 공유할 수 있는 것이어야 한다. 그러므로 관리하는 어떤 상태라도 본질적인 것이어야 한다.
+ UnsharedConcreteFlyweight
	- Flyweight 인터페이스는 공유를 가능하게 하지만, 모든 플라이웨이트 서브클래스들이 공유될 필요는 없다.  
	- UnsharedConcreteFlyweight 객체가 ConcreteFlyweight 객체를 자신의 자식으로 갖는 것은 흔한 경우이다.
+ FlyweightFactory
	- 플라이웨이트 객체를 생성하고 관리하며, 제대로 공유되도록 보장해 준다.  
	- Client가 플라이웨이트 객체를 요청하면 FlyweightFactory 객체는 이미 존재하는 인스턴스를 제공하거나 존재하지 않는다면 새로 생성한다.
+ Client
	- 플라이웨이트 객체에 대한 참조자를 관리하며, 플라이웨이트 객체의 부가적 상태를 저장한다.

---
- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <http://best-practice-software-engineering.ifs.tuwien.ac.at/patterns/facade.html>
	+ <https://online.visual-paradigm.com/diagrams/examples/class-diagram/gof-design-patterns-facade/>
