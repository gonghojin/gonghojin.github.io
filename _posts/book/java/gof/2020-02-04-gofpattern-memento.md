---
layout: post
comments: true
title:  "Gof 디자인패턴_메멘토(Memento)"
date:   2020-02-04 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 메멘토(Memento) 패턴
### 의도
캡슐화를 위배하지 않은 채 어떤 객체의 내부 상태를 잡아내고 실체화시켜 둠으로써, 이후 해당 객체가 그 상태로 되돌아올 수 있도록 한다.
### 동기
때에 따라서는 `객체의 내부 상태`를 기록해 둘 필요가 있다. 그러나 객체는 자체적으로 상태의 일부나 전부를 캡술화하여 상태를 외부에 공개하지 않기 때문에, 다른 객체는 상태에 접근하지 못한다.  
이 상태를 외부에 노출시키는 것은 캡슐화를 위배하는 것이지만 응용프로그램의 `확장성 및 신뢰도`와 절충할 수있는 부분이기도 합니다.  

메멘토 패턴은 Originator 객체가 가진 내부 상태의 스냅샷을 저장하는 객체이다. 이것을 사용하면 Originator 객체의 상태를 확인할 필요가 있을 때 Originator이 메멘토에게 상태를 알려달라는 요청을 보낼 수 있다.  
Originator은 현재 상태를 알려주는 정보로 메멘토를 초기화한다. 메멘토에 정보를 저장하고 검색할 수 있는 것은 Originator의 자격을 가진 객체뿐이다.  
즉, 메멘토 이외의 다른 객체에게는 보이지 않는다.  

### 활용성  
+ 어떤 객체의 상태에 대한 스냅샷(몇 개의 일부)을 저장한 후 나중에 이 상태로 복구해야 할 때
+ 상태를 얻는 데 필요한 직접적인 인터페이스를 두면 그 객체의 구현 세부 사항이 드러날 수밖에 없고, 이것으로 객체의 캡슐화가 깨질 

### 구조
![alt](/assets/gof/images/gof-design-patterns-memento.png)

+ Memento
	- Originator 객체의 내부 상태를 필요한 만큼 저장해 둔다. 메멘토는 Originator 객체를 제외한 다른 객체는 자신에게 접근할 수 없도록 막는다.    
	그래서 Memento 클래스에는 사실상 두 종류의 인터페이스가 있다.  
		- Caretaker 클래스는 제한 범위 인터페이스만을 볼 수 있다.
		- Originator 클래스는 광범위 인터페이스가 보인다. 즉 모든 자료에 접근 가능하다.  
- Originator
	- 원조본 객체이다. 메멘토를 생성하여 현재 객체의 상태를 저장하고 메멘토를 사용하여 내부 상태를 복원한다.  
- Caretaker
	- 메멘토의 보관을 책임지는 보관자이다. 메멘토의 내용을 검사하거나 건드리지는 않는다.  

### 협력 방법
+ Caretaker(보관자) 객체는 Originator 객체에 메멘토 객체를 요청한다. 또 요청한 시간을 저장하며, 받은 메멘토 객체를 다시 Originator 객체에게 돌려준다.  
	하지만 Originator 객체가 이전 상태로 돌아갈 필요가 없을 때는, 메멘토 객체를 Originator 객체에 전달하지 않아도 된다.  
+ 메멘토 객체는 수동적이다. 메멘토 객체를 생성한 Originator 객체만이 상태를 설정하고 읽어올 수 있다.  

### 결과
1. 캡슐화된 경계를 유지할 수 있다.
	- Originator 객체만 메멘토를 다룰 수 있기 때문에 메멘토가 외부에 노출되지 않는다.  
	이 패턴은 복잡한 Originator 클래스의 내부 상태를 다른 객체로 분리하는 방법으로 상태에 대한 정보의 캡슐화를 보장  
2. Originator 클래스를 단순화할 수 있다.  
3. 메멘토의 사용으로 더 많은 비용이 들어갈 수도 있다.  
-	Originator 클래스가 많은 양의 정보를 저장해야 할 때나, 자주 메멘토를 반환해야 할 떄

### 예제 코드
~~~java
// Originator
class Originator {
	// memento에 저장되는 상태값
	private String state;
	
	public void setState(String state) {
		this.state = state;
		System.out.println("Originator: Setting state to " + state);
	}
	
	public Memento saveToMemento() {
		System.out.println("Originator: Saving to Memento.");
		return new Memento(this.state);
	}
	
	public void restoreFromMemento(Memento memento) {
		this.state = memento.getSavedState();
	}
	
	public static class Memento {
		private final String state;
		
		public Memento(String stateToSave) {
			this.state = stateToSave;
		}
		
		// 외부 클래스에서만 접근 가능
		private String getSavedState() {
			return state;
		}
	}
}

// Caretaker
class Caretaker {
	public static void main(String[] args) {
			List<Originator.Memento> savedStates = new ArrayList<Originator.Memento>();
			
			Originator originator = new Originator();
			originator.set("State1");
			originator.set("State2");
			savedStates.add(originator.saveToMemento());
			originator.set("State3");
			savedStates.add(originator.saveToMemento());
			originator.set("State4");
			
			originator.restoreFromMemento(savedStates.get(1));   
	}
}
/*
 출력 결과
 Originator: Setting state to State1
 Originator: Setting state to State2
 Originator: Saving to Memento.
 Originator: Setting state to State3
 Originator: Saving to Memento.
 Originator: Setting state to State4
 Originator: State after restoring from Memento: State3
*/

~~~

### 관련 패턴
Command(명령) 패턴은 실행 취소가 가능한 연산의 상태를 저장할 때, 메멘토 패턴을 사용할 수 있다.  
메멘토 패턴은 반복자 패턴에서의 반복 과정 상태를 관리할 수 있다.

---
- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://online.visual-paradigm.com/diagrams/templates/class-diagram/gof-design-patterns-memento/>
	+ <https://ko.wikipedia.org/wiki/%EB%A9%94%EB%A9%98%ED%86%A0_%ED%8C%A8%ED%84%B4>

