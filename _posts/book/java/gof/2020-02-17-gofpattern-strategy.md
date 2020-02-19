---
layout: post
comments: true
title:  "Gof 디자인패턴_전략(Strategy)"
date:   2020-02-17 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 전략(Strategy)
### 의도
동일 계열의 알고리즘군을 정의하고, 각 알고리즘을 캡슐화하며, 이들을 상호교환이 가능하도록 만듭니다.  
알고리즘을 사용하는 클라이언트와 상관없이 독립적으로 알고리즘을 다양하게 변경할 수 있게 한다.

### 활용성
다음 상황에서 전략 패턴을 사용할 수 있다.
+ 행동들이 조금씩 다를 뿐 개념적으로 관련된 많은 클래스들이 존재할 때.  
	전략 패턴은 많은 행동 중 하나를 가진 클래스를 구성할 수 있는 방법을 제공한다.
+ 사용자가 몰라야 하는 데이터를 사용하는 알고리즘이 있을 때.  
	노출하지 말아야 할 복잡한 자료 구조는 Strategy 클래스에만 두면 되므로 사용자는 몰라도 된다.  
+ 하나의 클래스가 많은 클래스가 많은 행동을 정의하고, 이런 행동들이 그 클래스의 연산 안에서 복잡한 다중 조건문의 모습을 취할 때.  
많은 조건문보다는 각각을 Stategy 클래스로 옮겨놓는 것이 좋다.

### 구조
![alt](/assets/gof/images/gof-design-patterns-strategy.png)

+ Strategy
	- 제공하는 모든 알고리즘에 대한 공통의 연산들을 인터페이스로 정의한다.  
		Context 클래스는 ConcreteStrategy 클래스에 정의한 인터페이스를 통해서 실제 알고리즘을 구현한다.  
+ ConcreteStrategy
	- Strategy 인터페이스를 실제 알고리즘으로 구현
+ Context
	- 객체를 통해 구성된다. 즉 Strategy 객체에 대한 참조자를 관리하고, 실제로는 Strategy 서브클래스의 인스턴스를 갖고 있음으로써 구체화한다.  
	또한 Strategy 객체가 자료에 접근해가는 데 필요한 인터페이스를 정의한다. 
	
### 협력 방법
+ Strategy 클래스와 Context 클래스는 의사교환을 통해 선택한 알고리즘을 구현한다.  
	즉, Context 클래스는 알고리즘에 해당하는 연산이 호출되면 알고리즘 처리에 필요한 모든 데이터를 Strategy 클래스로 보낸다.  
	이때, Context 객체 자체를 Strategy 연산에다가 인자로 전송할 수도 있다.   
+ Context 클래스는 사용자 쪽에서 온 요청을 각 전략 객체로 전달한다.  
	이를 위해서 사용자는 필요한 알고리즘에 해당하는 ConcreteStrategy 객체를 생성하여 이를 Context 클래스에 전송하는데, 이 과정을 거치면 사용자는 Context 객체와 동작할 때 전달한 ConcreteStrategy 객체와 함께 동작한다.  

### 결과
전략 패턴에는 다음과 같은 장점과 단점이 있다.  
+ 동일 계열의 관련 알고리즘군이 생긴다.
	- Strategy 클래스 계층은 동일 계열의 알고리즘군 혹은 행동군을 정의한다.  
	이런 알고리즘 자체의 재사용도 가능하다. 즉, 상속을 통해서 알고리즘 공통의 기능성들을 추출하고 이를 재사용할 수 있다.
+ 조건문을 없앨 수 있다.
	- 서로 다른 행동이 하나로 묶이면 조건문을 사용해서 정확한 행동을 선택해야 하지만, 서로 다른 Strategy 클래스의 행동을 캡슐화하면 이들 조건문을 없앨 수 있다.
+ Strategy 객체와 Context 객체 사이에 의사소통 오버헤드가 있다.
	- 서브클래스에서 구현할 알고리즘의 복잡함과는 상관없이 모든 ConcreteStrategy 클래스는 Strategy 인터페이스를 공유한다.  
	따라서 어떤 ConcreteStrategy 클래스는 이 인터페이스를 통해 들어온 모든 정보를 다 사용하지 않는데도 이 정보를 떠안아야 할 때가 있다.

### 예제 코드
~~~java
// Strategy Interface
public interface DragonSlayingStrategy {
  void execute();
}

// ConcreteStrategy 1
public class MeleeStrategy implements DragonSlayingStrategy {

  private static final Logger LOGGER = LoggerFactory.getLogger(MeleeStrategy.class);

  @Override
  public void execute() {
    LOGGER.info("With your Excalibur you sever the dragon's head!");
  }
}

// ConcreteStrategy 2
public class ProjectileStrategy implements DragonSlayingStrategy {

  private static final Logger LOGGER = LoggerFactory.getLogger(ProjectileStrategy.class);

  @Override
  public void execute() {
    LOGGER.info("You shoot the dragon with the magical crossbow and it falls dead on the ground!");
  }
}

// Context 
public class DragonSlayer {
	private DragonSlayingStrategy strategy;
	
	public DragonSlayer(DragonSlayingStrategy strategy) {
		this.strategy = strategy;
	}

	public void chageStrategy(DragonSlayingStrategy strategy) {
		this.strategy = strategy;
	}
	
	public void goToBattle() {
		strategy.execute();
	}
}
~~~
~~~java
public class StrategyEx {
	public static void main(String[] args){ 
		DragonSlayer dragonSlayer = new DragonSlayer(new MeleeStrategy());
		dragonSlayer.goToBattle();
		
		dragonSlayer.changeStrategy(new ProjectileStrategy());
		dragonSlayer.goToBattle();
	}
}
~~~

---

- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%EC%A0%84%EB%9E%B5_%ED%8C%A8%ED%84%B4>
	+ <https://github.com/iluwatar/java-design-patterns>
