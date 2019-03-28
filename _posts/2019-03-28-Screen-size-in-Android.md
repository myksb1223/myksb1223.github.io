---
layout: post
title:  "안드로이드 진짜 화면크기 가져오기.(Get real screen size in Android)."
date:   2019-03-28 16:16:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android Screen 화면 ScreenSize 화면크기
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)의 메인 시간표를 Bitmap으로 저장하는 도중 새로 나온 폰에서는 전체화면으로 저장되지 않고 픽셀이 일정부분 빈 상태로 저장되는 현상이 생겼다.

기본적으로 화면의 크기를 가져올 때는 아래와 같이 구한다.

```java
DisplayMetrics metrics = getResources().getDisplayMetrics();
int screenHeight = metrics.heightPixels;
```

일단, 현재 Activity크기와 metrics의 크기가 같은지 체크를 위해 아래와 같이 테스트를 하였다.

```java
// 현재 Activity의 최상위 Layout의 id를 rootLayout으로 설정함.
rootLayout.getViewTreeObserver().addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {

    @Override
    public void onGlobalLayout() {
        int heightDiff = rootLayout.getRootView().getHeight()- rootLayout.getHeight();
        Log.d(TAG, "Activity height : " + rootLayout.getHeight() + ", ScreenHeight : " + rootLayout.getRootView().getHeight() + ", metrics height : " + metrics.heightPixels);
    }
});
```
위와 같이 사용하면, 새로나온 폰에서는 아래와 같이 나온다.

    Activity height : 3026, Screen height : 3120, Metrics height : 2858

일단 Activity height와 Screen height의 차이는 status bar의 크기만큼인 것으로 예상할 수 있고, 실제로 status bar의 높이를 아래처럼 가져오면 정확히 94라는 값이 나온다.

```java
public static int getStatusBarHeight(Context context) {
    int	result = 0;
    int resourceId = context.getResources().getIdentifier("status_bar_height", "dimen", "android");
    if (resourceId > 0) {
        result = context.getResources().getDimensionPixelSize(resourceId);
    }
    return result;
}
```

그러면 Screen height와 Metrics height의 차이는 도대체 무엇일까 고민하였다.

여기서, 새로나온 폰의 특징은 Navigation bar가 기기자체에 붙어서 나온 기종이 아닌, 스크린 내부에 들어간 기종이었다. 그래서 혹시나 하고, Navigation bar의 크기를 아래 처럼 불러왔다. [여기](https://stackoverflow.com/a/4744499)를 참고하였다.

```java
public static int getNavigationBarHeight(Context context) {
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN_MR1) {
        DisplayMetrics metrics = new DisplayMetrics();
        ((Activity)context).getWindowManager().getDefaultDisplay().getMetrics(metrics);
        int usableHeight = metrics.heightPixels;
        ((Activity)context).getWindowManager().getDefaultDisplay().getRealMetrics(metrics);
        int realHeight = metrics.heightPixels;
        if (realHeight > usableHeight)
            return realHeight - usableHeight;
        else
            return 0;
    }
    return 0;
}
```

정확히 262라는 값을 가져온 것이다.

결론적으로 안드로이드이 Navigation bar가 스크린 내에 존재하는 폰인 경우 Metrics의 높이는 Navigation bar의 높이를 제외하고 값을 가져오는 것이다. Navigation bar가 기기에 붙어서 나오는 기종은 0이라는 값을 반환한다.

즉, 아래와 같다.

    화면의 전체 사이즈 =  Metrics의 높이 + Navigation bar의 높이

Activity 화면의 크기를 가져와 봐야겠다는 생각을 하지 않았으면 찾기 힘들었을 수 있었는데 운이 좋았던 것 같다.
