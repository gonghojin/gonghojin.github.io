---
layout: post
comments: true
title:  "Gof 디자인패턴_프록시(Proxy) 패턴"
date:   2019-12-02 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 프록시(Proxy) 패턴
### 의도
다른 객체에 대한 접근을 제어하기 위한 대리자 역할을 하는 객체를 둔다.  
### 동기
어떤 객체에 대한 접근을 제어하는 한 가지 이유는 `실제로 그 객체를 사용할 수 있을 때까지` 객체 생성과 초기화에 들어가는 비용 및 시간을 물지 않겠다는 것이다.  
그래픽 객체를 문서 안에 넣을 수 있는 문서 편집기를 예로 들어보자.  
문서가 읽히는 그 시점에 모든 내용(리소스가 큰 그래픽 객체)를 다 읽어올 필요는 없다.  
따라서 그래픽 객체를 프록시라는 또 다른 객체로 만들어, 필요할 때 인스턴스를 생성한다.  

### 구조
![alt](/assets/gof/images/gof-design-patterns-proxy.png)

+ Proxy
	- 실제로 참조할 대상에 대한 참조자를 관리한다.
		- RealSubject와 Subject 인터페이스가 동일하면 프록시는 Subject에 대한 참조자를 갖는다.
	- Subject와 동일한 인터페이스를 제공하여 실제 대상을 대체할 수 있어야 한다.
	- 실제 대상에 대한 접근을 제어하고 실제 대상의 생성과 삭제를 책임진다.  
	
~~~
Proxy의 종류
	- 원격지 프록시
		- 요청 메시지와 인자를 인코딩하여 이를 다른 주소 공간에 있는 실제 대상에게 전달한다.
	- 가상 프록시
		- 실제 대상에 대한 추가적 정보를 보유하여 실제 접근을 지연할 수 있도록 해야 한다.  
	- 보호용 프록시
		- 요청한 대상이 실제 요청할 수 있는 권한이 있는지 확인한다.
~~~
+ Subject
	- RealSubject와 Proxy에 공통적인 인터페이스를 정의하여, RealSubject가 요청되는 곳에 Proxy를 사용할 수 있게 한다.  
+ RealSubject
	- 프록시가 대표하는 실제 객체

### 협력 방법
프록시 클래스는 자신이 받은 요청을 RealSubject 객체에 전달한다.  

### 예제 코드
~~~java
interface Image {
	public void displayImage();
}

// on System A
class RealImage implements Image {
	private String filename;
	public RealImage(String filename) {
		this.filename = filename;
		loadImageFromDisk();
	}
	
	private void loadImageFromDisk() {
		System.out.println("Loading " + filename);
	}
	
	@Override
	public void displayImage() {
		System.out.println("Displaying " + filename);
	}
}

// on System B
class ProxyImage implements Image {
	private String filename;
	private Image image;
	
	public ProxyImage(String filename) {
		this.filename = filename;
	}
	
	@Override
	public void displayImage() {
		if (image == null) {
			image = new RealImage(filename);
		}
		
		image.displayImage();
	}
}

class ProxyExample {
	public static void main(String[] args) {
		Image image1 = new ProxyImage("HiRes_10MB_Photo1");
		Image image2 = new ProxyImage("HiRes_10MB_Photo2");

		image1.displayImage();
		image2.displayImage();
		
		/*
		Loading    HiRes_10MB_Photo1
		Displaying HiRes_10MB_Photo1
		Loading    HiRes_10MB_Photo2
		Displaying HiRes_10MB_Photo2
		*/
	}
}

~~~

### 관련 패턴
+ vs Adapter Pattern
	- Adapter Pattern
		- 개조할 객체가 정의된 인터페이스와 다른 인터페이스를 제공한다.
	- Proxy Pattern
		- 자신이 상대하는 대상과 동일한 인터페이스를 제공한다.
		- Target이 수행할 연산을 거부할 수도 있기 때문에, 처리 대상이 제공하는 인터페이스의 부분 집합일 수도 있다.
+ vs Decorator Pattern 
	- Decorator Pattern
		- 사용 목적이 하나 이상의 서비스를 추가하기 위해 사용
	- Proxy Pattern
		- 객체에 대한 접근을 제어하는 목적으로 사용
---
- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%ED%94%8C%EB%9D%BC%EC%9D%B4%EC%9B%A8%EC%9D%B4%ED%8A%B8_%ED%8C%A8%ED%84%B4>
	+ <https://online.visual-paradigm.com/diagrams/examples/class-diagram/gof-design-patterns-proxy/>
