---
layout: post
comments: true
title:  "Gof 디자인패턴_싱글턴"
date:   2019-11-09 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 싱글턴(Singleton)
### 의도
`오직 한 개의 클래스 인스턴스`만을 갖도록 보장하고, 이에 대한 전역적인 접근점을 제공한다.  

### 활용성
+ 클래스의 인스턴스가 오직 하나여야 함을 보장하고, 잘 정의된 접근점으로 모던 사용자가 접근할 수 있도록 해야 할 때
+ 유일한 인스턴스가 서브클래싱으로 확장되어야 하며, 사용자는 코드의 수정없이 확장된 서브클래스의 인스턴스를 사용할 수 있어야 할 때

### 구조
![alt](/assets/gof/images/gof-design-patterns-singleton.png)

+ Singleton: Instance() 연산을 정의하여, 유일한 인스턴스로 접근할 수 있도록 한다.

### 협력 방법
+ 사용자는  Singleton 클래스에 정의된 Instance() 연산을 통해 유일하게 생성되는 단일체 인스턴스에 접근할 수 있다.

### 결과
+ 장점
1. 유일하게 존재하는 인스턴스로의 접근을 통제
	- Singleton 클래스 자체가 인스턴스를 캡슐화하기 때문에, 해당 클래스에서 사용자가 인스턴스에 접근할 수 있는지를 제어 가능

2. name space(이름 공간이 존재하는 공간)를 좁힌다.
	- 싱글턴 패턴은 전역 변수보다 더 좋다. 전역 변수를 사용해서 name space를 망치는 일을 없애주기 때문.  
	즉, 전역 변수를 정의하여 발생하는 문제(디버깅의 어려움과 같은)를 없앤다.

3. 연산 및 표현의 개선(refinement)을 허용한다.  
	- Singleton 클래스는 상속될 수 있기 때문에, 상속한 서브클래스를 통해서 새로운 인스턴스를 만들 수 있다.

### 구현
1. 인스턴스가 유일해야 함을 보장한다.

### [예제 코드](https://github.com/gonghojin/educations/tree/master/java_designpattern/src/creation/singleton/simple)

---
- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
