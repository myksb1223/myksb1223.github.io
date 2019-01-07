---
layout: post
title:  "FirebaseInstanceId Token retrieval failed SERVICE_NOT_AVAILABLE error"
date:   2018-09-06 13:07:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 안드로이드 Android Firebase 에러 error Gradle
sitemap :
  changefreq : daily
  priority : 1.0
---

갑자기 에뮬레이터나 폰에서 ANR이 발생하기 시작했다.

    E/FirebaseInstanceId: Token retrieval failed: SERVICE_NOT_AVAILABLE

문제가 무엇인지 아무리 찾아도 해결책이 없었다.

그런데 로그에 아래와 같은 에러가 같이 찍혔다.
E/com.facebook.internal.AttributionIdentifiers: getAttributionIdentifiers should not be called from the main thread

그래서 해결책을 찾아보니

    compile 'com.facebook.android:facebook-android-sdk:4.+'

위의 형태를 아래와 같이 바꾸라고 되어있었다.

    compile 'com.facebook.android:facebook-android-sdk:4.34.0'

그러니까 갑자기 되기 시작했다.
