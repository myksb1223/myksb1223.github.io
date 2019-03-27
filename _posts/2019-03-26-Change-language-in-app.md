---
layout: post
title:  "안드로이드 앱 내부에서 언어 변경하기.(Android change language in app)."
date:   2019-03-26 03:00:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android 언어 Language Locale
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 인스타그램(Instagram)이나 페이스북(Facebook)과 같이 앱 내부 설정에 언어 변경 옵션을 넣으려고 생각했다.

일단, 사우디아라비아 쪽 사용자들은 아랍언어가 아닌 영어로 따로 사용하고 싶어하는 경우가 많았던 것 같다.

그럼 어떻게 하면 앱 자체의 언어만 변경할 수 있을까? 당연히 `Locale`과 관계된 것은 생각했지만 막상 방법을 몰라서 검색을 했다.

검색을 해보면 [여기](http://devdeeds.com/android-change-language-at-runtime/)에 아주 자세한 설명이 나와있다. 그리고 안드로이드 OS의 발전에 따라 조금씩 바뀌기 때문에 [여기](https://stackoverflow.com/a/42269965)도 참고하였다.

이 두 자료만 있으면 충분히 해결할 수 있다. 무조건 아래 글을 읽기전에 위의 두 링크를 자세히 파악하도록 하자.

내가 조금 더 첨부하자면...
1. 다시 시스템 언어로 돌아오고 싶은 경우.
2. HomeScreenWidget에 적용하는 방법.
3. 조금 더 센스있는 처리(?) 정도이다.

### 1. 시스템 언어로 돌아오기
내가 지원하는 언어목록에서 `시스템 언어`라는 Row를 클릭하면 시스템 언어로 돌아가고 싶다면...
```java
private Context updateBaseContextLocale(Context context) {

       SharedPreferences pref = ...;
       String language =  pref.getString("SelectedLanguage", null);
       Locale locale = Locale.getDefault();
       if(language != null){
           String[] splited = language.split("-");
           if(splited.length == 2) {
               locale = new Locale(splited[0], splited[1]);
           }
           else {
               locale = new Locale(language);
           }
       }

       Locale.setDefault(locale);

       if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
           return updateResourcesLocale(context, locale);
       }

       return updateResourcesLocaleLegacy(context, locale);
   }
```

분명 내가 원하는 언어를 누르면 `Intent`플래그에 `CLEAR_TOP`을 넣고는 `startActivity`를 호출할 것이기 때문에 위와 같이 처리해주면, `Locale locale = Locale.getDefault();` 이 부분은 시스템 언어를 받아온다.

만약에 `startActivity`를 호출하지 않는다면... 아래처럼 해주면 된다.
```java
// Locale locale = Locale.getDefault();
Locale locale = null;
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
  locale = Resources.getSystem().getConfiguration().getLocales().get(0);
}
else {
  locale = Resources.getSystem().getConfiguration().locale;
}
```

### 2. HomeScreenWidget에 적용하는 방법.

AppWidgetProvider를 상속하기에 반드시 `onUpdate`메소드가 있다. 이를 이용하면 된다.

```Java
public void onUpdate(Context context, AppWidgetManager appWidgetManager, int[] appWidgetIds) {
	Context updated = updateBaseContextLocale(context);
	super.onUpdate(updated, appWidgetManager, appWidgetIds);

	final int N = appWidgetIds.length;

	// Perform this loop procedure for each App Widget that belongs to this provider
	for (int i=0; i<N; i++) {
		try {
			updateAppWidget(updated, appWidgetManager, appWidgetIds[i]);
		} catch (TransactionTooLargeException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (RuntimeException re) {
			re.printStackTrace();
		}
	}
}
```

### 3. 조금 더 센스있는 처리(?).

아마 보통 앱을 개발하면 많은 Activity나 Fragment파일이 있을 것이다. 하지만 매번 하나하나 언어 변경에 관한 처리를 넣어주는 것은 비효율적이다. 앱 위젯(HomeScreenWidget)도 마찬가지이다.

즉, Activity, AppWidgetProvider를 상속받은 Base 클래스를 만드는 것이 답이다.

```java
class BaseActivity extends AppCompatActivity {

  // Locale 변경관련 코드
}

class BaseAppWidgetProvider extends AppWidgetProvider {
  // Locale 변경관련 코드
  // 여기에는 attachBaseContext메소드가 없기에 사용하지 않아도 상관없다.
}

class MainActivity extends BaseActivity {
  // ...
}

class MyWidgetProvider extends BaseAppWidgetProvider {
  // ...
}
```

그리고, 한가지 더 필요한 것은 만약에 내가 어떤 데이터(strings.xml의 일부가 포함된)를 계속 가지고 있는 상태라면 무조건 `super.attachBaseContext(updateBaseContextLocale(base));`를 수행하기 전에 관련 데이터를 다 지우고 다시 데이터를 만들어야지만 언어가 변경되어서 보인다. 잊지 않도록 하자!

이렇게 하면 기본적으로 원하는 커스텀 뷰를 간단하게 구현할 수 있다.
다음 편에서는 Objective-c에서의 앱내 언어 변경에 관한 글을 쓰려고 한다.
