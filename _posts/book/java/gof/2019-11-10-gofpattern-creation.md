---
layout: post
comments: true
title:  "Gof 디자인패턴_생성 패턴에 대한 논의"
date:   2019-11-09 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 생성 패턴에 대한 논의
시스템이 생성하는 객체의 클래스로 시스템을 매개변수화하는 일반적인 방법은 두 가지가 있다.
1. 객체를 생성하는 클래스를 상속해서 서브클래스를 만드는 `팩토리 메서드`
	- 단점: 제품 클래스가 바뀔 때마다 새로운 서브클래스를 생성해야 한다.
2. 객체 합성으로 시스템을 매개변수화하는 `추상 팩토리, 빌더, 원형 패턴(프로토타입)`
	- 이 세 가지 패턴은 새로운 "팩토리 객체"를 만든다.  
		:: 팩토리 객체 - 제품 객체를 생성하는 책임을 갖는 객체
		- 추상 팩토리 패턴: 여러 클래스의 객체를 생성하는 팩토리 객체 보유
		- 빌더 패턴: 복잡한 프로토콜을 사용하여 제품을 구축하는 팩토리 객체 보유
		- 원형 패턴: 원형 객체를 복사하여 제품을 구축하는 팩토리 객체 보유
		
---
여러 요인에 따라서 어떤 패턴이 가장 좋은지 결정할 수 있다.  
개발자들은 객체를 생성하는 표준 방식으로서 `팩토리 메서드`를 자주 사용하지만,  
인스턴스화할 클래스가 변하지 않거나 초기화 연산처럼 서브클래스들이 쉽게 재정의할 수 있는 연산에서 인스턴스화가 된다면    
`상속`으로도 쉽게 해결 가능하기 때문에 이때는 꼭 팩토리 메서드를 사용할 필요는 없다. 

추상 팩토리, 프로토타입 또는 빌더 패턴을 사용하는 설계는 팩토리 메서드를 사용하는 설계보다 더 유연할 때가 많다.  
팩토리 메서드를 사용해서 시작한 설계에 좀더 유연성을 부가할 필요가 있다면 다른 생성 패턴을 사용하는 설계로 바꾸자.  
