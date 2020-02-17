---
layout: post
comments: true
title:  "Gof 디자인패턴_전략(Strategy)"
date:   2020-02-17 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 전략(Strategy)
### 의도
동일 계열의 알고리즘군을 정의하고, 각 알고리즘을 캡슐화하며, 이들을 상호교환이 가능하도록 만듭니다.  
알고리즘을 사용하는 클라이언트와 상관없이 독립적으로 알고리즘을 다양하게 변경할 수 있게 한다.

### 활용성
다음 상황에서 전략 패턴을 사용할 수 있다.
+ 행동들이 조금씩 다를 뿐 개념적으로 관련된 많은 클래스들이 존재할 때.  
	전략 패턴은 많은 행동 중 하나를 가진 클래스를 구성할 수 있는 방법을 제공한다.
+ 사용자가 몰라야 하는 데이터를 사용하는 알고리즘이 있을 때.  
	노출하지 말아야 할 복잡한 자료 구조는 Strategy 클래스에만 두면 되므로 사용자는 몰라도 된다.  
+ 하나의 클래스가 많은 클래스가 많은 행동을 정의하고, 이런 행동들이 그 클래스의 연산 안에서 복잡한 다중 조건문의 모습을 취할 때.  
많은 조건문보다는 각각을 Stategy 클래스로 옮겨놓는 것이 좋다.
---

- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%EC%83%81%ED%83%9C_%ED%8C%A8%ED%84%B4>

