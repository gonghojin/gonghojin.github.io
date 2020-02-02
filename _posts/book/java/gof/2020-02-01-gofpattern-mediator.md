---
layout: post
comments: true
title:  "Gof 디자인패턴_중재자(Mediator)"
date:   2020-02-01 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 중재자(Mediator) 패턴
### 의도
한 집합에 속해있는 `객체의 상호작용을 캡슐화하는 객체`를 정의한다. 객체들이 직접 서로를 참조하지 않도록 하여 결합도를 낮추며, 개발자가 객체의 상호작용을 독립적으로 다양화시킬 수 있게 해준다.
### 동기
객체지향 개발 방법론에서는 `행동을 여러 객체에` 분산시켜 처리하도록 권하고 있다. 여러 객체로 분할하게 되면 객체의 재사용성을 증대시킬 수 있지만, 이러한 분산으로 객체 사이에 수많은 연결 관계가 형성된다.  
또한 객체 간 상호작용의 급증은 오히려 재사용성을 저해할 수도 있다. 이때 상호작용과 관련된 행동을 하나의 객체로 모으는 `중재자 객체`를 활용함으로써, 객체 그룹간의 상호작용을 제어하고 조화를 이룰 수 있다.  
즉, 객체 사이의 연결 정도를 줄일 수 있다.

### 활용성  
+ 여러 객체가 잘 정의된 형태기이는 하지만 `복잡한 상호작용`을 가질 때, 객체 간의 의존성이 구조화되지 않으며, 잘 이해하기 어려울 때 
+ 한 객체가 다른 객체를 `너무 많이 참조하고 많은 상호작용`으로 인해 그 객체를 재사용하기 힘들 때
+ 여러 클래스에 분산된 행동들이 상속없이 상황에 맞게 수정되어야 할 때  

### 구조
![alt](/assets/gof/images/gof-design-patterns-mediator.png)

+ Mediator
	- Colleague 객체와 교류하는 데 필요한 인터페이스를 정의
+ ConcreteMediator
	- Colleague 객체와 조화를 이뤄서 협력 행동을 구현히며, 참조할 Colleague 객체를 파악하고 관리
+ Colleague 클래스들
	- 자신의 중재자 객체가 무엇인지 파악한다.  
		다른 객체와 협력이 필요하면 그 중재자를 통해 협력할 수 있도록 하는 역할

### 협력 방법
+ Colleague는 Mediator에서 요청을 송수신한다. Mediator는 필요한 Colleague 사이에 요청을 전달할 책임을 갖는다.  

### 결과
##### 장점
1. 서브클래싱을 제한
	- 중재자는 다른 객체 사이에 분산된 객체의 행동들을 하나의 객체로 국한한다.  
	이 행동을 변경하고자 한다면 Mediator 클래스를 상속하는 서브클래스만 만들면 된다.  
	(Colleague 클래스는 여전히 재사용 가능)  
2. Colleague 객체 사이의 종속성을 줄인다.  
3. 객체 프로토콜을 단순화  
	- 중재자는 다 대 다의 관계를 일 대 다의 관계로 축소한다.  
	일 대 다의 관계가 훨씬 이해하기 쉬울 뿐만 아니라 유지하거나 확장하기 쉽다.  
4. 객체 간의 협력 방법을 추상화한다.  
	- 객체 사이의 중재를 독립적인 개념으로 만들고 이것을 캡슘화함으로써, 사용자는 각 객체의 행동과 상관없이 객체 간 연결 방법에만 집중할 수 있다.  
	
### 예제 코드
~~~java
// Colleague interface
interface Command {
	void execute();
}

// Abstract Mediator
interface Mediator {
    void book();
    void view();
    void search();
    void registerView(BtnView v);
    void registerSearch(BtnSearch s);
    void registerBook(BtnBook b);
    void registerDisplay(LblDisplay d);
}

// Concrete mediator
class ParticipantMediator implements Mediator {
	BtnVAIEW btnView;
	BtnSearch btnSearch;
	BtnBook btnBook;
	LblDisplay lblDisplay;
	
	public void registerView(BtnView btnView) {
		this.btnView = btnView;
	}
	
	public void registerSearch(BtnSearch btnSearch) {
		this.btnSearch = btnSearch;
	}
	
	public void registerBook(BtnBook btnBook) {
		this.btnBook = btnBook;
	}
	
	public void registerDisplay(LblDisplay lblDisplay) {
		this.lblDisplay = lblDisplay;
	}
	
	public void book() {
		btnBook.setEnabled(false);
		btnView.setEnabled(true);
		btnSearch.setEnabled(true);
		show.setText("booking...");
	}
	
	public void view() {
		btnView.setEnabled(false);
		btnSearch.setEnabled(true);
		btnBook.setEnabled(true);
		show.setText("viewing...");
	}
	
	public void search() {
		btnSearch.setEnabled(false);
		btnView.setEnabled(true);
		btnBook.setEnabled(true);
		show.setText("searching...");
	}
}

// A concrete colleague
class BtnView extends JButton implements Command {
	Mediator mediator;
	
	BtnView(ActionListener actionListener, Mediator mediator) {
		super("View");
		addActionListener(actionListener);
		this.mediator = mediator;
		mediator.registerView(this);
	}
	
	public void execute() {
		mediator.view();
	}
}

// A concrete colleague
class BtnSearch extends JButton implements Command {
	Mediator mediator;
	
	BtnSearch(ActionListener actionListener, Mediator mediator) {
		super("Search");
		addActionListener(actionListener);
		this.mediator = mediator;
		mediator.registerSearch(this);
	}
	
	public void execute() {
		mediator.search();
	}
}

// A concrete colleague
class BtnBook extends JButton implements Command {
	Mediator mediator;
	
	BtnBook(ActionListener actionListener, Mediator mediator) {
		super("Book");
		addActionListener(actionListener);
		this.mediator = mediator;
		mediator.registerBook(this);
	}
	
	public void execute() {
		mediator.book();
	}
}

// A concrete colleague
class LblDisplay  extends JLabel {
	Mediator mediator;
	
	LblDisplay(Mediator mediator) {
		super("Just start...");
		this.mediator = mediator;
		mediator.registerDisplay(this);
		setFont(new Font("Arial", Font.BOLD, 24));
	}
}

class MediatorDemo extends JFrame implements ActionListener {
	Mediator med = new ParticipantMediator();
  
	MediatorDemo() {
		JPanel p = new JPanel();
		p.add(new BtnView(this, med));
		p.add(new BtnBook(this, med));
		p.add(new BtnSearch(this, med));
		getContentPane().add(new LblDisplay(med), "North");
		getContentPane().add(p, "South");
		setSize(400, 200);
		setVisible(true);
	}

	public void actionPerformed(ActionEvent actionEvent) {
		Command comd = (Command) actionEvent.getSource();
		comd.execute();
	}

	public static void main(String[] args) {
		new MediatorDemo();
	}
}

~~~

---
- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://ko.wikipedia.org/wiki/%EC%A4%91%EC%9E%AC%EC%9E%90_%ED%8C%A8%ED%84%B4>
	+ <https://online.visual-paradigm.com/diagrams/templates/class-diagram/gof-design-patterns-mediator/>

