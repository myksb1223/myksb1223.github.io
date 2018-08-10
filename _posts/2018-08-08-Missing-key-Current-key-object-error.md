---
layout: post
title:  "Firebase 통합시 Missing api_key/current_key object 에러발생(Missing api_key/current_key object in Firebase)"
date:   2018-08-08 19:17:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 안드로이드 Android Firebase 에러 error Gradle
sitemap :
  changefreq : daily
  priority : 1.0
---

Gradle 4로 업데이트 하고 Firebase를 통합하려고 하니... 이런 저런 에러가 많이 난다. 이번 에러는 무엇인가...

    Error:Execution failed for task ':classUp:processDebugGoogleServices'.
      > Missing api_key/current_key object

[Firebase 콘솔](https://console.firebase.google.com/)로 가서 내 프로젝트로 들어간 후...

설정 아이콘 클릭 -> 내 앱 부분으로 가면 GCM을 쓰던 당시에 다운 받았던 google-services.json파일을 새롭게 다운받을 수 있다. 다운 받은 후 예전 것과 교체해주면 된다.
