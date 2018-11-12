---
layout: post
title:  "안드로이드 commit()와 같은 메소드 사용시 주의사항(Caution when you use mehtod like commit())."
date:   2018-11-13 04:17:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 개발하던 중... SharedPreferences와 FragmentTransaction을 사용할 때에 생긴 실수이다.

```java
// 보통 아래와 같이 사용.
SharedPreferences prefs = getSharedPreferences(NAME, MODE);
SharedPreferences.Editor editor = prefs.edit();
editor.putInt(KEY, VALUE);
editor.commit();

// 아래와 같이 사용하면 안됨.

prefs.edit().putInt(KEY, VALUE);
prefs.edit().commit();
```

위의 이유는 SharedPreferences 소스를 보면 알겠지만 `prefs.edit()`을 할 경우 새로운 Editor 객체를 반환한다.

FragmentTransaction도 마찬가지이다.
```java
// 보통 아래 두가지 경우로 사용하는데...
android.support.v4.app.FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();
transaction.add(LAYOUT, FRAGMENT, TAG);
transaction.hide(FRAGMENT2).commit();

// 또는

getSupportFragmentManager().beginTransaction().add(LAYOUT, FRAGMENT, TAG).commit();
getSupportFragmentManager().beginTransaction().hide(FRAGMENT2).commit();

//아래 처럼 사용하면 안됨.

android.support.v4.app.FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();
transaction.add(LAYOUT, FRAGMENT, TAG).commit();
transaction.hide(FRAGMENT2).commit();
```

위도 비슷한 상황이다. 이미 transaction으로 commit()을 했는데 같은 녀석으로 commit()을 또하면 에러가 나는 것이다. 그래서 `getSupportFragmentManager().beginTransaction()` 처럼 불러준다.
