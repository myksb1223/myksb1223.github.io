---
layout: post
title:  "안드로이드 테마 건드리기.(Theme in Android)."
date:   2019-03-28 00:38:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android 테마 Theme
sitemap :
  changefreq : daily
  priority : 1.0
---

요즘은 어두운 테마가 유행이다. 요즘 나오는 폰들의 화면을 위해서 다들 어두운 테마 적용을 옵션으로 넣고 있다. 그래서 클래스업(ClassUp)도 어두운 테마를 적용하고 있고, `Style`에 대한 정리도 할 겸 글을 쓴다.

일단, 안드로이드의 디자인은 기본적으로 `Theme`를 기본으로 한다. 그래서 내가 어떤 테마를 쓰냐에 따라서 글씨색, 배경색등 다양한 부분이 바뀐다.

또한 원하는 액티비티, 원하는 뷰에 따로 테마를 적용할 수 있어 디자인하기도 쉽고, 테마 역시 상속의 개념을 지원하기 때문에 하나의 주 테마와 서브 테마로 나누어 사용할 수도 있다.

클래스업(ClassUp)을 기준으로 `Theme`적용에 대해서 설명하려고 한다.

- 클래스업(ClassUp)은 `Theme.AppCompat.Light.NoActionBar`를 기본으로 한다.
- 밝은 테마(`AppTheme`)와 어두운 테마(`AppTheme.BlackTheme`)로 나뉜다. (이름은 원하는 방식대로 지으면 된다.)

```xml
<!-- styles.xml -->
<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar" >
  // ...
  <item name="android:background">@color/white</item>
</style>

<style name="AppTheme.BlackTheme" parent="AppTheme">
  // ...
  <item name="android:background">@color/black</item>
</style>
```

굳이 `AppTheme`를 상속한 이유는 `AppTheme`의 정해진 값들을 사용하면서 어두운 테마로 변경할 것이기 때문이다.

여기서 내가 어떤 특정한 뷰에서는 기본 배경색이 아닌 다른 색을 사용하고 싶은 경우가 있다. 이 경우에는 아래와 같이 작성해 주면 된다.

```xml
<!-- styles.xml -->
<attr name="navigationbar_background" format="reference" />

<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar" >
  // ...
  <item name="android:background">@color/white</item>
  <item name="navigationbar_background">@color/blue</item>
</style>

<style name="AppTheme.BlackTheme" parent="AppTheme">
  // ...
  <item name="android:background">@color/black</item>
  <item name="navigationbar_background">@color/red</item>
</style>

// layout.xml

<View
  // ...
  android:background="?attr/navigationbar_background" />
```

위와 같이 사용하면 된다. `color`는 rgb형태로는 사용할 수 없기때문에 `colors.xml`에 작성한 후에 사용할 수 있다.

`drawable`내에 존재하는 파일 역시 적용할 수 있기 때문에 예를 들어 버튼의 선택여부에 관한 xml 파일을 작성하였다면 그 역시 적용할 수 있다. 단, 테마별로 xml파일을 만들어야 한다.

이렇게 작성을 다 했으면 이제 코드로 적용을 해야하는데... 선택되었을 때, 전체 모든 화면에 적용시켜야 하기때문에 아래와 같이 만들 수 있다.

```java
class BaseActivity extends AppCompatActivity {
  @Override
  public void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      SharedPreferences pref = ...;
      if(pref.getBoolean("isBlackTheme", false)) {
          this.setTheme(R.style.AppTheme_BlackTheme);
      }
      else {
          this.setTheme(R.style.AppTheme);
      }
  }

}

class MainActivity extends BaseActivity {
  // ...
}
```

위와 같이 하나의 BaseActivity를 만들고 테마를 적용시킨 후, 모든 Activity는 해당 BaseActivity를 상속받는 형식으로 사용하면 각각 테마를 적용하지 않고도 사용할 수 있다.

이제 iOS에 어두운 테마를 적용해 볼 차례인데... iOS는 어떤 방식으로 적용할 수 있을까...?
