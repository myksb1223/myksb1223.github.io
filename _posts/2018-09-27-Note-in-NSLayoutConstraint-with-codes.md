---
layout: post
title:  "NSLayoutConstraint 코드로 적용시 주의 사항(Note in NSLayoutConstraint with codes)."
date:   2018-09-27 02:19:00
author: Seungbeom Kim
categories: develop_diary
tags:	아이폰 iOS NSLayoutConstraint Objective-c
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에 아랍언어를 지원하면서 NSLayoutConstraint 덕을 많이 보았다. 하지만 내가 코드로 직접 NSLayoutConstraint를 적용했던 경우에는 완전 LTS만을 생각해서 부여하지 않은 속성이 있어 완벽히 RTS에 맞게 UI가 변형되지 않았다.

그래서 오랜만에 예전 기억을 되살려서 NSLayoutConstraint를 코드로 직접 구현하는데... 계속 에러가 난다.

결국 알고 봤더니...
내가 코드로 직접 NSLayoutConstraint를 적용하기 위해서는 내가 코드로 준 Frame의 크기과 Contraint의 크기가 매칭이 되어야 한다는 것을 잊고 있었던 것이다.

바보같다... ㅋㅋ 하지만 또 까먹을 것 같아서 블로그에 적어둔다!

항상 NSLayoutConstraint를 코드로 적용하기 전에는 내가 해당 View에 적용한 Frame의 크기를 확인하도록 하자!
