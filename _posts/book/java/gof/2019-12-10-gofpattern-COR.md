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

### 협력 방법
+ 사용자는 처리를 요청하고, 이 처리 요청은 실제로 그 요청을 받을` 책임이 있는 ConcreteHandler 객체를 만날 때까지` 정의된 연결 고리를 따라서 계속 전달된다.

### 결과
##### 장점
+ 객체 간의 행동적 결합도가 줄어든다.  
	+ 메시지를 보내는 측이나 받는 측 모두 서로를 모르고, 또 연결된 객체들조차도 그 연결 구조가 어떻게 되는지 모른다.  
	결과적으로, 이 패턴은 객체들 간의 상호작용 과정을 단순화시킨다.  
+ 객체에게 책임을 할당하는 데 유연성을 높일 수 있다.  
	+ 객체의 책임을 여러 객체에게 분산시킬 수 있으므로, 런타임에 객체 연결 고리를 변경하거나 추가하여 책임을 변경하거나 확장할 수 있다.

##### 단점
+ 메시지 수신이 보장되지 않는다.
	+ 어떤 객체가 이 처리에 대한 수신을 담당한다는 것을 명시하지 않으므로 요청이 처리된다는 보장은 없다.

## 예제 코드
~~~java
abstract class Logger {
	public static int ERR = 3;
	public static int NOTICE = 5;
	public static int DEBUG = 7;
	protected int mask;
	
	// 책임 연쇄의 다음 요소
	protected Logger next;
	
	public Logger setNext(Logger log) {
		this.next = log;
		
		return log;
	}
	
	public void message(String msg, int priority) {
		if (priority <= mask) {
			writeMessage(msg);
		}
	}
	
	abstract protected void writeMessage(String msg);
}

class StdoutLogger extends Logger {
	public StdoutLogger(int mask) {
		this.mask = mask;
	}
	
	@Override
	protected void writeMessage(String msg) {
		System.out.println("Writing to stdout: " + msg);
	}
}

class EmailLogger extends Logger {
	public EmailLogger(int mask) {
		this.mask = mask;
	}
	
	protected void writeMessage(String msg) {
		System.err.println("Sending to stderr: " + msg);
	}
}

public class ChainOfResponsibilityEx {
	public static void main(String[] args){
		// Build the chain Of Responsibility
		Logger logger, logger1;
		logger1 = logger = new StdoutLogger(Logger.DEBUG);
		logger1 = logger1.setNext(new EmailLogger(Logger.NOTICE));
		logger1 = logger1.setNext(new StderrLogger(Logger.ERR));
    
		// Handled by StdoutLogger
		logger.message("Entering function y.", Logger.DEBUG);

		// Handled by StdoutLogger and EmailLogger
		logger.message("Step1 completed.", Logger.NOTICE);

		// Handled by all three loggers
		logger.message("An error has occurred.", Logger.ERR);
	}
}
~~~

---
- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%EC%B1%85%EC%9E%84_%EC%97%B0%EC%87%84_%ED%8C%A8%ED%84%B4>
	+ <https://online.visual-paradigm.com/diagrams/examples/class-diagram/gof-design-patterns-chain-of-responsibility/>

