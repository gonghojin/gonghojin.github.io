---
layout: post
comments: true
title:  "Gof 디자인패턴_템플릿 메서드(Template Method)"
date:   2020-02-19 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 템플릿 메서드(Template Method)
### 의도
객체의 연산에는 알고리즘의 뼈대만을 정의하고 각 단계에서 수행할 구체적 처리는 서브클래스 쪽으로 미룬다.  
알고리즘의 구조 자체는 그대로 놔둔 채 알고리즘 각 단계 처리를 서브클래스에서 재정의할 수 있게 한다.

### 활용성
다음 상황에서 템플릿 메서드를 사용할 수 있다.
+ 어떤 한 알고리즘을 이루는 부분 중 변하지 않는 부분을 한 번 정의해 놓고 다양해질 수 있는 부분은 서브클래스에서 정의할 수 있도록 남겨두고자 할 때  
+ 서브클래스 사이의 공통적인 행동을 추출하여 하나의 공통 클래스에 몰아둠으로써 코드 중복을 피하고 싶을 

### 구조
![alt](/assets/gof/images/gof-design-patterns-templateMethod.png)

+ AbstractClass
	- 서브클래스들이 재정의를 통해 구현해야 하는 알고리즘 처리 단계 내의 `기본 연산`을 정의한다.  
	그리고 알고리즘의 뼈대를 정의하는 템플릿 메서드를 구현한다. 템플릿 메서드는 AbstractClass에 정의된 연산 또는 다른 객체 연산뿐만 아니라 기본 연산도 호출한다.
+ ConcreteClass
	- 서브클래스마다 달라진 알고리즘 처리 단계를 수행하기 위 기본 연산을 구현한다.
	
### 결과
템플릿 메서드는 코드 재사용을 위한 기본 기술이다. 특히 클래스 라이브러리 구현 시 중요한 기술인데, 이는 라이브러리에 정의할 클래스들의 공통 부분을 분리하는 수단이기 때문이다.

### 예제 코드
~~~java
public abstract class StealingMethod {
	protected abstract String pickTarget();
	protected abstract void confuseTarget(String target);
	protected abstract void stealTheItem(String target);
	
	public void steal() {
		String target = pickTarget();
		confuseTarget(target);
		stealTheItem(target);
	}
}

public class SubtleMethod extends StealingMethod {
	@Override
	protected String pickTarget() {
		return "shop keeper";
	}
	
	@Override
	protected void confuseTarget(String target) {
	}
	
	@Override
	protected void stealTheItem(String target) {
	}
}

public class HitAndRunMethod extends StealingMethod {
	@Override
	protected String pickTarget() {
		return "old goblin woman";
	}
	
	@Override
	protected void confuseTarget(String target) {
	}
	
	@Override
	protected void stealTheItem(String target) {
	}
}

public class HalflingThief {
	private StealingMethod method;
	
	public HalflingThief(StealingMethod method) {
		this.method = method;
	}
	
	public void steal() {
		method.steal();
	}
	
	public void changeMethod(StealingMethod method) {
		this.method = method;
	}
}

public class TemplateMethodEx {
	public static void main(String[] args){
	  HalflingThief halflingThief = new HalflingThief(new HitAndRunMethod());
	  halflingThief.steal();
	  halflingThief.changeMethod(new SubtleMethod());
	  halflingThief.steal();
	}
}
~~~

### 관련 패턴
템플릿 메서드는 거의 모든 추상 클래스에서 사용할 정도로 필수적이고 기본적인 패턴이다.  
팩토리 메서드 패턴을 종종 템플릿 메서드 패턴이라고도 한다. 템플릿 메서드 패턴은 상속을 이용하여 다양한 알고리즘을 만들어 낸다는 점에서는 전략 패턴과 관계가 있으며, 각 전략들은 위임을 통하여 전체 알고리즘을 다양화한다.  
  
  
---

- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%ED%85%9C%ED%94%8C%EB%A6%BF_%EB%A9%94%EC%86%8C%EB%93%9C_%ED%8C%A8%ED%84%B4>
	+ <https://github.com/iluwatar/java-design-patterns>
