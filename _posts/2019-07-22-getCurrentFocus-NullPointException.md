---
layout: post
title:  "안드로이드 getCurrentFocus() NullPointException.(getCurrentFocus() NullPointException)."
date:   2019-07-22 16:17:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android 테마 Theme
sitemap :
  changefreq : daily
  priority : 1.0
---

항상 알면서도 실수하는 부분이 있는 것 같아서 이번 버그는 적어두려고 한다.

```
Caused by java.lang.NullPointerException
Attempt to invoke virtual method 'android.os.IBinder android.view.View.getWindowToken()' on a null object reference
```

화면 내에 뒤로가기 버튼을 따로 또 넣었는데, 이 버튼을 누르면 키보드를 제거한 후에 화면을 끝낸다.

```java
public void backBtnPressed(View v) {
  InputMethodManager inputMethodManager = (InputMethodManager) getSystemService(INPUT_METHOD_SERVICE);
  inputMethodManager.hideSoftInputFromWindow(getCurrentFocus().getWindowToken(), 0);
  finish();
}
```

여기서 꼭 고민해야 할 부분은 `getCurrentFocus()`를 부를 때 현재 포커스 된 뷰가 있는지를 판단하는 것이다.

#### 시나리오
새로운 액티비티 -> 키보드 안 뜸, 포커싱 된 뷰가 없음 -> 바로 뒤로가기 버튼 누르기 -> 꺼짐

즉 `EditText`와 같은 곳에 포커싱이 되어 있으면 절대 꺼지지 않지만... 그렇지 않으면 꺼진다는 뜻이다.
