---
layout: post
title:  "안드로이드와 AWS SNS의 GCM을 FCM으로 통합하기.(Migration of Android with AWS SNS GCM to FCM.)"
date:   2018-08-12 04:32:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 안드로이드 Android GCM FCM 아마존클라우드서비스 AWS SNS SimpleNotifcationService
sitemap :
  changefreq : daily
  priority : 1.0
---

GCM을 사용했다면 [Google Cloud Platform](https://console.cloud.google.com/)에 프로젝트를 등록했을 것이다.

[Firebase console](https://console.firebase.google.com/)로 가서 GCM 프로젝트를 옮기도록 한다.

해당 프로젝트에 들어가서 톱니바퀴 모양을 누르면 프로젝트 설정이라고 있다. 들어가서 일단 google-services.json을 다운받아서 예전 google-services.json과 교체해 준다.

그리고는 [도큐먼트](https://developers.google.com/cloud-messaging/android/android-migrate-fcm)에서 시키는 대로 하면 된다.

마지막으로 AWS SNS로 가서 예전 GCM의 자격증명을 아까 설정에 들어갔을 때, **클라우드 메시징**이라는 탭으로 가서 서버키로 변경해주면 끝난다.

참고로, 안드로이드에서 device token이 새로 생성될 경우는 앱을 (재)설치할 경우, 디바이스를 교체할 경우이기 때문에 앱의 기능에 로그아웃 기능이나 탈퇴 기능이 있다고 해서 해당 token을 다시 불러올 필요가 없다.
