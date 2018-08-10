---
layout: post
title:  "App not installed 에러(error)"
date:   2018-08-10 19:34:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 안드로이드 Android APK 에러 error
sitemap :
  changefreq : daily
  priority : 1.0
---

안드로이드 스튜디오(Android Studio)에서 Gradle을 업데이트 하고 이것저것 업데이트를 조금 했더니 갑자기 **Generate Signed APK**에서 무엇인가 조금 바뀌었다. 그냥 아무 생각없이 마지막 체크박스 두개 중에 V2 (Full APK Signature)만 클릭했는데... 아래 그림처럼 안드로이드 7(Nougat)버전 밑으로는 설치가 되지 않았다.

<img src="{{ site.baseurl }}/assets/develop_diary/app_not_installed_error_1.png" title="App not installed 1" class="post-image">

그래서 뭐가 문제인지 봤더니... 아래 그림처럼 양쪽 다 체크를 해줘야 한다.

<img src="{{ site.baseurl }}/assets/develop_diary/app_not_installed_error_2.png" title="App not installed 2" class="post-image">

[V2관련한 자료](https://source.android.com/security/apksigning/v2)는 링크에 있다.
