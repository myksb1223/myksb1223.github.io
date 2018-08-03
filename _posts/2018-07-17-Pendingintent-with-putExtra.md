---
layout: post
title:  "PendingIntent에서 값 넘기기(Pendingintent with putExtra)."
date:   2018-07-17 17:09:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 안드로이드 Android PendingIntent PutExtra PendingIntent값넘기기 안드로이드개발 Android개발
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)은 기본적으로 메인화면이 두개이다. 하나는 시간표 창, 다른 하나는 노트 창.
즉, 마지막으로 본 창이 무엇이냐에 따라 앱이 종료된 후 다시 켰을 때 보이게 되는 창이 결정된다.

시간표 창 -> 종료 -> 재실행 -> 시간표 창
노트 창 -> 종료 -> 재실행 -> 노트창

간단하게 위와 같은 식이다. 뭐 잘못된 것은 없다. 다만... 노트 위젯을 누르면 노트 창으로 가게끔 하고 싶었다.

주절주절... 정리해 보면!
지금 클래스업(ClassUp) 안드로이드 버전에서는 노트 위젯을 누르면 바로 노트 창이 열리는 것이 아닌 마지막으로 종료된 창이 열린다. 그래서 이 부분을 바꾸겠다는 것이다.

간단하게 아래와 같은 과정으로 생각했다.
1. 구분자를 넘긴다.
2. 구분자를 확인하다.
3. 확인이 되면 노트 창을 연다.

```java
Intent intent = new Intent(context, ClassUpActivity.class);
intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK|Intent.FLAG_ACTIVITY_SINGLE_TOP|Intent.FLAG_ACTIVITY_CLEAR_TOP);
intent.putExtra("type", 2);
PendingIntent pendingIntent = PendingIntent.getActivity(context, appWidgetId, intent, PendingIntent.FLAG_UPDATE_CURRENT);
views.setOnClickPendingIntent(R.id.rootLayout, pendingIntent);
```

그러면 Activity에서 type이 2인지만 확인하면 된다.

```java
Bundle extras = getIntent().getExtras();
if(extras != null) {
	if (extras.containsKey("pType")) {
		int type = extras.getInt("pType");
		if(type == 2) {
			//노트창으로 이동...
		}
	}
}
```

위의 코드를 넣었는데... 안되는 경우가 발생한다.

1. 실행 -> 시간표 창 이동 -> 종료 -> 노트 위젯 클릭 (0)
2. 노트 위젯 클릭 실행 -> 시간표 창 이동 -> 홈 버튼 -> 노트 위젯 클릭 (0)
3. 실행 -> 시간표 창 이동 -> 종료 -> 앱 아이콘으로 앱 재실행 -> 홈 버튼 -> 노트 위젯 클릭 (X)

위 경우를 잘 살펴보면, 2번과 3번의 차이점은 '무엇으로 앱을 실행시켰는가?' 이다.
앱 아이콘으로 실행한 경우에는 type이 넘어오지 않고, 노트 위젯으로 실행하면 type이 넘어온다.
그 이후 몇 번이고 홈 화면으로 간 후 돌아오면 전자는 계속 type이 없고, 후자는 계속 type이 있다.

즉, 홈 버튼으로 background 상태로 앱이 들어갔다가 다시 foreground 상태로 돌아올 때 어디를 통하든 상관없이 getIntent()의 값은 그대로라는 것이다.

```java
public void onNewIntent(Intent intent) {
	super.onNewIntent(intent);
	setIntent(intent);
}
```

위와 같이 foreground로 돌아올 때, onNewIntent()를 Override(오버라이드)하면 방금의 문제는 해결된다.

Activity 생명주기와 같은 기본을 간과해서 헤맨 경우라 볼 수 있는 것 같다.

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
