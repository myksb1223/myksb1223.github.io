---
layout: post
title:  "안드로이드 홈 화면 위젯(Android Homescreen AppWidget)."
date:   2018-07-10 23:22:00
author: Seungbeom Kim
categories: develop_diary
---

안드로이드 홈 화면 위젯의 디자인 변경 및 몇 가지 기능을 추가했다.
클래스업(ClassUp)에는 5가지 종류의 위젯이 있다.
1. 오늘의 수업을 텍스트로 보여주는 위젯.
2. 오늘, 내일 수업을 텍스트로 보여주는 위젯.
3. 전체 수업을 큐브 형태로 쌓은 위젯.
4. 전체 수업을 시간표 형태로 보여주는 위젯.
5. 노트의 내용을 표시해주는 위젯.

오늘은 5번 위젯을 수정했다.

<img src="{{ site.baseurl }}/assets/develop_diary/android_appwidget_1.png" title="AppWidget Picture 1" class="post_image" width="300px" height="533px"><img src="{{ site.baseurl }}/assets/develop_diary/android_appwidget_2.png" title="AppWidget Picture 2" class="post_image" width="300px" height="533px">

unique_id(TEXT) |	server_timestamp(TEXT) | start_schedule_timestamp(TEXT) | note_type(INTEGER) |
-|-|-|-|

일단, 가독성을 조금 더 올리기 위해서 아래의 4부분을 수정하였다.
1. 별로 필요없는 오늘 날짜 크기를 줄임.
2. 섹션을 나눔.
3. 글자색 변경.
4. 글자 배치 변경.

디자인만 바꾼 것 같아서 별로 한 것이 없어보이지만... 몇가지 생각해야할 부분이 있었다.
주요한 부분은 섹션 나누기였다.
안드로이드는 기본 데이터베이스로 SQLite를 사용한다.
노트 테이블 구조를 약간 살펴보면 아래와 같다.

note_type == 0 이면 일반 노트, note_type == 1이면 일정이다.
server_timestamp : 서버에 등록된 노트의 시간.
start_schedule_timestamp : 일정의 시작 시간.

위젯의 시간 표시는 start_schedule_timestamp를 이용하는데, 일반 노트의 경우에는 start_schedule_timestamp 값이 NULL이다.
그래서 일단 값을 읽을 때,
1. note_type이 0이면 start_schedule_timestamp에 server_timestamp를 넣어준다.
2. start_schedule_timestamp가 오늘인지, 내일인지, 모레인지 등으로 나눠서 각각에 해당되는 노트를 넣어주어야 한다.
즉, Dictionary 형태가 필요하다. Key는 오늘, 내일, 모레 등의 값. Value는 List<노트> 이다.

```java
HashMap<String, LinkedList<WidgetNote>> dict = new HashMap<>();
Cursor mClassUpCursor = ...;
if(mClassUpCursor != null) {
	while(mClassUpCursor.moveToNext()) {
		String unique_id = mClassUpCursor.getString(0);
		String server_timestamp = mClassUpCursor.getString(1);
		String start_schedule_timestamp = mClassUpCursor.getString(12);

		if (note_type == 0) {
			start_schedule_timestamp = server_timestamp;
			end_schedule_timestamp = server_timestamp;
		}

		WidgetNote note = new WidgetNote(unique_id, ..., start_schedule_timestamp, ...);

		String key = getKeyForNote(server_timestamp);
		LinkedList<WidgetNote> timestampArr = null;
		if(dict.get(key) == null) {
			timestampArr = new LinkedList<WidgetNote>();
			dict.put(key, timestampArr);
		}
		else {
			timestampArr= dict.get(key);
		}
		timestampArr.add(note);
	}
	mClassUpCursor.close();
}

public String getKeyForNote(String sort_timestamp) {
	Date schedule_date = new Date(Long.parseLong(sort_timestamp));
	Locale current = context.getResources().getConfiguration().locale;
	SimpleDateFormat format = new SimpleDateFormat("yyyyMMdd", current);
	String sort_time_str = format.format(schedule_date);
	return sort_time_str;
}
```

코드는 위와 같이 표현할 수 있다.
참고로 제일 밑에 보이는 함수 getKeyForNote(String) :  20180710과 같은 형태로 timestamp값을 변경한다. (오늘, 내일, 모레 등의 구분은 단지 날짜만 필요하다.)

하나의 TextView에 여러개의 색을 입히고 싶을 경우에는 SpannableStringBuilder과 ForegroundColorSpan을 사용한다.

```java
int start = ..., end = ..., color = ...;
SpannableStringBuilder sb = new SpannableStringBuilder(content);
ForegroundColorSpan cs = new ForegroundColorSpan(color);
sb.setSpan(cs, start, end, Spannable.SPAN_INCLUSIVE_INCLUSIVE);
remoteView.setTextViewText(R.id.content, sb);
```

그렇게 복잡하지 않은 과정으로 작업이 마무리 된 것 같다.
RemoteViews는 생각하면 일반 View와 달리 제약이 많아서 조금 까다로운 것 같다.

안드로이드 홈 화면 위젯에서 ListView를 사용할 때, 가끔 폰 마다 바로바로 업데이트가 적용안되는 경우가 발생하는데, 혹시 그 원인을 아시는 분은 댓글이나 쪽지 부탁드립니다.

그리고 잘못된 부분이나 의견 혹은 질문 있으시면 댓글이나 쪽지나 메일등 주세요~!
