---
layout: post
title:  "안드로이드 알람매니저로 푸쉬 알림 사용하기.(Android AlarmManager with Push notification.)"
date:   2018-08-12 03:43:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 안드로이드 Android 알람매니저 AlarmManager 알림 Notification
sitemap :
  changefreq : daily
  priority : 1.0
---

**클래스업(ClassUp)**에는 수업, 노트에 알림을 걸 수 있어, 원하는 시간대에 울리게 할 수 있다. 아이폰은 잘 작동하지만 안드로이드가 문제였다. 내가 지금까지 테스트한 폰에서는 잘 울리지만... 다른 폰에서는 시도때도 없이 울린다고 리뷰 및 연락이 왔었다.

과연 무엇이 문제일까? 생각하여 오랫동안 찾아보고 적용했지만... 전혀 먹히지 않았다.

보통 로컬 알림(Local notification)은 PendingIntent를 사용하여 브로드캐스트 및 서비스를 이용할 것이다. 나 역시 그렇게 해왔다. 그리고 한 번에 10개, 20개의 PendingIntent를 생성해서 알람매니저(AlarmManager)에 등록했다. 아마 문제는 여기에 있을 거라고 생각한다.

결국 **구글캘린더** 소스를 분석했다. 보자마자 뜨헉... 했다. 도대체 어떻게 분석해야할까...? 막막했다. 어찌나 코드가 정교하게 쓰여있던지 정말 보기 힘들었다.

대충 구조를 파악하면 아래와 같다.

<img src="{{ site.baseurl }}/assets/develop_diary/google_calendar_alarm.png" title="Android alarm 1" class="post-image">

이렇게 분석했는데... [링크](https://developer.android.com/guide/topics/providers/calendar-provider)로 가면 구조를 바로 알 수 있다.

**클래스업(ClassUp)**에 어떻게 적용시켜야 할까? 고민하였다. 아래와 같이 적용하였다.

<img src="{{ site.baseurl }}/assets/develop_diary/classup_alarm.png" title="Android alarm 2" class="post-image">

일단 테스트 중인데, 이렇게 하면 정말 완벽한 알림 구조가 될 것 같다. 역시 다른 사람의 코드를 분석하는 것은 정말 큰 도움이 된다.
