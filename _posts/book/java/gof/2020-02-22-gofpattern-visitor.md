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

### 협력 방법
+ 방문자 패턴을 사용하는 사용자는 ConcreteVisitor 클래스의 객체를 생성하고 객체 구조를 따라서 각 원소를 방문하며 순회해야 한다.  
+ (방문자가) 구성 원소들을 방문할 때, 구성 원소는 해당 클래스의 Visitor 연산을 호출한다. 이 원소들은 자신을 Visitor 연산에 필요한 인자로 제공하여 (필요하면) 방문자 자신의 상태에 접근할 수 있도록 한다.

### 결과
방문자 패턴 사용의 장단점
##### 장점
+ Visitor 클래스는 새로운 연산을 쉽게 추가한다.
	- Visitor 클래스는 복잡한 객체를 구성하는 요소에 속한 연산을 쉽게 추가할 수 있다.  
	새로운 방문자를 추가하면 객체 구조에 대한 새로운 연산을 추가한 것이 된다.  
+ 방문자를 통해 관련된 연산들을 한 군데로 모으고 관련되지 않은 연산을 떼어낼 수 있다.

##### 단점
+ 새로운 ConcreteElement 클래스를 추가하기가 어렵다.
	- 방문자 패턴을 사용하면 Element 클래스에 대한 새로운 서브클래스를 추가하기가 어려워 진다.  
	ConcreteElement 클래스가 새로 생길 때마다, Visitor 클래스에 대한 새로운 추상 연산 및 모든 ConcreteVisitor 클래스에 그 연산에 대응하는 구현을 제공해야 한다.

### 예제 코드
~~~java
interface CarElementVisitor {
	void visit(Wheel wheel);
	void visit(Engine engine);
	void visit(Body body);
	void visit(Car car);
}

interface CarElement {
	void accept(CarElementVisitor carElementVisitor);
}

class Wheel implements CarElement {
	private String name;
	
	public Wheel(String name) {
		this.name = name;
	}
	
	public String getName() {
		return this.name;
	}
	
	public void accept(CarElementVisitor visitor) {
		visitor.visit(this);
	}
}

class Engine implements CarElement {
    public void accept(CarElementVisitor visitor) {
        visitor.visit(this);
    }
}

class Body implements CarElement {
    public void accept(CarElementVisitor visitor) {
        visitor.visit(this);
    }
}

class Car implements CarElement {
	CarElement[] elements;
	
	public CarElement[] getElements() {
		return elements.clone(); 
	}
	
	public Car () {
		this.elements = new CarElement[]{
			new Wheel("front left"), new Wheel("front right"),
			new Wheel("back left") , new Wheel("back right"),
			new Body(), new Engine()
		};
	}
	
	public void accept(CarElementVisitor visitor) {
		for (CarElement element : this.getElements()) {
			element.accept(visitor);
		}
		
		 visitor.visit(this);
	}
}

class CarElementPrintVisitor implements CarElementVisitor {
	public void visit(Wheel wheel) {
		System.out.println("Visiting "+ wheel.getName() + " wheel");
	}

	public void visit(Engine engine) {
		System.out.println("Visiting engine");
	}

	public void visit(Body body) {
		System.out.println("Visiting body");
	}

	public void visit(Car car) {
		System.out.println("Visiting car");
	}
}

class CarElementDoVisitor implements CarElementVisitor {
	public void visit(Wheel wheel) {
		System.out.println("Kicking my "+ wheel.getName() + " wheel");
	}

	public void visit(Engine engine) {
		System.out.println("Starting my engine");
	}

	public void visit(Body body) {
		System.out.println("Moving my body");
	}

	public void visit(Car car) {
		System.out.println("Starting my car");
	}
}

public class VisitorDemo {
	static public void main(String[] args){
		Car car = new Car();
		car.accept(new CarElementPrintVisitor());
		car.accept(new CarElementDoVisitor());
	}
}
~~~

---

- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%EB%B9%84%EC%A7%80%ED%84%B0_%ED%8C%A8%ED%84%B4>
