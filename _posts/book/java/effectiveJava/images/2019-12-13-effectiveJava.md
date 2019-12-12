---
layout: post
comments: true
title:  "이펙티브 자바 3/E_예외"
date:   2019-12-13 00:00:00
author: Gongdel
categories: Book
tags:	 Java Book EffectiveJava
cover:  "/assets/instacode.png"
---
# 예외
예외를 제대로 활용한다면 프로그램의 가독성, 신뢰성, 유지보수성이 높아지지만, 잘못 사용하면 반대의 효과만 나타난다. 예외를 효과적으로 활용해보자.  
## 69. 예외는 진짜 예외 상황에만 사용하라
### 핵심 정리
예외는 예외 상황에서 쓸 의도로 설계되었다. 정상적인 제어 흐름에서 사용해서는 안 되며, 이를 프로그래머에게 가용하는 API를 만들어서도 안 된다.
