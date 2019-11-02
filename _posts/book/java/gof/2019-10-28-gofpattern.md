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
- 장점
1. 구체적인 클래스를 분리한다.  
	- 팩토리는 제품 객체를 생성하는 과정과 책임을 캡슐화(ConcreteFactory 구상 클래스에 의해)한 것이기 때문에, 구체적인 구현 클래스가 사용자에게서 분리된다.  
	제품 클래스 이름이 `구상 팩토리에` 분리되므로, Client 코드에는 나타나지 않는 것이다.

2. 제품군을 쉽게 대체 가능
	- 구체 팩토리의 클래스는 응용프로그램에서 한 번만 나타나기 때문에 응용프로그램이 사용할 구체 팩토리를 변경하기 쉽다.
		- 구체 팩토리의 변경 용이성은 제품군을 쉽게 변경할 수 있는 이점으로 이어진다.

3. 

- 단점
1. 새로운 종류의 제품을 제공하기 어렵다.
	- 새로운 종류의 제품을 만들기 위해 기존 추상 팩토리를 확장해야 하는데, 이는 추상 팩토리와 모든 서브클래스의 변경을 가져온다.  
	즉 , 인터페이스가 변경되는 새로운 제품을 생성하는 연산이 추가되거나, 기존 연산의 반환 객체 타입이 변경되었으므로, 이를 상속받는 서브클래스 모두 변경되어야 한다.

### 구현
1. 팩토리를 단일체로 정의한다.
	- 한 제품군에 대해서 하나의 ConcreteFactory 인스턴스만 있으면 된다.  
	즉, 갖가지 제품의 종류를 만들어 내는 팩토리는 제품군에 대해서 하나면 된다.

2. 제품을 생성한다.
	- AbstractFactory는 단지 제품을 생성하기 위한 `인터페이스`를 선언하는 것이고, 그것을 생성하는 책임은 Product의 서브클래스인 ConcreteProduct에 있다.  
	- AbstractFactory는 각 제품 생성을 위한 팩토리 메서드를 재정의함(overriding)함으로써 각 제품의 인스턴스를 만든다.

3. 확장 가능한 팩토리들을 정의한다.
	- AbstractFactory에는 생성할 각 제품의 종류별로 서로 다른 연산(CreateProductA(), CreateProductB())을 정의한다.  
	따라서 새로운 종류의 제품이 추가되면 AbstractFactory의 인터페이스에도 새로운 연산을 추가해야 한다.
		

### [예제코드](https://github.com/gonghojin/educations/tree/master/java_designpattern/src/creation/factory/abstractf)
