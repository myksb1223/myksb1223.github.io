---
layout: post
title:  "안드로이드 테마 건드리기 2편.(Theme in Android 2nd)."
date:   2019-03-28 16:28:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android 테마 Theme
sitemap :
  changefreq : daily
  priority : 1.0
---

[안드로이드 테마 건드리기 1편](https://myksb1223.github.io/develop_diary/2019/03/28/Theme-in-Android.html) 보기.

클래스업(ClassUp)에 [어두운 테마를 적용](https://myksb1223.github.io/develop_diary/2019/03/28/Theme-in-Android.html) 중에 한가지 문제가 발생하였다.

메인 시간표 화면을 Bitmap으로 저장시켜 스크린샷으로 만드는 과정에서 전부 검은색 화면이 나오는 것이었다. 이 화면에는 어두운 테마를 적용할 필요가 없었다. 당연히, 각각 적용하면 되지만, 노가다가 있기에 하고 싶지 않았다.

먼저, xml 파일 자체에 `android:theme="@style/AppTheme"`와 같이도 적용해 보았지만 소용없었다.

그래서 Bitmap으로 저장시킬 뷰만 Style을 적용하는 방법을 찾던 도중 아래와 같은 좋은 방법을 찾았다. (`ContextThemeWrapper` 사용)

```java
LayoutInflater mInflater = LayoutInflater.from(new ContextThemeWrapper(context, R.style.AppTheme));
View v = mInflater.inflate(R.layout.view, null);
```

하지만 위의 방법은 먹히지 않았다. 왜 그런가 찾아보았더니... [여기](https://stackoverflow.com/a/31636876)에 이유가 있었다.

즉, 나도 같은 현상이라... 고민한 결과 유일하게 Theme를 적용시키지 않은 곳이 Application 부분이었다. 그래서 `getApplicationContext`를 사용해 보았다.

```java
LayoutInflater mInflater = LayoutInflater.from(context.getApplicationContext());
View v = mInflater.inflate(R.layout.view, null);
```

위와 같이 하니까 잘 적용된다.
