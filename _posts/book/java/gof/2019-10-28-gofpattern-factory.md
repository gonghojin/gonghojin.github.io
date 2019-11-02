---
layout: post
comments: true
title:  "Gof 디자인패턴_팩토리 메서드"
date:   2019-11-02 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 팩토리 메서드(Factory Method)
### 의도 
객체를 생성하기 위해 인터페이스를 정의하지만, `어떤 클래스의 인스턴스를 생성할지에 대한 결정`은 서브클래스가 내리도록 한다.

### 활용성
+ 어떤 클래스가 자신이 생성해야 하는 객체의 클래스를 예측할 수 없을 때
+ 생성할 객체를 기술하는 책임을 자신의 서브클래스가 지정했으면 할 때
+ 객체 생성의 책임을 몇 개의 보조 서브클래스 가운데 하나에게 위임하고, 어떤 서브클래스가 위임자인지에 대한 정보를 국소화시키고 싶을 때

### 구조
![Image Alt 텍스트](/assets/gof/images/factorymethod.png)
 
+ Product(Vehicle): 팩토리 메서드가 생성하는 객체의 인터페이스를 정의
+ ConcreteProduct(Truck, Car...): Product 클래스에 정의된 인터페이스를 실제로 구현
+ Creator(VehicleFactory): Product 타입의 객체를 반환하는 팩토리 메서드를 선언한다.
	+ 팩토리 메서드를 기본적으로 구현하는데, 이 구현에서는 ConcreteProduct 객체를 반환한다.
	+ 또한 Product 객체의 생성을 위해 팩토리 메서드를 호출한다.
+ ConcreteCreator(CarFactory, TruckFactory): 팩토리 메서드를 재정의하여 ConcreteProduct의 인스턴스를 반환한다.

### 협력방법
+ Creator는 자신의 서브클래스를 통해 실제 필요한 팩토리 메서드를 정의하여 적절한 ConcreteProduct의 인스턴스를 반환할 수 있게 한다.

### 결과
팩토리 메서드 패턴은 응용프로그램에 국한된 클래스가 코드에 종속되지 않도록 해준다.  
응용프로그램은 Product 클래스에 정의된 인터페이스와만 동작하도록 코드가 만들어지기 때문에, 사용자가 정의한 어떤 ConcreteProduct 클래스와도 동작할 수 있게 된다.

### 구현
+ 구현 방법은 크게 두가지
	1. Creator 클래스를 추상 클래스로 정의하고, 정의한 팩토리 메서드에 대한 구현은 제공하지 않는 경우
	2. Creator가 구체 클래스이고, 팩토리 메서드에 대한 기본 구현을 제공하는 경우

### [예제코드](https://github.com/gonghojin/educations/tree/master/java_designpattern/src/creation/factory/method)

---
- 참조
	+ Gof의 디자인 패턴	
