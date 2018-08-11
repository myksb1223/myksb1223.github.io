---
layout: post
title:  "안드로이드 위젯으로 앱 들어갈 때 액티비티가 쌓이는 현상(Activity is stacked when home screen widgets is clicked)"
date:   2018-08-12 03:43:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 안드로이드 Android 에러 error 위젯 widget 홈화면위젯 HomeScreenWidgets 액티비티 Activity 액티비티중첩 StackedActivity
sitemap :
  changefreq : daily
  priority : 1.0
---

안드로이드 클래스업(ClassUp) 홈 화면 위젯(HomeScreenWidgets)을 클릭한 후, 홈 버튼을 누르고 앱 아이콘을 누를 때 액티비티(Activity)가 중첩된다.

인텐트(Intent) 플래그에 NEW_TASK, CLEAR_TASK, CLEAR_TOP을 써봤지만 아무런 소용이 없었다.

그래서 manifest 파일의 launch 모드를 sinlgeTop으로 바꿔도 보았지만 소용이 없었다. sinlgeTask나 singleInstance는 무조건 내가 마지막에 본 액티비티(Activity)를 무시하고 메인으로 돌아가기 때문에 사용할 생각자체가 없었다.

구글메일(Gmail), 옴니노트(Omni note), 에버노트(Evernote) 등 해당 위젯을 참고하고, 옴니노트는 git에 소스가 공개되어 있어서 분석도 했다. 하지만 옴니노트는 singleTask를 쓰고 있었기에 우리와 전혀 맞지 않았고, 프래그먼트(Fragment)를 사용하기때문에 액티비티(Activity)를 새롭게 호출하는 코드자체가 없었다.

지금 가지고 있는 구조를 전부 프래그먼트(Fragment)로 바꿀 수는 없었고 다른 방법을 생각했다.

그러다가 갑자기 구글캘린더(Google Canlendar)앱이 공개된 것이 생각나서 소스를 분석했다. 여기에 해답이 보였다.

    intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK |
    Intent.FLAG_ACTIVITY_TASK_ON_HOME);

위와 같은 플래그를 사용하는 것이었다. [링크](https://stackoverflow.com/questions/21833402/difference-between-intent-flag-activity-clear-task-and-intent-flag-activity-task)에 FLAG_ACTIVITY_TASK_ON_HOME 설명이 제대로 나와있다. 여기 플래그를 사용하면 안드로이드 상위버전에서는 딱 두번만 중첩된다. 그래서 여기에서 메인 액티비티(Activity)에 아래의 코드를 넣어주었다.

```java
if (!isTaskRoot()) {
  finish();
  return;
}
```

즉, 실행될 때 Root가 아니면 그냥 액티비티를 종료하라는 것이다.
이 두 코드의 조합으로 해결하였다.

또 다른 방법은 더미액티비티를 사용하는 것이다. [링크](https://stackoverflow.com/questions/23838983/flag-activity-new-task-not-behaving-as-expected-when-used-in-pendingintent)를 참조하면 쉽다.

즉, 위젯이나 알림에서 앱을 들어갈 때, 메인 액티비티를 실행하는 것이 아닌 더미액티비티를 실행하고, 메인 액티비티가 실행 중이 아니라면 실행하고, 실행중이라면 아무것도 안하고 그냥 더미액티비티 자체를 종료하면 된다.

메인 액티비티가 실행 중
```java
//MainActivity.java
static MainActivity main = this;

//DummyActivity.javac

if(MainActivity.main != null && MainActivity.main.isTaskRoot()) {
  Intent intent = new Intent(this, MainActivity.class);
  intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK | Intent.FLAG_ACTIVITY_CLEAR_TOP);
  startActivity(intent);
}

finish();
```

즉, 위와 같은 형태로 구현하면 된다. 첫 번째 경우는 위젯이나 알림을 누르고 들어가면 항상 메인 액티비티가, 앱 아이콘을 클릭하면 항상 마지막 액티비티가 보인다.

두 번째 경우는 항상 마지막 액티비티가 보인다. 앱을 종료하지 않는 이상...

이틀동안 이것을 고민했는데 해결되서 좋다. 역시 다른 사람의 소스를 참고하는 것은 정말 중요한 것 같다.
