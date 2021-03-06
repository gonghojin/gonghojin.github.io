---
layout: post
comments: true
title:  "이펙티브 자바 3/E_모든 객체의 공통 메서드"
date:   2019-09-19 22:00:00
author: Gongdel
categories: Book
tags:	 Java Book EffectiveJava
cover:  "/assets/instacode.png"
---
# 3장. 모든 객체의 공통 메서드
## 10. equals는 일반 규약을 지켜 재정의하라!
equals 메서드는 재정의하기 쉬워 보이지만 곳곳에 함정이 도사리고 있어서, 올바른 재정의가 필요하다.  
다음에서 열거한 상황 중 하나에 해당한다면 재정의하지 않는 것이 최선이다.
+ 각 인스턴스가 본질적으로 고유하다.
	+ 값을 표현하는 게 아니라 동작하는 개체를 표현하는 클래스가 여기 해당한다.  
	Thread가 좋은 예로, object의 equals 메서드는 이러한 클래스에 딱 맞게 구현되었다.
+ 인스턴스의 '논리적 동치성(logical eqaulity)'을 검사할 일이 없다.
+ 상위 클래스에서 재정의한 equals가 하위 클래스에도 딱 들어맞는다.
+ 클래스가 private이거나 package-private이고 equals 메서드를 호출할 일이 없다.

그렇다면 equals를 재정의해야 할 때는 언제일까?  
객체 식별성(두 객체가 물리적으로 같은가)이 아니라 논리적 동치성을 확인해야 하는데, 상위 클래스의 equals가 논리적 동치성을 비교하도록 재정의되지 않았을 때다.  
주로 값 클래스들(Integer와 String처럼 값을 표현하는 클래스)이 여기 해당한다. (두값 객체를 eqauls로 비교 시 객체가 같은지가 아니라 값이 같은지를 알고 싶어하기 때문)    

##### equals 메서드를 재정희 할 떄는 반드시 일반 규약을 따라야 하며, 다음은 Object 명세에 적힌 규약이다.
equals 메서드는 동치 관계를 구현하며, 다음을 만족한다.
+ 반사성: null이 아닌 모든 참조 값 x에 대해 x.equals(x)는 true다
+ 대칭성: null이 아닌 모든 참조 값 x, y에 대해, x.equals(y)가 true면 y.equals(x)도 true다. 
+ 추이성: null이 아닌 모든 참조 값 x, y, z에 대해, x.equals(y)가 true이고 y.equals(z)도 true면 x.equals(z)도 true다
+ 일관성
+ null-아님: null이 아닌 모든 참조 값 x에 대해, x.equals(null)은 false다.

그렇다면 Object 명세에서 말하는 동치관계란 무엇일까? 쉽게 말해, 집합을 서로 같은 원소들로 이뤄진 부분집합으로 나누는 연산이다.  
이 부분집을 동치 클래스라 한다. equals 메서드가 쓸모 있으려면 모든 원소가 같은 동치 클래스에 속한 어떤 원소와도 서로 교환할 수 있어야 한다.  

##### 보충 설명: null-아님
1. 명시적 null 검사 - 필요없다
~~~java
@Override public boolean equals(Object o) {
	if (o == null) {
		return false;
	}
	...
}
~~~
동치성을 검사하려면 equals는 건네받은 객체를 적절히 형변환한 후 필수 필드들의 값을 알아내야 한다. 그러려면 형변환에 앞서 instanceof 연산자로 입력 매개변수가 올바른 타입인지 검사해야 한다.  
2. 묵시적 null 검사 - 이쪽이 더 낫다.
~~~java
@Override public boolean equals(Object o) {
	if (!(o instanceof MyType)) {
		return false;
	}
	MyType mt = (MyType) o; 
	...
}
~~~

### 핵심정리
`꼭 필요한 경우가 아니면 equals를 재정의하지 말자.` 많은 경우가 Object의 equals로 원하는 비교를 정확히 충족해준다.  
재정의해야 할 떄는 그 클래스의 핵심 필드 모두를 빠짐없이, 다섯 가지 규약을 확실히 지켜가며 비교해야 한다.

## 11. equals를 재정의하려거든 hashCode도 재정의하라.
### 핵심정리
equals를 재정의 할때는 hashCode도 반드시 재정의 해야한다. 그렇지 않으면 프로그램이 제대로 동작하지 않을 것이다.  
재정의한 hasCode는 Object의 API 문서에 기술된 일반 규약을 따라야하며, 서로 다른 인스턴스라면 되도록 해시코드도 서로 다르게 구현해야 한다.

### 주의

- 성능을 높인답시고 해시코드를 계산할 때 핵심 필드를 생략해서는 안 된다. 해시 품질이 나빠져 해시테이블의 성능을 떨어 트릴수 있다.
- hashMap, hashSet과 같은 컬렉션의 원소를 사용할때 문제를 일으킬수 있다.
- 논리적으로 같은 객체는 같은 해시코드를 반환해야 한다.

## 12. toString을 항상 재정의하라
### 핵심정리
디버깅에 도움이되며 유용한 정보(보통의 경우 로깅)를 제공할 수 있다.  
+ toString은 그 객체가 가진 주요 정보 모두를 반환하는 게 좋다.  

## 13. clone 재정의는 주의해서 진행하라
### 핵심정리
Cloneable이 몰고 온 문제를 돌이켜 봤을 때, 새로운 인터페이스를 만들 때는 절대 사용하면 안되며, 새로운 클래스도 사용하면 안된다.  
final 클래스일 경우는 위험도가 적지만, 성능 최적화 관점에서 검토한 후 드물게 사용해야 한다. 기본원칙은 `'복제기능은 생성자와 팩터리를 이용하는게 최고'`라는 것이다.  
단, 배열만은 clone 메서드 방식이 가장 깔끔하다.

## 14. Comparable을 구현할지 고려하라
### 핵심정리
sort기능을 고려할시 꼭 Comparable을 구현하고 사용하도록 하자.  
박싱된 기본타입 클래스의 경우, 정적 compare나 Comparator 인터페이스가 제공하는 비교자 생성 메서드를 사용하자.

