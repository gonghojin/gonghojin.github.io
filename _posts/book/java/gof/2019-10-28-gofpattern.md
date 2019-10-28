---
layout: post
comments: true
title:  "Gof 디자인패턴_추상팩토리와 팩토리 메서드"
date:   2019-10-28 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 추상 팩토리(Abstract Factory)
### 목적
상세화된 서브클래스(구상 클래스)를 정의하지 않고도 `서로 관련성 있거나 독립적인 여러 객체의 군을` 생성하기 위한 인터페이스를 제공한다.

WidgetFactory 인터페이스 [AbstractFactory] - 각 추상화된 위젯 클래스의 인스턴스를 생성하여 반환하는 연산을 정의
+ Client는 WidgetFactory에 원하는 요소의 인스턴스를 생성하는 연산을 호출하여 인스턴스를 획득
	+ Client는 어떤 Concrete class(구상 클래스)가 이들 연산은 구현하여 결과를 반환하는지 알 필요도 없다. 즉, 팩토리에만 메시지를 보낼 뿐이다.

ModifiWidgetFactory, PMWidgetFactory[WidgetFactory를 상속받는 Concrete Class(구상 클래스)] - 실제 위젯을 생성하는 연산을 구현

### 활용
추상 팩토리는 다음의 경우에 사용한다.
+ 객체가 생성되거나 구성, 표현되는 방식과 무관하게 시스템을 독립적으로 만들고자 할 때
+ 여러 제품군 중 하나를 선택해서 시스템을 설정해야 하고 한번 구성한 제품을 다른 것으로 대체할 수 있을 때 
+ 관련된 제품 객체들이 함께 사용되도록 설계되었고, 이 부분에 대한 제약이 외부에도 지켜지도록 하고 싶을 때
+ 제품에 대한 클래스 라이브러리를 제공하고, 그들의 구현이 아닌 인터페이스를 노출시키고 싶을 

### 구조
![Image Alt 텍스트](/assets/gof/images/abstractFactory.png)

+ AbstractFactory(WidgetFactory): `개념적 제품에 대한 객체`를 생성하는 연산으로 `인터페이스를` 정의한다.
+ ConcreteFactory(ModifiWidgetFactory, PMWidgetFactory): `구체적인 제품에 대한 객체를 생성하는 연산`을 구현한다.
+ AbstractProduct(Window, ScrollBar): `개념적 제품 객체`에 대한 `인터페이스`를 정의한다.
+ ConcreteProduct(MotifiWindow, MotifiScrollBar): 구체적으로 `팩토리가 생성할 객체`를 정의하고, `AbstractProduct가` 정의하는 인터페이스를 구현한다.
+ Client: AbstractFactory와 AbstractProduct 클래스에 선언된 인터페이스를 사용한다.

### 협력 방법
+ 일반적으로 ConcreteFactory 클래스의 인스턴스 한 개가 `런타임`에 만들어 진다. 이 구상 팩토리(concrete factory)는` 어떤 특정 구현을 갖는 제품 객체를` 생성한다.  
서로 다른 제품 객체를 생성하려면 사용자는 `서로 다른 구상 팩토리를` 사용해야 한다.
+ AbstractFactory는 필요한 제품 객체를 생성하는 책임을 ConcreteFactory 구상 클래스에 위임한다.

### 장점과 단점
1. 구체적인 클래스를 분리한다.  
	- 팩토리는 제품 객체를 생성하는 과정과 책임을 캡슐화(ConcreteFactory 구상 클래스에 의해)한 것이기 때문에, 구체적인 구현 클래스가 사용자에게서 분리된다.  
	제품 클래스 이름이 `구상 팩토리에` 분리되므로, Client 코드에는 나타나지 않는 것이다.
