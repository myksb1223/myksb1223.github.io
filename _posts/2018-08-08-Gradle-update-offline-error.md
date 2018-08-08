---
layout: post
title:  "Gradle 업데이트 시 offline 에러발생(Gradle update offline error)"
date:   2018-08-08 17:40:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 안드로이드 Android Gradle offline 에러 error Gradle업데이트 GradleUpdate
sitemap :
  changefreq : daily
  priority : 1.0
---

오늘 안드로이드 스튜디오(Android Studio)의 **Gradle**을 4로 업데이트를 했는데... 계속 아래와 같은 에러가 발생했다.

    FAILURE: Build failed with an exception.

    * What went wrong:
    Could not resolve all files for configuration ':classUp:debugAnnotationProcessorClasspath'.
    > Could not resolve com.jakewharton:butterknife-compiler:8.4.0.
      Required by:
        project :app
      > No cached version of com.jakewharton:butterknife-compiler:8.4.0 available for offline mode.
      > No cached version of com.jakewharton:butterknife-compiler:8.4.0 available for offline mode.
      > No cached version of com.jakewharton:butterknife-compiler:8.4.0 available for offline mode.
      > No cached version of com.jakewharton:butterknife-compiler:8.4.0 available for offline mode.

**File** -> **Other Settings** -> **Default Settings...** 로 들어가서 **Build, Execution, Deployment** -> **Build Tools** -> **Gradle**에서 **Offline work**도 체크해제 해주었고, **Compiler**에서도 **--offline**를 없애주었는데도 같은 에러가 계속 발생했다.

해결은... **Android Studio** -> **Preferences...** 에서 **Offile work**와 **--offline**을 없애주어야 한다.
