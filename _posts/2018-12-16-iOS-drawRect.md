---
layout: post
title:  "iOS drawRect에 대한 이야기.(drawRect in iOS)."
date:   2018-12-16 11:04:00
author: Seungbeom Kim
categories: develop_diary
tags:	아이폰 iOS drawRect
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)을 최근에 달력처럼 무한 스크롤 방식을 도입하면서 한가지 생긴 문제점은 수업이 많으면 많을 수록 느려졌다. 그래서 생각한 방법 중에 하나는 drawRect를 이용하여 직접 그리는 것이었다.

직접 UIView의 SubClass들을 이용해서 `addSubView:`를 이용하는 것보다 빠르기 때문이다.

여기서 만난 문제는 먼저 `CALayer`와 `drawRect:`였다. 각 수업의 gradient 효과를 위해서 CALayer의 SubClass인   `CAGradientLayer`를 사용하였는데 `drawRect:`로 그린 내용들이 보이지 않는 것이었다.

즉, 여기서 알 수 있는 결과는 `CALayer`는 `drawRect:` 위에 표현된다는 것이다.

그래서 `drawRect:`에 직접 gradient를 직접 그렸다.

이 때 또 생긴 문제는 `gradient`의 alpha값을 0으로 하면 계속 검은색이 나오는 것이었다. `drawRect:`가 수행되어 색을 그리는데 그 밑에 이미 검은색으로 배경색이 결정되어 있다는 뜻이다.

즉, `backgroundColor`가 nil이기 때문에 계속 검은색으로 나오는 것이다.

해당 값을 `[UIColor clearColor]`로 바꾸어주면 alpha가 0일 때 아예 투명하게 만들 수 있다.
