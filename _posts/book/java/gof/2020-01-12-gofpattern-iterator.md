---
layout: post
comments: true
title:  "Gof 디자인패턴_반복자(Iterator) 패턴"
date:   2020-01-12 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 반복자(Iterator) 패턴
### 의도
내부 표현부를 노출하지 않고 어떤 집합 객체에 속한 원소들을 순차적으로 접근할 수 있는 방법을 제공

### 활용성
##### 사용 목적
+ 객체 내부 표현 방식을 모르고도 집합 객체의 각 원소들에 접근하고 싶을 때
+ 잡합 객체를 순회하는 다양한 방법을 지원하고 싶을 때
+ 서로 다른 집합 객체 구조에 대해서도 동일한 방법으로 순회하고 싶을 때

### 구조
![alt](/assets/gof/images/gof-design-patterns-iterator.png)

+ Iterator
	- 원소를 접근하고 순회하는 데 필요한 인터페이스를 제공한다.
+ ConcreteIterator
	- Iterator에 정의된 인터페이스를 구현하는 클래스로, 순회 과정 중 집합 객체 내에서 현재 위치를 기억한다.
+ Aggregate
	- Iterator 객체를 생성하는 인터페이스를 정의한다.
+ ConcreteAggregate
	- 해당하는 ConcreteIterator의 인스턴스를 반환하는 Iterator 생성 인터페이스를 구현한다.

### 협력 방법
ConcreteIterator는 집합 객체 내 현재 객체를 계속 추적하고 다음번 방문할 객체를 결정한다.  

### 결과
##### Iterator 패턴의 주요 특징 세가지
+ 집합 객체의 다양한 순회 방법을 제공한다.  
+ Iterator는 Aggregate 클래스의 인터페이스를 단순화한다.  
	+ Iterator의 순회 인터페이스는 Aggregate 클래스에 정의한 자신과 비슷한 인터페이스들을 없애서 Aggregate 인터페이스를 단순화할 수 있다.  
+ 집합 객체에 따라 하나 이상의 순회 방법이 제공될 수 있다.  


---
- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://github.com/iluwatar/java-design-patterns/tree/master/interpreter/src/main/java/com/iluwatar/interpreter>
	+ <https://online.visual-paradigm.com/diagrams/templates/class-diagram/gof-design-patterns-interpreter/>

