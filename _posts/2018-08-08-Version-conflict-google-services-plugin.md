---
layout: post
title:  "Version conflict google service plugin"
date:   2018-08-08 19:00:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 안드로이드 Android Firebase 에러 error Gradle Google로그인 GoogleLogin
sitemap :
  changefreq : daily
  priority : 1.0
---

예전에 사용하던 구글 로그인을 Firebase로 구글 로그인으로 변경하려고 하던 중에 아래와 같은 에러가 났다.

    please fix the version conflict either by updating the version of the google-services plugin

나는 bulde.gradle에서

    apply plugin: 'com.google.gms.google-services'

을 중앙에 위치시켰는데... 맨 아래로 옮기니 해결되었다.
