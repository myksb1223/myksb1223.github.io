---
layout: post
title:  "안드로이드 홈 스크린 위젯(앱 위젯) ANR(ANR in Android HomeScreen Widgets)."
date:   2018-10-22 23:07:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android 홈스크린위젯 HomeScreenWidgets ANR AppWidgets
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)의 홈스크린 위젯 중에는 background 이미지를 호출하는 경우가 있다. 이 경우 이미지를 로딩하다 ANR이 발생하는 것이 확인되었다.

결국 이미지 로딩할 때 비동기 처리를 하기로 했다. 당연한 것이지만 설마했는데...

AsyncTask를 사용하기로 했고 아래와 같이 쉽게 구현할 수 있다.

일단, AsyncTask를 커스텀한 클래스는 멤버변수로 RemoteViews, AppWidgetManager, appWidgetId를 가져야 한다.

왜냐하면 해당 이미지가 로딩되었을 때, `updateAppWidget()`를 호출해 주어야 하기 때문이다.

```Android
public static class ShowImageTask extends AsyncTask<Void, Void, Bitmap> {

  AppWidgetManager appWidgetManager;
  RemoteViews views;
  int appWidgetId;

  ShowImageTask(AppWidgetManager appWidgetManager, int appWidgetId, RemoteViews views, ...) {
    this.appWidgetManager = appWidgetManager;
    this.views = views;
    this.appWidgetId = appWidgetId;
    // other values...
  }

  @Override
  protected Bitmap doInBackground(Void... voids) {
    Bitmap background = null;

    // Bitmap 만들기.

    return background;
  }

  @Override
  protected void onPostExecute(Bitmap result) {
    if(result == null) {
      // background가 없을 경우.
    }
    else {
      views.setImageViewBitmap(R.id.widgetBackground, result);
    }

    try {
      appWidgetManager.updateAppWidget(appWidgetId, views);
    } catch (IllegalArgumentException ex) {
      ex.printStackTrace();
    }

    super.onPostExecute(result);
  }
}
```

위젯 업데이트 하는 부분에서 아래와 같이 호출해주면 된다.

```Android
new ShowImagetask(appWidgetManager, appWidgetId, views).execute();
```

이렇게 되면 자연스럽게 위젯에서 이미지를 로딩할 수 있다.
