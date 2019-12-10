---
layout: post
comments: true
title:  "Gof 디자인패턴_책임 연쇄(Chain Of Responsibility) 패턴"
date:   2019-12-10 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 책임 연쇄(Chain Of Responsibility) 패턴
### 의도
메시지를 보내는 객체와 이를 받아 처리하는 객체들 간의 `결합도를 없애기 위한` 패턴이다.  
`하나의 요청`에 대한 처리가 반드시 한 객체에서만 되지 않고, `여러 객체에게 처리 기회를` 줄 수 있다.  

### 활용성
책임 연쇄 패턴은 다음의 경우에 사용한다.
+ 하나 이상의 객체가 요청을 처리해야 하고, 그 요청 처리자 중 어떤 것이 `선행자(priori)`인지 모를 때, 처리자가 자동으로 확정되어야 한다.  
+ 메시지를 받을 객체를 명시하지 않은 채 여러 객체 중 하나에게 처리를 요청하고 싶을 때
+ 요청을 처리할 수 있는 객체 집합이 동적으로 정의되어야 할 때

### 구조
![alt](/assets/gof/images/gof-design-patterns-chain-of-responsibility.png)

+ Handler
	- 요청을 처리하는 인터페이스를 정의하고, 후속 처리자(successor)와 연결을 구현한다.  
	`즉, 연결 고리에 연결된 다음 객체에게 다시 메시지를 보낸다.`
+ ConcreteHandler
	- 책임져야 할 행동이 있다면 스스로 요청을 처리하여 후속 처리자에게 접근할 수 있다.  
	`즉, 자신이 처리할 행동이 있으면 처리하고, 그렇지 않으면 후속 처리자에 다시 처리를 요청한다.`
+ Client
	- ConcreteHandler 객체에게 필요한 요청을 보낸다.
