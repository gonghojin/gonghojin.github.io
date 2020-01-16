---
layout: post
comments: true
title:  "이펙티브 자바 3/E_제네릭"
date:   2019-10-12 22:00:00
author: Gongdel
categories: Book
tags:	 Java Book EffectiveJava
cover:  "/assets/instacode.png"
---
# 5장. 제네릭
## 26. 로 타입은 사용하지 말라.
### 제네릭 (한글/영문) 용어 정리 

한글 용어 | 영문 용어 | 예
------------ | -------------| -------------|
매개변수화 타입         | parameterized type           |List< String >
실제 타입 매개변수      | actual type parameter        |String
제네릭 타입             | generic type                 |List< E >
정규 타입 매개변수      | formal type parameter        |E
비한정적 와이들카드 타입|unbounded wildcard type       |List<?>
로(raw) 타입            | raw type                     | List
한정적 타입 매개변수    | bounded type type parameter  | < E extends Number >
재귀적 타입 한정        | recursive type bound         | <T extends Comparable<T>>
한정적 와일드 카드 타입 | bounded wildcard type        | List<? extends Number>
제네릭 메서드           | generic method               |static <E> List<E>, asList<E[] a>
타입 토큰               | type token                   |String.class

---
### 핵심 정리
로 타입을 사용하면 런타임에 예외가 일어날 수 있으니 사용하면 안된다.
로 타입은 제네릭이 도입되기 이전 코드와의 호환성을 위해 제공될 뿐이다.
Set<Object>는 어떤 타입의 객체도 저장할 수 있는 매개변수화 타입이고,
Set<?>는 모종의 타입 객체만 저장할 수 있는 와일드카드 타입니다.
raw 타입인 Set은 제네릭 타입 시스템에 속하지 않는다.
Set<Object>, Set<?>는 안전하지만, raw 타입인 Set은 안전하다. 

## 27. 비검사 경고를 제거하라.
제네릭을 사용하기 시작하면 수많은 컴파일러 경고를 보게 될 것이다.(ex: 비검사 형변환 경고, 비검사 메서드 호출 경고 등)     
`할 수 있는 한 모든 비검사 경고를 제거하라!`
### 핵심 정리
비검사 경고는 중요하니 무시하지 말자. 모든 비검사 경고는 런타임에 ClassCastException을 일으킬 수 있는 잠재적 가능성을 뜻하니 최선을 다해 제거하라.  
경고를 없앨 방법을 찾지 못하겠다면, 그 코드가 타입 안전함을 증명하고 가능한 한 범위를 @SuppressWarnings("unchecked") 애너테이션으로 경고를 숨겨라. 그런 다음 경고를 숨기기로 한 근거를 주석으로 남겨라.
##### @SuppressWarnings은 가능한 한 좁은 범위에 적용하자.
~~~java

   public <T> T[] toArray(T[] a){
        @SuppressWarnings("unchecked") T[] result = (T[]) Arrays.copyOf(a, a.length, a.getClass());
        ...
    }
    
~~~

## 28. 배열보다는 리스트를 사용하라.
### 핵심 정리
배열과 제네릭에는 매우 다른 타입 규칙이 적용된다. 배열은 공변이고 실체화되는 반면, 제네릭은 불공변이고 타입 정보가 소거된다.  
그 결과 배열은 런타임에는 타입 안전하지만 컴파일 타임에는 그렇지 않다. 제네릭은 반대다. 그래서 둘을 섞어 쓰기란 쉽지 않다.  
둘을 섞어 쓰다가 컴파일 오류나 경고를 만나면, 가장 먼저 배열을 리스트로 대체하는 방법을 적용해보자.

+ 공변: Sub가 Super의 하위 타입이라면 배열 Sub[]는 Super[]의 하위 타입이 된다. 
	즉, 함께 변한다. 
+ 배열은 실체화된다?
	+ 배열은 런타임에도 자신이 담기로 한 원소의 타입을 인지하고 확인한다. 반면 제네릭은 타입 정보가 런타임에는 소거된다.

##### 배열 대신 리스트를 통해 타입 안정성을 확보하자.

~~~java
//배열을 사용할 경우
public void chooser(Collection<T> choices) {
    T[] array = (T[]) choices.toArray();
    ...
}

//배열을 리스트로 변경
public void chooser(Collection<T> choices) {
    List<T> list = new ArrayList<>(choices);
    ...
}
~~~

## 29. 이왕이면 제네릭 타입으로 만들라.
### 핵심 정리
클라이언트에서 직접 형변환해야 하는 타입보다 제네릭 타입이 더 안전하고 쓰기 편하다.  
그러니 새로운 타입을 설계할 때는 형변환 없이도 사용할 수 있도록 하라. 그렇게 하려면 제네릭 타입으로 만들어야 할 경우가 많다.  
기존 타입 중 제네릭이었어야 하는 게 있다면 제네릭 타입으로 변경하자. 기존 클라이언트에는 아무 영향을 주지 않으면서, 새로운 사용자를 훨씬 편하게 해주는 길이다.

#####  배열을 사용한 코드를 제네릭으로 만드는 방법 1 - 가독성이 좋다( 더 선호하는 방법, 단점은 존재(이팩티브 자바 173쪽))
~~~java
public class Stack<E> {
	...
	private E[] elements;
	// 배열 elements는 push(E)로 넘어온 E 인스턴스만 담는다.
	// 따라서 타입 안전성을 보장하지만, 이 배열의 런타임 타임은 E[]가 아닌 Object[] 다!
	@SuppressWarnings("uncheked")
	public Stack() {
		elements = (E[]) new Object[DEFAULT_INITIAL_CAPACITY];
	}
}
~~~

#####  배열을 사용한 코드를 제네릭으로 만드는 방법 2
~~~java
public class Stack<E> {
	...
	private Object[] elements;
	public E pop() {
		// push에서  E 타입만 허용하므로 이 형변환은 안전하다
		@SuppressWarnings("uncheked")
		E result = (E) elements[--size];
		
		elements[size] = null; // 다 쓴 참조 해제
		return result;
	}
}
~~~

## 30. 이왕이면 제네릭 메서드로 만들라.
### 핵심 정리
제네릭 타입과 마찬가지로, 클라이언트에서 입력 매개변수와 반환값을 명시적으로 형변환해야 하는 메서드보다 제네릭 메서드가 더 안전하며 사용하기도 쉽다.  
타입과 마찬가지로, 메서드도 형변환 없이 사용할 수 있는 편이 좋으며, 많은 경우 그렇게 하려면 제네릭 메서드가 되어야 한다. 역시 타입과 마찬가지로, 형변환을 해줘야 하는 기존 메서드는 제네릭하게 만들자.  
기존 클라이언트는 그대로 둔 채 새로운 사용자의 삶을 훨씬 편하게 만들어 줄 것이다.  

## 31. 한정적 와일드카드를 사용해 API 유연성을 높이라.
### 핵심 정리
조금 복잡하더라도 와일드카드 타입을 적용하면 API가 훨씬 유연해진다. 그러니 널리 쓰일 라이브러리를 작성한다면 반드시 와일드카드 타입을 적절히 사용해줘야 한다.  
PECS 공식을 기억하자. 즉, 생선자(producer)는 extends를 소비자(consumer)는 super를 사용한다. Comparable과 Comparator는 모두 소비자라는 사실도 잊지 말자.
+ 매개변수화 타입 T
	+ 생산자(예: Stack의 pushAll)
		+ <? extends T>
	+ 소비자(예: Stack의 popAll)
		+ <? super T>
## 32. 제네릭과 가변인수를 함께 쓸 때는 신중하라.
### 핵심 정리
가변인수와 제네릭은 궁합이 좋지 않다. 가변인수 기능은 배열을 노출하여 추상화가 완벽하지 못하고, 배열과 제네릭의 타입 규칙이 서로 다르기 때문이다.  
제네릭 varags(가변인수) 매개변수는 타입 안전하지는 않지만, 허용된다. 메서드에 제네릭 (혹은 매개변수화된) varags 매개변수를 사용하고자 한다면, 먼저 그 메서드가 타입 안전한지 확인한 다음 @SageVarags 애너테이션을 달아 사용하는 데 불편함이 없게끔 하자.


##### 제네릭 varargs 배열 매개변수에 값을 저장하는 것은 안전하지 않다.

~~~java
static void dangerous(List<String>... stringLists) {
    List<Integer> intList = List.of(42); //java 9
    Object[] objects = stringList;
    objects[0] = intList; // 힙 오염 발생
    String s = stringList[0].get(0); // ClassCastException
}
~~~

`제네릭 varargs가 안전하다면 @SafeVarargs를 붙인다. varargs 배열은 메서드가 호출될 때 생성되며
넘겨진 배열에 어떤 값을 저장하거나, 배열을 외부에 노출하지 않을 경우 안전하다.`

##### varargs가 리턴되어 노출된 경우 문제
~~~java
@Test(expected = ClassCastException.class)
    public void test2() {
        String[] strArray = twoPick("하나", "둘", "셋");// ClassCastException 발생!
        System.out.println(strArray[0]);
    }

    private <T> T[] twoPick(T a, T b, T c){
        System.out.println("twoPick: " + a.getClass().getName());
        return toArray(a, b);
    }

    // T의 타입은 컴파일타임에 결정된다.
    // 컴파일 시점에 T의 타입을 알수 없기 때문에 Object가 선택된다.
    private <T> T[] toArray(T... args) {
        System.out.println("toArray: "+ args.getClass().getName());
        return args;
    }

~~~

```
출력 내용
twoPick: java.lang.String
toArray: [Ljava.lang.Object;
``` 

---

## 33. 타입 안전 이종 컨테이너를 고려하라.
### 핵심 정리
컬렉션 API로 대표되는 일반적인 제네릭 형태에서는 한 컨테이너가 다룰 수 있는 타입 매개변수의 수가 고정되어 있다. 하지만 컨테이너 자체가 아닌 키를 타입 매개변수로 바꾸면 이런 제약이 없는 타입 안전 이종 컨테이너를 만들 수 있다.  
타입 안전 이종 컨테이너는 Class를 키로 쓰며, 이런 식으로 쓰이는 Class 객체를 타입 토큰이라 한다. 또한, 직접 구현한 키 타입도 쓸 수 있다. 예컨대 데이터베이스의 행(컨테이너)을 표현한 DatabaseRow 타입에는 제네릭 타입인 Column<T>를 키로 사용할 수 있다.  

##### Class.cast()를 사용해 타입 안정성을 보장하는 컨테이너 사용법
~~~java
public class Favorites {

    private Map<Class<?>, Object> favorites = new HashMap<>();

    public <T> void putFavorite(Class<T> type, T instance) {
        //cast 메서드를 사용해 타입 안정성을 보장
        favorites.put(Objects.requireNonNull(type), type.cast(instance));
    }

    public <T> T getFavorite(Class<T> type) {
        //cast 메서드를 사용해 타입 안정성을 보장
        return type.cast(favorites.get(type));
    }
}
~~~
