---
layout: post
title:  "Gradle 업데이트 시 project refresh failed 에러발생(Gradle update project refresh failed error)"
date:   2018-08-08 18:28:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 안드로이드 Android Gradle ProjectFefreshFailed 에러 error Gradle업데이트 GradleUpdate
sitemap :
  changefreq : daily
  priority : 1.0
---

안드로이드 스튜디오(Android Studio)의 Gradle을 4로 업데이트 후에, Firebase 통합을 위해

    classpath 'com.google.gms:google-services:3.3.1'

아래를 설정한 후, Sync를 하자마자... 아래와 같은 에러가 발생했다.

    Gradle 'ClassUp 7.1.1_2' project refresh failed
        Error:
          More than one variant of project :library matches the consumer attributes:
          - Configuration ':library:debugApiElements' variant android-aidl:
          - Found artifactType 'android-aidl' but wasn't required.
          - Required com.android.build.api.attributes.BuildTypeAttr 'debug' and found compatible value 'debug'.
          - Required com.android.build.gradle.internal.dependency.AndroidTypeAttr 'Aar' and found compatible value 'Aar'.
          - Found com.android.build.gradle.internal.dependency.VariantAttr 'debug' but wasn't required.
          - Required org.gradle.api.attributes.Usage 'java-api' and found compatible value 'java-api'.
          - Configuration ':library:debugApiElements' variant android-classes:
          - Found artifactType 'android-classes' but wasn't required.
          - Required com.android.build.api.attributes.BuildTypeAttr 'debug' and found compatible value 'debug'.
          - Required com.android.build.gradle.internal.dependency.AndroidTypeAttr 'Aar' and found compatible value 'Aar'.
          - Found com.android.build.gradle.internal.dependency.VariantAttr 'debug' but wasn't required.
          - Required org.gradle.api.attributes.Usage 'java-api' and found compatible value 'java-api'.
          - Configuration ':library:debugApiElements' variant android-manifest:
          - Found artifactType 'android-manifest' but wasn't required.
          - Required com.android.build.api.attributes.BuildTypeAttr 'debug' and found compatible value 'debug'.
          - Required com.android.build.gradle.internal.dependency.AndroidTypeAttr 'Aar' and found compatible value 'Aar'.
          - Found com.android.build.gradle.internal.dependency.VariantAttr 'debug' but wasn't required.
          - Required org.gradle.api.attributes.Usage 'java-api' and found compatible value 'java-api'.
          - Configuration ':library:debugApiElements' variant android-renderscript:
          - Found artifactType 'android-renderscript' but wasn't required.
          - Required com.android.build.api.attributes.BuildTypeAttr 'debug' and found compatible value 'debug'.
          - Required com.android.build.gradle.internal.dependency.AndroidTypeAttr 'Aar' and found compatible value 'Aar'.
          - Found com.android.build.gradle.internal.dependency.VariantAttr 'debug' but wasn't required.
          - Required org.gradle.api.attributes.Usage 'java-api' and found compatible value 'java-api'.
          - Configuration ':library:debugApiElements' variant jar:
          - Found artifactType 'jar' but wasn't required.
          - Required com.android.build.api.attributes.BuildTypeAttr 'debug' and found compatible value 'debug'.
          - Required com.android.build.gradle.internal.dependency.AndroidTypeAttr 'Aar' and found compatible value 'Aar'.
          - Found com.android.build.gradle.internal.dependency.VariantAttr 'debug' but wasn't required.
          - Required org.gradle.api.attributes.Usage 'java-api' and found compatible value 'java-api'.

해결법은 시간이 없어서 3.2.1 버전으로 바꿔주었다.
