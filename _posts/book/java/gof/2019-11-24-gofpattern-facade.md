---
layout: post
comments: true
title:  "Gof 디자인패턴_파사드(Facade) 패턴"
date:   2019-11-24 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 파사드(Facade) 패턴
### 의도
한 서브시스템 내의 인터페이스 집합에 대한 `획일화된 하나의 인터페이스를` 제공하는 패턴으로, 서브시스템을 사용하기 쉽도록 `상위 수준의 인터페이스를` 정의한다.

### 동기
시스템을 서브시스템으로 구조화하면 복잡성을 줄이는 데에 큰 도움이 된다. 공통적인 설계 목표는 `서브시스템 사이의 의사소통 및 종속성을` 최소화하려는 것이다.  
이런 목표를 달성하도록 도와주는 패턴이 `Facade(파사드)이다`.  
주어진 서브시스템의 일반적인 기능에 대한 `단순화된 하나의 인터페이스`를 제공하려는 것이다.
![alt](/assets/gof/images/gof-design-patterns-facade1.png)

### 활용성
+ 복잡한 서브시스템에 대한 `단순한 인터페이스 제공`이 필요할 때
+ 추상 개념에 대한 구현클래스와 사용자 사이에 너무 많은 종속성이 존재할 때  
	+ 퍼사드의 사용을 통해 사용자와 다른 서브시스템 간의 결합도를 줄일 수 있다.
		+ 서브시스템에 정의된 모든 인터페이스가 공개되면 빈번한 메서드 호출이 있을 수 있으나,  
		이런 호출은 단순한 형태로 통합하여 제공하고 나머지는 내부적으로 처리함으로써 사용자와 서브시스템 사이의 호출 횟수는 실질적으로 감소하게 된다.

### 구조
![alt](/assets/gof/images/gof-design-patterns-facade2.png)

+ 파사드(Facade): 단순하고 일관된 통합 인터페이스를 제공  
	+ 서브시스템을 구성하는 어떤 클래스가 어떤 요청을 처리해야 하는지 알고 있다.
	+ 사용자의 요청을 해당 서브시스템 객체에 전달  
	
+ 서브시스템 클래스: 서브시스템의 기능을 구현하고, Facade 객체로 할당된 작업을 실제로 처리하지만,  
	Facade에 대한 아무런 정보가 없다. `즉, 이들에 대한 어떤 참조자도 가지고 있지 않다.`

### 결과
##### 장점
+ 서브시스템의 구성요소를 보호할 수 있다.  
	+ 이로써 `사용자가 다루어야 할 객체의 수가 줄어들며`, 서브시스템을 쉽게 사용할 수 있다.
+ 서브시스템과 사용자 코드 간의 결합도를 더욱 약하게 만든다.  
	결합도가 낮다는 소리는 각 시스템 요소가 다른 요소로부터 그리고 변경으로부터 잘 격리되어 있다는 의미다. 시스템 요소가 서로 잘 격리되어 있으면 각 요소를 이해하기도 더 쉬워진다.)

### 예제 코드
Client가 파사드(Compute)를 통해 컴퓨터 내부의 부품(CPU, HDD) 등을 접근한다는 내용의 예제
+ Facade  

~~~java
class Compute {
	public void startComputer() {
		CPU cpu = new CPU();
		Memory memory = new Memory();
		HardDrive hardDrive = new HardDrive();
		
		cpu.freeze();
		memory.load(..., hardDrive.read());
		cpu.jump(...);
		cpu.execute();
	}
}
~~~

+ SubClass  

~~~java
class CPU {
	public void freeze() {...}
	public void jump(long position) {...}
	public void execute() {...}
}

class Memory {
	public void load(..., byte[] data) {...}
}

class HardDrive {
	public byte[] read(...) {...}
}
~~~

+ Client  

~~~java
class Client {
	public static void main(String[] args){
	  Compute facade = /* facade instance */;
	  facade.startComputer();
	}
}
~~~

### 관련 패턴
+ 추상 팩토리 패턴
	+ 서브시스템 객체를 생성하는 인터페이스를 제공하기 위해 Facade와 함께 사용할 수 있다.
	
---
- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <http://best-practice-software-engineering.ifs.tuwien.ac.at/patterns/facade.html>
	+ <https://online.visual-paradigm.com/diagrams/examples/class-diagram/gof-design-patterns-facade/>
