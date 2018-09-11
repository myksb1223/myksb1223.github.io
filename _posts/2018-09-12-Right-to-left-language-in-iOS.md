---
layout: post
title:  "아이폰에서 아랍 언어와 같이 오른쪽에서 왼쪽방향의 언어를 쓸 경우(Right to left language like Arabic in iOS)."
date:   2018-09-12 02:15:00
author: Seungbeom Kim
categories: develop_diary
tags:	아이폰 iOS RTS LTS Arabic Objective-c
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에 아랍언어를 사용하는 사용자가 많아지면서 아랍관련 지원을 위해 공부하기 시작했다. 일단 아랍어는 오른쪽에서 왼쪽으로 글을 읽고 쓴다...

보통이면 왼쪽에서 오른쪽인데... 그래서 언어 설정을 아랍어로 변경하면 UI도 전부 오른쪽에서 왼쪽으로 재배치된다. 정말 신기하다.

아랍어를 사용하는 인구는 약 5억명정도이다. 그래서 아랍에 지원을 위해 이번에 뛰어들게 되어 잊지 않기위해 블로그에 글을 남긴다.

일단, iOS에서 AutoLayout을 사용하였고, 앱이 지원언어에 아랍어만 추가하면 자동적으로 UI가 반대로 바뀐다. 그래서 세부적으로 작업할 일이 사라진다.

하지만 클래스업(ClassUp)같은 경우에는 AutoLayout이 생기기도 훨씬 전부터 개발해왔던터라... AutoLayout이 전부 적용되어 있지 않다. 나의 습관이 AutoLayout보다 그냥 코드로 레이아웃을 짜는게 습관이 되었기 때문이다.

이 경우에는 국가별, 나라별로 지정하여 따로 구현해주는 방법도 있겠지만 추천하지 않는다.

가장 좋은 방법은 아래와 같이 현재 사용하고 있는 시스템 언어가 무엇인가가 중요하다.

    if([NSLocale characterDirectionForLanguage:[[NSLocale preferredLanguages] objectAtIndex:0]] == NSLocaleLanguageDirectionRightToLeft) {
      // ...
    }

위와 같이 사용하면 된다.

웬만하면 정말 AutoLayout을 사용하는 것을 추천한다!
