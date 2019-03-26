---
layout: post
title:  "안드로이드 커스텀 뷰 2편.(CustomView in Android 2nd)."
date:   2019-03-27 00:04:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android 커스텀뷰 CustomView
sitemap :
  changefreq : daily
  priority : 1.0
---

[이전 글]에 이어서 이번에는 커스텀 뷰(CustomView)에 대해 조금 더 알아보면...

이전까지는 레이아웃을 가져와서 작업을 했다면 이번에는 기본적으로 `View`를 상속받은 다음에 직접 하나하나 그리는 방식이다.

직접 그리기 위해서는 `onDraw`를 사용한다. 그러면 직접 그리는 방식의 장점은 무엇일까?

기본적으로 레이아웃 파일을 이용하여 뷰를 가져오면 처리속도가 느리다. 그렇기 때문에 한 화면에 많은 뷰가 들어가면 들어갈 수록 속도는 점점 느려진다.

클래스업(ClassUp)의 경우 수업 화면이 무한 가로 스크롤이 되는 구조인데, 레이아웃을 이용하여 수업을 20개 정도 넣으면 확연히 속도가 느려지는 것을 체감할 수 있다. `ViewSwitcher`를 이용하고, 뷰를 옮길 때마다 매번 20개씩 다시 그려야하기 때문이다.

그래서 수업하면 전체를 직접 `onDraw`로 그리는 방식을 선택했고 속도는 정말 말도 안되게 개선되었다.

어쨌든 조금 복잡한 레이아웃 구조라면 `onDraw`를 이용하여 그리는 방식을 추천한다.

```java
class CustomView extends View {
  public CustomView(Context context) {
    super(context);
  }

  @Override
  protected void onDraw(Canvas canvas) {
    // ...
  }
}
```

`onDraw`는 `Canvas`객체에 직접 하나씩 그리는 작업을 하는 곳이다. 예를 들어 글씨를 쓰고 싶다면 아래와 같이 할 수 있다.

```java
@Override
protected void onDraw(Canvas canvas) {
  String str = "CustomView";
  Paint p = new Paint();
  // p에 대한 설정값.
  canvas.drawText(str, x, y, p);
}
```

선들을 그리고 싶다면 아래와 같이 할 수 있다.

```java
@Override
protected void onDraw(Canvas canvas) {
  Paint p = new Paint();
  // p에 대한 설정값.

  int linesIndex = ...; // 라인의 수
  float[] lines = new float[라인의 수*4];
  for(int i=0; i<linesIndex; i++) {
    lines[i] = ...; // 시작의 x 위치
    lines[i] = ...; // 시작의 y 위치
    lines[i] = ...; // 종료의 x 위치
    lines[i] = ...; // 종료의 y 위치
  }

  canvas.drawLines(mLines, 0, linesIndex, p);
}
```

사각형을 그리고 싶다면...

```java
@Override
protected void onDraw(Canvas canvas) {
  Paint p = new Paint();
  // p에 대한 설정값.
  Rect r = new Rect();
  // r에 대한 설정값.

  canvas.drawRect(r, p);
}
```

이렇게 다양한 방법으로 그릴 수 있다. 이미지도 그릴 수 있고 `StaticLayout`등과 같은 `View`를 만들고 `draw`를 사용하여 레이아웃도 그릴 수 있다.

그 외에 `View`를 상속받았을 때 사용할 수 있는 다양한 메소드를 이용하여 원하는 나만의 뷰를 만들 수 있다.

조금 더 나은 개발을 위해서는 꼭 `onDraw`로 뷰를 직접 그리는 방식을 해보길 추천한다.
