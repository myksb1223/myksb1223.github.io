---
layout: post
title:  "안드로이드 홈 스크린 위젯 설정 창 주의사항(Note when making Android Homescreen Widget Configure)."
date:   2018-09-27 02:14:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android 홈스크린위젯 HomeScreenWidgets
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에 4.x 버전에서는 위젯이 설치되지 않는 현상이 발생했다. 아예 설치하면 저절로 onDelete() 메소드가 호출되는 것이다. 하지만 새로운 버전을 업데이트 하기 전까지 잘 되었다. 그래서 업데이트를 위한 코드 수정중에 잘못 수정된 부분이 있다는 것이라 생각했다.

Git을 이용한 버전저장을 해둔 덕에 다행이 쉽게 비교하며 문제의 원인을 찾을 수 있었다.

바로 AndroidManifest.xml에서 Configure Activity 등록 시 속성 값에 문제가 있었다.

    <activity android:name=".ClassUpAppTextLargeWidgetConfig"
            android:theme="@style/AppTheme"
            android:screenOrientation="portrait"
            android:launchMode="singleTask"
            android:taskAffinity=""
            android:excludeFromRecents="true">
            <intent-filter>
                <action android:name="android.appwidget.action.APPWIDGET_CONFIGURE"/>
            </intent-filter>
      </activity>

문제가 있는 버전에는 위와 같이 launchMode와 taskAffinity가 선언되어 있었다. 당연히 Android 4.x에서도 적용될 것으로 생각했으나 Configure Activity가 뜨고 완료 시 위젯이 없어진다.

launchMode와 taskAffinity를 없애주면 문제는 해결된다.
