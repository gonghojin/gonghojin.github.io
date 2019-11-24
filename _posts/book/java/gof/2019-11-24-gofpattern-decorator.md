---
layout: post
comments: true
title:  "Gof 디자인패턴_데코레이터(Decorator) 패턴"
date:   2019-11-24 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 데코레이터(Decorator) 패턴
### 의도
객체에 `동적으로 새로운 책임을` 추가할 수 있게 해준다. 기능을 추가하려면, 서브 클래스를 생성하는 것보다 융통성 있는 방법을 제공한다.  

### 동기
가끔 전체 클래스에 새로운 기능을 추가할 필요는 없지만, 개별적인 객체에` 새로운 책임을` 추가할 필요가 있다.  
이렇게 새로운 서비스의 추가가 필요할 때 이를 해결하는 일반적인 방법은 상속이지만, `동적으로 확장이 필요한 상황`에서는 별로 유용하지는 않다.  

더 나은 방법은 기존 객체에 `새로운 책임을 가진 객체`를 동적으로 감싸는 방법인데, 이렇게 무엇인가를 감싸는 객체를 `데코레이터(Decorator)`라고 한다.  

### 구조
![alt](/assets/gof/images/gof-design-patterns-decorator.png)
+ Component: 동적으로 추가할 서비스를 가질 가능성이 있는 객체들에 대한 `인터페이스`
+ ConcreteComponent: 추가적인 서비스가 `실제로 정의`해야 할 필요가 있는 객체
+ Decorator: Component 객체에 대한 참조자를 관리하면서, Component에 정의된 인터페이스를 만족하도록 인터페이스를 정의
+ ConcreteDecorator: Component에 새롭게 추가할 서비스를 실제로 구현하는 클래스  

### 협력 방법
+ Decorator는 자신의 Component 객체 쪽으로 요청을 전달합니다. 요청 전달 전 및 전달 후에 자신만의 추가 연산을 선택적으로 수행할 수도 있다.

### 결과
##### 장점
1. 단순한 상속보다 설계의 융통성을 더 많이 증대시킬 수 있다.
	+ 데코레이터 패턴은 객체에 새로운 행동을 추가할 수 있는 가장 효과적인 방법
		+ `상속과 달리,` 런타임에 새로운 책임 부여가 가능  

2. 클래스 계통의 상부측 클래스에 많은 기능이 누적되는 상황을 피할 수 있다.  
	+ 데코레이터 패턴은 책임 추가 작업에서 `"필요한 비용만 그때 지불하는"` 방법을 제공한다.
		+ 지금 예상하지 못한 특성들을 한꺼번에 다 개발하기 위해 고민하고 노력하기 보다는   
			발견하지 못하고 누락된 서비스들은 Decorator 객체를 통해 `지속적으로 추가`할 수 있다.

##### 단점
1. 장식자를 사용함으로써 `작은 규모의 객체들이` 많이 생긴다.  
	+ 클래스들이 어떻게 조합하여 새로운 모습과 기능을 만들어내는가에 따라서 새로운 객체가 계속 만들어 진다.  
		+ 이 객체들을 잘 이해하고 있다면 시스템의 재정의가 쉽겠지만, 그렇지 않다면 객체들을 이해하고 수정하는 과정이 복잡해진다.

### 예제 코드
+ Component & ConcreteComponent  

~~~java
// Component
interface Window {
	public void draw();
	public String getDescription();
}

// ConcreteComponent
class SimpleWindow implements Window {
	public void draw() {
		// draw Window
	}
	
	public String getDescription() {
		return "simple window";
	}
}
~~~

+ Decorator
	- 아래의 클래스들은 모든 `Window` 클래스들의 데코레이터를 포함하고 있다.  
	
~~~java
// abstract decorator class - Window를 구현하고 있는 점을 주목하자.
abstract class WindowDecorator implements Window {
	protected Window decoratedWindow; // the Window being decorated
	
	public WindowDecorator(Window decoratedWindow) {
		this.decoratedWindow = decoratedWindow;
	}
}

// vertical scrollbar 기능을 추가하는 concrete decorator 1
class VerticalScrollBarDecorator extends WindowDecorator {
	public VerticalScrollBarDecorator (Window decoratedWindow) {
		super(decoratedWindow);
	}
	
	@Override
	public void draw() { 
		drawVerticalScrollBar();
		decoratedWindow.draw(); // * 
	}
	
	private void drawVerticalScrollBar() {
		// draw the vertical scrollbar
		// 새로운 기능을 부여
	}
	
	@Override
	 public String getDescription() {
		return decoratedWindow.getDescription() + ", including vertical scrollbars";
	}
}

// horizontal scrollbar 기능을 추가하는 concrete decorator 2
class HorizontalScrollBarDecorator extends WindowDecorator {
    public HorizontalScrollBarDecorator (Window decoratedWindow) {
        super(decoratedWindow);
    }
		
    @Override
    public void draw() {
        drawHorizontalScrollBar();
        decoratedWindow.draw();
    }

    private void drawHorizontalScrollBar() {
    	// draw the horizontal scrollbar
    	// 새로운 기능을 부여
    }
		
    @Override
    public String getDescription() {
        return decoratedWindow.getDescription() + ", including horizontal scrollbars";
    }
}
~~~

+ App  

~~~java
public class DecoratedWindowTest {
	public static void main(String[] args) {
		// create a decorated Window with horizontal and vertical scrollbars
		// horizontal and vertical scrollbars로 데코레이터된 Window 객체 생성
		Window decoratedWindow = new HorizontalScrollBarDecorator (
			new VerticalScrollBarDecorator(new SimpleWindow())
		);
	
		// print the Window's description
		System.out.println(decoratedWindow.getDescription());
	}
}
~~~

### 관련 패턴
+ Adapter pattern
	+ Decorator 패턴은 일종의 Adapter 패턴이라고 할 수 있다.
		+ Adapter 패턴은 인터페이스를 변경시켜주는 것
		+ Decorator 패턴은 객체의 책임, 행동을 변경시켜주는 것  

+ Composite pattern
	+ Decorator는 한 구성 요소만을 갖는 Composite로 볼 수 있다.
		+ 그러나 이 목적은 `객체의 합성`이 아니라 `객체에 새로운 행동`을 추가하기 위한 것

+ Strategy pattern
	+ Decorator는 객체의 겉모양을 변경
	+ Strategy는 객체의 내부를 변경

---
- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%BD%94%EB%A0%88%EC%9D%B4%ED%84%B0_%ED%8C%A8%ED%84%B4>
	+ <https://online.visual-paradigm.com/diagrams/examples/class-diagram/gof-design-patterns-decorator/>
