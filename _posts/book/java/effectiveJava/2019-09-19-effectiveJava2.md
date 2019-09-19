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
+ 인스턴스의 '논리적 동치성(logical eqaulity)'을 검사할 일이 없다.
+ 상위 클래스에서 재정의한 equals가 하위 클래스에도 딱 들어맞는다.
+ 클래스가 private이거나 package-private이고 equals 메서드를 호출할 일이 없다.
