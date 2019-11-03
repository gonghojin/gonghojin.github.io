---
layout: post
comments: true
title:  "Gof 디자인패턴_빌더"
date:   2019-11-03 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 빌더(Builder)
### 의도
복잡한 객체를 `생성하는 방법과 표현하는 방법`을 정의하는 클래스를 별도로 분리하여, 서로 다른 표현이라도 이를 생성할 수 있는 `동일한 절차`를 제공할 수 있도록 한다.

### 활용성
+ 복합 객체의 생성 알고리즘이 이를 합성하는 요소 객체들이 무엇인지 이들의 조립 방법에 독립적일 때
+ 합성할 객체들의 표현이 서로 다르더라도 생성 절차에서 이를 지원해야 할 때

### 구조
![Image Alt 텍스트](/assets/gof/images/gof-design-patterns-builder.png)

+ Builder: Product 객체의 일부 요소들을 생성하기 위한 추상 인터페이스를 정의
+ ConcreteBuilder: Builder 클래스에 정의된 인터페이스를 구현하며, 제품의 부품들을 모아 빌더를 복합한다.  
	생성한 요소의 표현을 정의하고 관리합니다. 또한 제품을 검색하는 데 필요한 인터페이스를 제공한다. 
+ Director: Builder 인터페이스를 사용하는 객체를 합성한다.
+ Product: 생성할 복합 객체를 표현한다. ConcreteBuilder는 제품(Product)의 내부 표현을 구축하고 복합 객체가 어떻게 구성되는지에 관한 절차를 정의한다.

### 협력 방법
+ 사용자는 Director 객체를 생성하고, 이렇게 생성한 객체를 자신이 원하는 Builder 객체로 합성해 나간다.
+ 제품의 일부가 구축(built)될 때마다 Director는 Builder에 통보한다.
+ Builder는 Director의 요청을 처리하여 제품에 부품을 추가한다.
+ 사용자는 Builder에서 제품을 검색한다.

### 결과
장점
1. 제품에 대한 내부 표현을 다양하게 변화할 수 있다.
	- Builder 객체는 디렉터를 제공하고 제품을 복합하기 위해 필요한 추상 인터페이스를 정의한다.   
	즉, 어떤 요소로 전체 제품을 복합하고 그 요소들이 어떤 타입들로 구현되는지 알고 있는 쪽은 빌더 뿐이다.
		- 제품을 복합할 때는 `빌더에 정의한 추상 인터페이스를 통해 사용자가 동작하기 때문에`, 새로운 제품의 표현 방법이나 제품의 복합 방법이 바뀔 떄
		추상 인터페이스를 정의한 Builder 클래스에서 상속을 통해 새로운 서브클래스를 정의하면 된다.

2. 생성과 표현에 필요한 코드를 분리한다.
	- 빌더 패턴을 사용하면, 복합 객체를 생성하고 복합 객체의 내부 표현 방법을 별도의 모듈로 정의할 수 있다.  
	사용자는 제품의 내부 구조를 정의한 클래스는 전혀 모른 채(모듈화), 빌더와 상호작용을 통해서 필요한 복합 객체를 생성한다.

3. 복합 객체를 생성하는 절차를 좀더 세밀하게 나눌 수 있다.  
	- 한번에 복합 객체를 생성하는 것처럼, 빌더 패턴은 디렉터의 통제 아래 하나씩 내부 구성요소들을 만들어 나간다.  
	그렇기 떄문에 Builder 클래스의 인터페이스에는 이 제품을 생성하는 과정 자체가 반영되어 있다.

### 구현
1. 추상 클래스인 Builder 클래스에 디렉터가 요청하는 각각의 요소들을 생성하는 연산들을 정의한다.
	- 기본적으로 여기 정의된 연산은 아무것도 구현되지 않은 단순한 인터페이스이다.
		- 이 클래스를 상속하는 서브클래스 ConcreteBuilder가 자신에게 필요한 요소를 생성하도록 재정의한다.  

2. 조합과 구축에 필요한 인터페이스를 정의한다.
	- 빌더는 단계별로 제품들을 생성한다. 이를 위해서 모든 종류의 제품을 생성하는 데 필요한 일반화된 연산들을 정의한다.


### 예제코드
+ Product  

~~~java
class Pizza {
	private String dough = "";
	private String sauce = "";
	private String topping = "";

	// setter
}
~~~

+ Abstract Builder

~~~java
abstract class PizzaBuilder {
	protected Pizza pizza;
	
	public Pizza getPizza() {
		return pizza;
	}
	
	public void creaateNewPizzaProduct() {
		pizza = new Pizza();
	}

	public abstract void buildDough();
	public abstract void buildSauce();
	public abstract void buildTopping();
	
}
~~~

+ ConcreteBuilder

~~~java
class HawaiianPizzaBuilder extends PizzaBuilder {
	public void buildDough() {
		pizza.setDough("cross");
	}

	public void buildSauce() {
		pizza.setSauce("mild");
	}

	public void buildTopping() {
		pizza.setTopping("ham+pineapple");
	}
}


class SpicyPizzaBuilder extends PizzaBuilder {
	public void buildDough() {
		pizza.setDough("pan baked");
	}

	public void buildSauce() {
		pizza.setSauce("hot");
	}

	public void buildTopping() {
		pizza.setTopping("pepperoni+salami");
	}
}
~~~

+ Director

~~~java
class Cook {
	private PizzaBuilder pizzaBuilder;
	
	public void setPizzaBuilder(PizzaBuilder pizzaBuilder) {
		this.pizzaBuilder = pizzaBuilder;
	}

	public Pizza getPizza() {
		return pizzaBuilder.getPizza();
	}
	
	public void constructPizza() {
		pizzaBuilder.createNewPizzaProduct();
		pizzaBuilder.buildDough();
		pizzaBuilder.buildSauce();
		pizzaBuilder.buildTopping();
	}
}
~~~

+ 생성할 피자 타입

~~~java
public class BuilderExample {
	public static void main(String[] args){
	  Cook cook = new Cook();
	  PizzaBuilder hawaiianPizzaBuilder = new HawaiianPizzaBuilder();
	  PizzaBuilder spicyPizzaBuilder = new SpicyPizzaBuilder();
	  
	  cook.setPizzaBuilder(hawaiianPizzaBuilder);
	  cook.constructPizza();
	  
	  Pizza pizza = cook.getPizza();
	}
}
~~~

---
- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%EB%B9%8C%EB%8D%94_%ED%8C%A8%ED%84%B4>
