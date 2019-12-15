---
layout: post
comments: true
title:  "Gof 디자인패턴_명령(Command) 패턴"
date:   2019-12-15 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 명령(command) 패턴
### 의도
요청 자체를 캡슐화하는 것으로, 요청이 서로 다른 사용자를 매개변수로 만들고 요청을 대기시키거나 로깅하며, 되돌릴 수 있는 연산을 지원한다.

### 활용성
##### 장점
+ 수행할 동작을 객체로 매개변수화하고자 할 때
	+ 절차 지향 프로그램에서는 이를 `콜백(callback)` 함수, 즉 어딘가 등록되었다가 나중에 호출되는 함수를 사용해서 이러한 매개변수화를 표헌한다.  
	객체 지향 방식으로 이를 나타낸 것이 바로 `명령 패턴`이다.

+ 서로 다른 시간에 요청을 명시하고, 저장하며, 실행하고 싶을 때  
	+ Command 객체는 원래의 요청과 다른 생명주기(life time)가 있다. 요청을 받아 처리하는 객체가 주소 지정 방식과는 독립적으로 표현될 수 있다면,  
	Command 객체를 다른 프로세스에게 넘겨주고 거기서 해당 처리를 진행하게 할 수 있다.

### 구조
![alt](/assets/gof/images/gof-design-patterns-command.png)

+ Command
	+ 연산 수행에 필요한 인터페이스를 선언한다.
+ ConcreteCommand
	+ Receiver 객체와 액션 간의 연결성을 정의한다.  
	또한 처리 객체에 정의된 연산을 호출하도록 Execute를 구현한다.
+ Client
	+ 객체를 생성하고 처리 객체로 정의한다.
+ Invoker
	+ Command에 처리를 수행할 것을 요청한다.
+ Receiver
	+ 요청에 관련된 연산 수행 방법을 알고 있다.  
		어떤 클래스도 요청 수신자로서 동작할 수 있다.

### 협력 방법
+ Client는 ConcreteCommand 객체를 생성하고 이를 수신자로 지정한다.
+ Invoker 클래스는 ConcreteCommand 객체를 저장한다.
+ Invoker 클래스는 command에 정의된 Execute() 호출하여 요청을 발생시킨다.  
	명령어가 취소 가능한 것이라면 ConcreteCommand는 이전에  Execute() 호출 전 상태의 취소 처리를 위해 저장한다.
+ ConcreteCommand 객체는 요청을 실제 처리할 객체에 정의된 연산을 호출한다.

### 결과
+ Command는 연산을 호출하는 객체와 연산 수행 방법을 구현하는 객체를 분리한다.
+ Command는 일급 클래스이다. 다른 객체와 같은 방식으로 조작되고 확장할 수 있다.
+ 새로운 Command 객체를 추가하기 쉽다. 기존 클래스르 변경할 필요없이 단지 새로운 명렁어에 대응하는 클래스만 정의하면 된다.

### 예제 코드
~~~java
// Invoker class
public class Switch {
	private Command flipUpCommand;
	private Command flipDownCommand;
	
	public Switch(Command flipUpCommand, Command flipDownCommand) {
		this.flipUpCommand = flipUpCommand;
		this.flipDownCommand = flipDownCommand;
	}
	
	public void flipUp() {
		flipUpCommand.execute();
	}
	
	public void flipDown() {
		flipDownCommand.execute();
	}
}

// Receiver class
public class Light {
	public Light() {}
	
	public void turnOn() {
		System.out.println("The light is on");
	}
	
	public void turnOff() {
		System.out.println("The light is off");
	}
}

// Command interface
public interface Command {
	void execute();
}

// ConcreteCommand 1
public class TurnOnLightCommand implements Command {
	private Light light;
	
	public TurnOnLightCommand(Light light) {
		this.light = light;
	}
	
	public void execute() {
		this.theLight.turnOn();
	}
}

// ConcreteCommand 2
public class TurnOffLightCommand implements Command {
	private Light light;
	
	public TurnOffLightCommand(Light light) {
		this.light = light;
	}
	
	public void execute() {
		light.turnOff();
	}
}

// Client
public class TestCommand {
	public static void main(String[] args){
	  Light light = new Light();
	  Command switchUp = new TurnOnLightCommand(light);
	  Command switchDown = new TurnOffLightCommand(light);
	  
	  Switch sw = new Switch(switchUp, switchDown);
	  sw.flipUp();
	  sw.flipDown();
	}
}
~~~

---
- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%EC%BB%A4%EB%A7%A8%EB%93%9C_%ED%8C%A8%ED%84%B4>
	+ <https://online.visual-paradigm.com/diagrams/examples/class-diagram/gof-design-patterns-command/>

