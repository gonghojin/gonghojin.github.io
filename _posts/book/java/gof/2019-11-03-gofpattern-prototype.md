---
layout: post
comments: true
title:  "Gof 디자인패턴_프로토타입"
date:   2019-11-03 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 프로토타입(prototype)
### 의도
원형이 되는(prototypical) 인스턴스를 사용하여 생성할 객체의 종류를 명시하고, 이렇게 만든 `견본을 복사해서` 새로운 객체를 생성

### 활용성
+ 프로토타입 패턴은 제품의 생성, 복합, 표현 방법에 독립적인 제품을 만들고자 할 때 쓴다. 
+ 인스턴스화할 클래스를 `런타임`에 지정할 때(ex: 동적 로딩)
+ 제품 클래스 계통과 병렬적으로 만드는 팩토리 클래스를 피하고 싶을 때

프로토 타입으로 초기화해 두고, 나중에 이를 복제해서 사용하는 것이 매번 필요한 상태 조합의 값들을 수동적으로 초기화하는 것보다 더 편리할 수도 있다.

### 구조
![click](/assets/gof/images/gof-design-patterns-prototype.png)

+ Prototype: 자신을 복제하는 데 필요한 인터페이스를 정의
+ ConcretePrototype: 자신을 복제하는 연산을 구현
+ Client: Prototype에 자기 자신의 복제를 요청하여 새로운 객체를 생성

### 협력 방법
Client는 원형 클래스에 스스로를 복제하도록 요청한다.

### 결과
원형 패턴은 `추상 팩토리 및 빌더`와 비슷한 결과를 가진다. Client는 어떠한 구체적인 제품이 있는 알 필요가 없기 때문에, 다루어야 할 클래스의 수가 적다.
###### 추가적 특성

+ 런타임에 새로운 제품을 추가하고 삭제할 수 있다.
	- Client에게 프로토타입으로 생성되는 인스턴스를 등록하는 것만으로도 시스템에 새로운 제품 클래스를 추가할 수 있다.  
	런타임에 새로운 프로토타입을 넣고 빼기 용이하다는 점에서 다른 생성 패턴에 비해 유연성을 가진다. 

+ 값들을 다양화함으로써 새로운 객체를 명세합니다.
+ 구조를 다양화함으로써 새로운 객체를 명세할 수 있다.
	- 구성요소와 부분 구성요소의 복합을 통해 새로운 객체를 구축한다.
+ 서브클래스의 수를 줄인다.
	- 프로토타입 패턴에서는 프로토타입을 복제하는 것이므로, 팩토리 메서드에서 새로운 객체를 만들어 달라고 요청하는 과정이 불필요하다.
+ 동적으로 클래스에 따라 응용프로그램을 설정할 수 있다.
	
### 구현
1. 프로토타입 관리자(레지스트리)를 사용한다.
	- 프로토타입의 수가 아직 정해지지 않은 때라면, 가능한 프로토타입이 등록된 레지스트리를 관리해야 한다.  
	Client는 프로토타입 자체를 다루지는 않으며, 단지 레지스트리에서 프로토타입을 검색하고 그것을 레지스트리에 저장만 하면 된다.

2. Clone() 연산을 구현한다.
3. Clone을 초기화한다.

### 예제 코드
+ Prototype class
~~~java
public class Cookie implements Cloneable {
	public Object clone() {
		try {
			Cookie copy = (Cookie) super.clone();
	
			return copy;
		} catch (CloneNotSupportedException e) {
			e.printStackTrace();
			return null;
		}
	}
}
~~~
+ 복제하기 위한 Concrete Prototypes 
~~~java
public class CoconutCookie extends Cookie {}
~~~

+ Client class
~~~java
public class CookieMachine {
	private Cookie cookie;
	
	public CookieMachine(Cookie cookie) {
		this.cookie = cookie;
	}
	
	public Cookie makeCookie() {
		return (Cookie) cookie.clone();
	}
	
	public Object clone() {}
	
	public static void main(String[] args){
		Cookie tempCookie = null;
		Cookie prot = new CoconutCookie();
		CookieMachine cm = new CookieMachine(prot);
		for (int i = 0; i < 100; i++) {
			tempCookie = cm.makeCookie();
		}
	}
}
~~~

---
- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85_%ED%8C%A8%ED%84%B4>
