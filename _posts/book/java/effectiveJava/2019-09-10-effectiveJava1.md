---
layout: post
comments: true
title:  "이펙티브 자바 3/E_객체생성과 파괴"
date:   2019-09-01 15:00:00
author: Gongdel
categories: Book
tags:	 Java Book EffectiveJava
cover:  "/assets/instacode.png"
---
# 2장. 객체 생성과 파괴
## 생성자 대신 정적 팩터리 메서드를 고려하라!
클래스의 인스턴스를 얻는 일반적인 방법은 public 생성자다. 하지만 꼭 알아야할 기법이 하나 더 있다.  
> 클래스는 생성자와 별도로 `정적 팩터리 메서드(static factory method)`를 제공할 수 있다.

#### 정적 팩토리 메서드가 생성자보다 좋은 장점 다섯 가지
1. 이름을 가질 수 있다.
	+ 생성자에 넘기는 매개변수와 생성자 자체만으로는 객체의 특성을 제대로 알지 못하지만, 정적 팩토리는 이름만 잘 지으면 반환될 객체의 특성을 쉽게 묘사 가능하다.
~~~
	어느 쪽이 '값이 소수인 BigInteger를 반환한다는' 의미를 더 잘 설명할까?
	생성자: BigInteger(int, int, Random) 
	정적 팩터리 메서드: BigInteger.probablePrime
~~~

2. 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.
	+ 반복되는 요청에 같은 객체를 반환하는 식으로 정적 팩터리 방식의 클래스는 언제 어느 인스턴스를 살아있게 할지를 철저히 통제가능

3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
5. 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.

#### 정적 팩토리 메서드가 생성자보다 좋은 단점 두 가지
1. 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.
	+ 상속보다 컴포지션을 사용하도록 유도하고 불변 타입으로 만들려면 이 제약을 지켜야 한다는 점에서는 오히려 장점으로 받아들일 수도 있다.
2. 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.
	+ 생성자처럼 API 설명에 명확히 드러나지 않으니 사용자는 정적 팩터리 메서드 방식 클래스를 인스턴스화할 방법을 알아내야 한다.

~~~
정적 팩터리 메서드의 흔한 명명 방식
+ from: 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드
	예) Date d = Date.from(instant);
+ of: 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드
	예) Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);
+ valueOf: from과 of 더 자세한 버전
	예) BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);
+ instance 혹은 getIntance: (매개변수를 받는다면) 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지는 않는다.
	예) StackWalker luke = StackWalker.getInstance(options);
+ create 혹은 newInstance: instance 혹은 getInstance와 같지만, 매번 새로운 인스턴스를 생성해 반환함을 보장한다.
	예) Object newArray = Array.newinstance(classObject, arrayLen);

....  
~~~

## 핵심정리
정적 팩터리 메서드와 public 생성자는 각자의 쓰임새가 있으니 상대적인 장단점을 이해하고 사용하는 것이 좋다.  
그렇다고 하더라도 `정적 팩터리를 사용하는 게 유리한 경우가 더 많으므로 무작정 public 생성자를 제공하던 습관이 있다면 고치자.`
