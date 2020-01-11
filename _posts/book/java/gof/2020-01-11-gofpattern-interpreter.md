---
layout: post
comments: true
title:  "Gof 디자인패턴_해석자(Interpreter) 패턴"
date:   2020-01-11 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book Gof 사내스터디
cover:  "/assets/instacode.png"
---
## 해석자(Interpreter) 패턴
### 의도
어떤 언어에 대해, 그 언어의 문법에 대한 표현을 정의하면서 그것(표현)을 사용하여 해당 언어로 기술된 문장을 해석하는 해석자를 함께 정의 

### 활용성
##### 사용하기 좋은 상황
+ 정의할 언어의 문법이 간단할 때
	- 문법이 복잡하다면 문법을 정의하는 클래스 계통이 복잡해지고 관리할 수 없게 된다.  
+ 효율성을 고려할 사항이 아닐 때

### 구조
![alt](/assets/gof/images/gof-design-patterns-interpreter.png)

+ AbstractExpression
	+ 추상 구문 트리에 속한 모든 노드에 해당하는 클래스들이 공통으로 가져야 할 Interpret() 연산을 추상 연산으로 정의한다.
+ TerminalExpression
	+ 문법에 정의한 터미널 기호와 관련된 해석 방법을 구현한다.  
	문장을 구성하는 모든 터미널 기호에 대해서 해당 클래스를 만들어야 한다.
+ NonterminalExpression
	+ 문법의 오른편에 나타나는 모든 기호에 대해서 클래스를 정의해야 한다.
+ Context
	+ 번역기에 대한 포괄적인 정보를 포함
+ Client
	+ 언어로 정의한 특정 문장을 나타내는 추상 구문 트리이다.  
	이 추상 구문 트리는 NonterminalExpression과 TerminalExpression 클래스의 인스턴스로 구성된다.  
	이 인스턴스의 Interpret() 연산을 호출한다.

### 협력 방법
+ Client는 NonterminalExpression과 TerminalExpression 인스턴스로 해당 문장에 대한 추상 구문 트리를 만든다.  
	그리고 사용자는 Interpret() 연산을 호출하는데, 이때 해석에 필요한 문맥 정보를 초기화한다.
+ 각 NonterminalExpression 노드는 또 다른 서브 표현식에 대한 Interpret()를 사용하여 자신의 Interpret() 연산을 정의한다.  
	Interpret() 연산은 재귀적으로 Interpret() 연산을 사용하여 기본적 처리를 담당한다.

### 결과
+ 문법의 변경과 확장이 용이해 진다.  
	+ 패턴에서 문법에 정의된 규칙을 클래스로 표현하였기 때문에 문법을 변경하거나 확장하고자 할 떄는 상속을 사용하면 된다.
		+ 확장을 통해 기존의 표현식을 지속적으로 수정하거나, 새로운 서브 클래스 정의를 통해 새로운 표현식을 정의할 수 있다.
+ 문법의 구현이 용이해 진다.
	+ 추상 구문 트리의 노드에 해당하는 클래스들은 비슷한 구현 방법을 갖는다.  
		이들 클래스를 작성하는 것은 쉬운 일이며, 컴파일러나 파서 생성기를 사용해서 자동 생성할 수도 있다.

### 예제 코드
~~~java
// AbstractExpression class
public abstract class Expression {
	public abstract int interpret();
	
	@Override
	public abstract String toString();
}

// TerminalExpression 1
public class MultiplyExpression extends Expression {
	private Expression leftExpression;
	private Expression rightExpression;
	
	public MultiplyExpression(Expression leftExpression, Expression rightExpression) {
		this.leftExpression = leftExpression;
		this.rightExpression = rightExpression;
	}

	@Override
	public int interpret() {
		return leftExpression.interpret() * rightExpression.interpret();
	}

	@Override
	public String toString() {
		return "*";
	}
}

// TerminalExpression 2
public class MinusExpression extends Expression {
	
	private Expression leftExpression;
	private Expression rightExpression;
	
	public MinusExpression(Expression leftExpression, Expression rightExpression) {
		this.leftExpression = leftExpression;
		this.rightExpression = rightExpression;
	}
	
	@Override
	public int interpret() {
		return leftExpression.interpret() - rightExpression.interpret();
	}
	
	@Override
	public String toString() {
		return "-";
	}
}

// TerminalExpression 3
public class PlusExpression extends Expression {

	private Expression leftExpression;
	private Expression rightExpression;
	
	public PlusExpression(Expression leftExpression, Expression rightExpression) {
		this.leftExpression = leftExpression;
		this.rightExpression = rightExpression;
	}
	
	@Override
	public int interpret() {
		return leftExpression.interpret() + rightExpression.interpret();
	}
	
	@Override
	public String toString() {
		return "+";
	}
}

public class NumberExpression extends Expression {
	
	private int number;
	
	public NumberExpression(int number) {
		this.number = number;
	}
	
	public NumberExpression(String s) {
		this.number = Integer.parseInt(s);
	}
	
	@Override
	public int interpret() {
		return number;
	}
	
	@Override
	public String toString() {
		return "number";
	}
}

// Client
public class ExpressionExample {
	public static void main(String[] args){
	  String tokenString = "4 3 2 - 1 + *";
	  Stack<Expression> stack = new Stack<Expression>();
		
	  String[] tokenList = tokenString.split(" ");
	  for (String s: tokenList) {
	  	if (isOperator(s)) {
	  		String rightExpression = stack.pop();
	  		String leftExpression = stack.pop();
	  		
	  		Expression operator = getOperatorInstance(s, leftExpression, rightExpression);
	  		int result = operator.interpret();
	  		Expression resultExpression = new NumberExpression(result);
	  	}
	  }
	}

	 public static boolean isOperator(String s) {
		return s.equals("+") || s.equals("-") || s.equals("*"); 
	}
	
	public static Expression getOperatorInstance(String s, Expression left, Expression right) {
		switch (s) {
			case "+":
				return new PlusExpression(left, right);
			case "-":
				return new MinusExpression(left, right);
			case "*":
				return new MultiplyExpression(left, right);
			default:
				return new MultiplyExpression(left, right);
		}
	}
}
~~~

###  잘 알려진 사용예
가장 일반적인 형태는 `복합체(Composite) 패턴`이 사용되는 곳에 해석자 패턴을 사용할 수 있다.  
복합체 패턴으로 정의한 클래스들이 하나의 언어 구조를 정의할 때만 해석자 패턴이라고 한다.  

## 관련 패턴
추상 구문 트리는 복합체(Composite) 패턴의 한 인스턴스로 볼 수 있으며, 하나의 구문 트리 내에 터미널 기호를 여러 개 공유하기 위해서는 플라이급(Flyweight) 패턴을 적용할 수 있다.  
또한 Interpreter은 반복자(Iterator)패턴을 사용해서 자신의 구조를 순회한다.

---
- 참조
	+ [Gof의 디자인 패턴](https://www.google.com/search?newwindow=1&sxsrf=ACYBGNTM3TLPpNtM8XVERiP7AyPyLDi3sQ%3A1572758465286&ei=wWO-XfOOEcTGmAWs26i4Cw&q=gof%EC%9D%98+%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4&oq=gof&gs_l=psy-ab.1.1.35i39l2j0i67j0j0i131l4j0j0i131.1801221.1802149..1803884...0.1..0.188.465.0j3......0....1..gws-wiz.......0i71.wMtI5vf-WEU)	
	+ <https://github.com/iluwatar/java-design-patterns/tree/master/interpreter/src/main/java/com/iluwatar/interpreter>
	+ <https://online.visual-paradigm.com/diagrams/templates/class-diagram/gof-design-patterns-interpreter/>

