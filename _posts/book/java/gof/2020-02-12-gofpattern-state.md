---
layout: post
comments: true
title:  "Gof 디자인패턴_상태(State)"
date:   2020-02-12 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 상태(State) 패턴
### 의도
객체의 내부 상태에 따라 스스로 행동을 변경할 수 있게 허가하는 패턴으로, 이렇게 하면 객체는 마치 자신의 클래스를 바꾸는 것처럼 보인다.

### 활용성
다음 상황 가운데 하나에 속하면 상태 패턴을 사용할 수 있다.
+ 객체의 행동이 상태에 따라 달라질 수 있고, 객체의 상태에 따라서 런타임에 행동이 바뀌어야 한다.
+ 어떤 연산에 그 객체의 `상태에 따라 달라지는 다중 분기 조건 처리`가 많이 들어 있을 때. 객체의 상태를 표현하기 위해 상태를 하나 이상의 열거형 상수(enumerated constant)로 정의해야 한다.  
	동일한 조건 문장들을 하나 이상의 연산에 중복 정의해야 할 떄도 잦다. 이때, 객체의 상태를 별도의 객체로 정의하면, 다른 객체들과 상관없이 그 객체의 상태를 다양화시킬 수 있다.  
	
### 구조
![alt](/assets/gof/images/gof-design-patterns-state.png)  

+ Context
	- 사용자가 관심있는 인터페이스를 정의하며, 객체의 현재 상태를 정의한 ConcreteState 서브클래스의 인스턴스를 유지,관리한다.
+ State
	- Context의 각 상태별로 필요한 행동을 캡슐화하여 인터페이스로 정의
+ ConcreteState 서브클래스
	- 각 서브클래스들은 Context의 상태에 따라 처리되어야 할 실제 행동을 구현한다.

### 협력 방법
+ 상태에 따라 다른 요청을 받으면 Context 클래스는 현재의 ConcreteState 객체로 전달한다.  
	이 ConcreteState 클래스의 객체는 State 클래스를 상속하는 서브클래스들 중 하나의 인스턴스이다.
+ Context 클래스는 실제 연산을 처리할 State 객체에 자신을 매개변수로 전달한다.  
	이로써 State 객체는 Context 클래스에 정의된 정보에 접근할 수 있다.
+ Contxt 클래스는 사용자가 사용할 수 있는 기본 인터페이스를 제공한다.  
	사용자는 상태 객체를 Context 객체와 연결시킨다. 즉, Context 클래스에 현재 상태를 정의한다.  
	이렇게 Context 객체를 만들고 나면 사용자는 더는 State 객체를 직접 다루지 않고 Context 객체에 요청을 보내기만 하면 된다.

### 결과
상태 패턴을 사용했을 때 나타나는 효과는 다음과 같다.  
+ 상태에 따른 행동을 국소화하며, 서로 다른 상태에 대한 행동을 별도의 객체로 관리한다.
	- 상태 패턴을 사용하면 임의의 한 상태에 관련된 모든 행동을 하나의 객체로 모을 수 있다.  
	한 상태에 종속적인 코드를 State 클래스의 서브클래스에 모두 정의하였기 때문에 새로운 상태와 새로운 전이 규칙이 발견되면 새로운 서브 클래스만 정의하면 된다.  
+ 상태 전이를 명확하게 만듭니다.  
	- 어떤 객체가 자신의 현재 상태를 오직 내부 데이터 값으만 정의하면, 상태 전이는 명확한 표현을 갖지 못한다.  
	단지 이 상태 변수에 값을 할당하는 문장 정도밖에는 되지 않습니다. 반면에 각 상태별로 별도의 객체를 만드는 것은 상태 전이를 명확하게 해 주는 결과가 된다.
+ 상태 객체는 공유될 수 있다.

~~~java
interface StateLike {
	void writeName(StateContxt contxt, String name);
}


class StateLowerCase implements StateLike {
	@Override
	public void writeName(final StateContext context, final String name) {
		System.out.println(name.toLowerCase());
		context.setState(new StateMultipleUpperCase());
	}
}

class StateMultipleUpperCase implements Statelike {
	private int count = 0;

	@Override
	public void writeName(final StateContext context, final String name) {
		System.out.println(name.toUpperCase());
		// 2번 사용 후 State 변
		if(++count > 1) {
			context.setState(new StateLowerCase());
		}
	}
}

class StateContext {
	private StateLike stateLike;
	StateContext() {
		this.setStateLike(new StateLowerCase());
	}
	
	void setStateLike(final StateLike newState) {
		this.stateLike = newState;
	}
	
	public void writeName(final String name) {
		this.stateLike.writeName(this, name);
	}
}

public class DemoOfClientState {
	public static void main(String[] args) {
		final StateContext sc = new StateContext();

		sc.writeName("Monday");
		sc.writeName("Tuesday");
		sc.writeName("Wednesday");
		sc.writeName("Thursday");
		sc.writeName("Friday");
		sc.writeName("Saturday");
		sc.writeName("Sunday");
	}
}
~~~

~~~
결과>
	monday
	TUESDAY
	WEDNESDAY
	thursday
	FRIDAY
	SATURDAY
	sunday
~~~

### 관련 패턴
상태 객체의 공유 시점과 공유 방법을 정의하는 데에 플라이급 패턴을 사용한다.  
상태 객체는 종종 하나만 존재할 때가 많은데, 이떄는 단일체이다.

---

- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%EC%83%81%ED%83%9C_%ED%8C%A8%ED%84%B4>

