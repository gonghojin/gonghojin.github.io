---
layout: post
comments: true
title:  "Gof 디자인패턴_감시자(Observer)"
date:   2020-02-06 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 감시자(Observer) 패턴
### 의도
객체 사이에 일 대 다의 의존 관계를 정의해 두어, 어떤 객체의 상태가 변할 때 그 객체에 의존성을 가진 다른 객체들이 그 변화를 통지받고 자동으로 갱신될 수 있게 만든다.
### 동기
어떤 하나의 시스템을 서로 연동되는 클래스 집합으로 분할했을 때 발생하는 공통적인 부작용은 `관련된 객체 간에 일관성을` 유지하도록 해야 한다는 것이다.  
일관성 관리를 위해서 객체 간의 결합도를 높이는 것은, 각 클래스의 재사용성이 떨어지기 때문에 비효율적이다.  

감시자 패턴은 이런 관련성을 관리하는 패턴입니다. 이 패턴에서 중요한 객체는 주체(subject)와 감시자(observer)이다.  
주체는 독립된 여러 개의 감시가 있을 수 있다. 모든 감시자는 `주체의 상태 변화가 있을 때마다` 이 변화를 통보 받는다. 각 감시자는 주체의 상태와 자신의 상태를 동기화시키기 위해 주체의 상태를 알아본다.  
이런 종류의 상호작용을 `Publish-Subscribe(게시-구독) 관계`라고 한다.

### 활용성  
다음 상황 중 어느 한 가지에 속하면 감시자 패턴을 사용한다.  
+ 어떤 추상 개념이 두 가지 양상을 갖고, 하나가 다른 하나에 종속적일 때.  
	각 양상을 별도의 객체로 캡슐화하여 이들 각각을 재사용할 수 있다.
+ 한 객체에 가해진 변경으로 다른 객체를 변경해야 하고, 프로그래머들은 얼마나 많은 객체들이 변경되어 하는지 몰라도 될 때

### 구조
![alt](/assets/gof/images/gof-design-patterns-observer.png)

+ Subject(주체)
	- Observer(감시자)들을 알고 있는 주체이다. 임의 개수의 Observer object(감시자 객체)는 Subject를 감시할 수 있다.  
		 Subject는 Observer Object를 붙이거나 떼는 데 필요한 인터페이스를 제공한다.
+ Observer(감시자)
	- 주체에 생긴 변화에 관심 있는 객체를 갱신하는 데 필요한 인터페이스를 정의한다.		
	이로써 주체의 변경에 따라 변화되어야 하는 객체들의 일관성을 유지한다.
+ ConcreteObserver
	- 주체의 상태와 일관성을 유지해야 하는 상태를 저장한다. 주체의 상태와 감시자의 상태를 일관되게 유지하는 데 사용하는 갱신 인터페이스를 구현
 
### 결과
감시자 패턴을 사용하게 되면 주체 및 감시자 모두를 독립적으로 변형하기 쉽다. 감시자를 재사용하지 않고도 주체를 재사용할 수 있고, 주체 없이도 감시자를 재사용할 수 있다.  
또한 주체나 감시자의 수정 없이도 감시자를 추가할 수 있다.  
추가적으로 감시자 패턴을 사용하면 다음과 같은 이점이 있다.
- Subject와 Observer 클래스 간에는 추상적인 결합도만이 존재한다.
	- 주체가 아는 것은 감시자들의 리스트뿐이다. 이 감시자들은 Observer 클래스에 정의된 인터페이스만 따른다.  
	그러나 주체는 어떤 ConcreteObserver 클래스가 있는지에 대해서는 알 필요가 없다. 그러므로 주체와 감시자 간의 결합은 추상적이다.
- 브로드캐스트(broadcast) 방식의 교류를 가능하게 한다.
	- 일반적인 요청과 달리, 감시자 패턴에서 주체가 보내는 통보는 구체적인 수신자를 지정할 필요가 없다. 이 통보는 주체의 정보를 원하는 모든 객체에 자동으로 전달되어야 한다.
### 예제 코드
~~~java
// 주
...
import java.util.Observable; // 이 부분이 옵저버에게 신호를 보내는 '주체'
public class EventSource extends Observable implements Runnable {
	public void run() {
		try {
		 	final InputStreamReader isr = new InputStreamReader(System.in);
			final BufferedReader br = new BufferedReader(isr);
			while( true )
			{
				final String response = br.readLine();
				setChanged();
				notifyObservers( response );
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
// 옵서버
public class ResponseHandler implements Observer {
	private String resp;
	public void update (Observable obj, Object arg) { 
		if (arg instanceof String) {
			resp = (String) arg;
			System.out.println("\nReceived Response: "+ resp ); 
		}
	}
}

public class ObserverExample {
	public static void main(String[] args){
		// 이벤트 발행 주체를 생성
	  final EventSource eventSource = new EventSource();
		
	  // 옵서버 생성
		final ResponseHandler responseHandler = new ResponseHandler();
		
		// 옵저버가 발행 '주체'가 발행하는 이벤트를 '구독'하게 함
		eventSource.addObserver(responseHandler);
		
		// 이벤트를 발행시키는 쓰레드 시작
		Thread thread = new Thread(eventSource);
		thread.start();
	}
}
~~~

---

- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%EC%98%B5%EC%84%9C%EB%B2%84_%ED%8C%A8%ED%84%B4>

