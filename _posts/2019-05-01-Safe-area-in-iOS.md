---
layout: post
title:  "iOS Safe area 이해하기"
date:   2019-05-01 03:31:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS Objective-c safearea
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp) 개발하는 도중, iPhoneX 및 그 상위 모델을 가지고 있지 않아서 시뮬레이터로 가끔 테스트하곤 했는데, 기기 대여해주는 곳을 찾아서 한번 찾아가 보았다. (사실 Apple에서 앱 제출을 위해서는 6.5inch 스크린 샷이 반드시 필요했기 때문에 간 것이 더 크다.)

이 때, 당연히 밑에 바?를 고려하지 않고 만든 것이 당연하다고 생각했는데... 어라? 모든 앱들이 밑에 바를 포함시키지 않는 것을 알았다. 그리고 찾아보니 safearea라는 것을 적용해야 한다는 것을 알았다.

사실 변명을 하자면... 오래 전부터 개발했기 때문에 `storyboard`자체를 사용하지 않고, `xib`자체도 거의 사용하지 않는 편이다. `autolayout`으로 넘어온 기간도 매우 짧았고 `NavigationBar`등도 따로 만들어서 사용했기 때문에 `safearea`가 아닌 그냥 Top, Height 등의 제약조건으로 `NSLayoutContraint`를 이용했다.

어쨌든 `safearea`라는 것이 무엇인지 찾아보니... 정말 좋은 것이었다. 이제서야 알게 되다니...

각 기종마다 건드리지 말아야할 영역을 알려주는 것이라고 생각하면 쉬울 것 같다.

기본적으로 예전 디바이스는 `statusbar`는 `View`영역에서 제외하는 것이다. iPhoneX 이후 모델은 `statusbar` 및 `bottombar`의 크기를 알아서 `View`영역에서 제외시켜 준다.

그러면 원하는 View를 위치시킬 때 해당 View의 bottom, top, left, right를 `superview`가 아닌 `safearea`에 적용시키면 알아서 작동한다.

예를 들어 클래스업(ClassUp)의 메인 시간표 화면과 같이 배경은 전체화면으로 내부에 들어가는 수업박스는 iPhoneX 이상 기종의 `bottombar`를 제외한 영역으로 하고 싶다고 한다면, 배경의 top, bottom, left, right는 `superview`로 제약을, 내부에 들어가는 뷰들은 bottom, top, left, right 중에 해당되는 속성을 `safearea`로 설정하면 된다.

<img src="{{ site.baseurl }}/assets/develop_diary/safearea_1.png" title="SafeArea 1" class="post-image">

<p style="clear: left;">

<figure>
<img src="{{ site.baseurl }}/assets/develop_diary/safearea_2.png" title="SafeArea Picture 2" class="post-image-double"><img src="{{ site.baseurl }}/assets/develop_diary/safearea_3.png" title="SafeArea Picture 3" class="post-image-double">
<figcaption style="text-align: center;">왼쪽이 BackView, 오른쪽이 PagerView.</figcaption>
</figure>
