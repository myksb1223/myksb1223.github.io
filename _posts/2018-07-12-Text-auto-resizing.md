---
layout: post
title:  "글자 크기 자동 조절(Text auto resizing)."
date:   2018-07-12 01:39:00
author: Seungbeom Kim
categories: develop_diary
---

클래스업 메인 화면은 아래 그림과 같다.<br>
여기서 몇가지 아쉬운 부분이라면... 수업 박스가 작을 경우 글씨가 표시되지 않는다.

<img src="{{ site.baseurl }}/assets/develop_diary/autoresizing_1.jpg" title="AppWidget Picture 1" class="post-image"><img src="{{ site.baseurl }}/assets/develop_diary/autoresizing_2.png" title="AppWidget Picture 2" class="post-image">


먼저, iOS를 보면...
iOS에서의 수업 박스 형태는 약간 커스터마이징(Customizing)되었기 때문에 [UIControl](https://developer.apple.com/documentation/uikit/uicontrol) 내에 여러개의 UILabel이 있는 형태이다. 즉 UIButton을 사용하지 않기 때문에 touch에 대한 부분을 모두 구현해 주어야 한다.

다시 본론으로 돌아와서...
수업박스에는 총 6가지가 표시된다.
1. 시작시간(선택)
2. 종료시간(선택)
3. 수업명(필수)
4. 강의실(선택)
5. 교수명(선택)
6. 이모티콘(선택)

수업명, 강의실, 교수명은 하나의 UILabel에 표시된다. 그렇게 때문에 결국 하나로 생각할 수 있다.

UILabel에는 예전부터 [adjustFontSizeToFitWidth](https://developer.apple.com/documentation/uikit/uilabel/1620546-adjustsfontsizetofitwidth)라는 옵션이 있다.
간단히 설명하면, 내용이 UILabel의 크기를 벗어나면 글씨를 알맞게 줄인다는 것이다. 이 때, 얼마까지 줄일 것인지 설정할 수 있는데, 기본값은 0이다.

그런데 줄어들지 않는다...
그래서 찾아봤더니... lineBreakMode를  NSLineBreakByClipping으로 바꾸라는 것이었다. 맞는 말이다. adjustFontSizeToFitWidth를 적용하면 절대로 WordWrapping되는 경우가 생기지 않을테니까 말이다.

```objective-c
self.subjectTitle.lineBreakMode = NSLineBreakByClipping;
self.subjectTitle.adjustsFontSizeToFitWidth = YES;
self.subjectTitle.minimumScaleFactor = 0.1;
```

그러면 이제 안드로이드는 어떻게 할지 찾아보았다.
안드로이드는 8.0부터 [텍스트 크기를 자동으로 조절하는 기능](https://developer.android.com/guide/topics/ui/look-and-feel/autosizing-textview)을 지원한다.

```java
if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
	subjectName.setAutoSizeTextTypeUniformWithConfiguration(6, textSize, 1, TypedValue.COMPLEX_UNIT_SP);
}
```

autoSizeMinTextSize는 0보다 커야한다.
autoSizeMaxTextSize는 최대로 증가할 수 있는 크기이다.
autoSizeStepGranularity는 얼마씩 크기를 줄일 건지를 나타낸다.
COMPLEX_UNIT_SP는 글자 크기를 sp형식으로 처리하겠다는 뜻이다.

<img src="{{ site.baseurl }}/assets/develop_diary/autoresizing_3.jpg" title="AppWidget Picture 1" class="post-image"><img src="{{ site.baseurl }}/assets/develop_diary/autoresizing_4.png" title="AppWidget Picture 2" class="post-image">


클래스업(ClassUp)에서는 사용자가 가장 적당한 크기로 글씨를 볼 수 있어야 하기 때문에 약간 세부적인 제어가 필요할 것 같다.

잘못된 부분, 질문등은 댓글이나 쪽지주세요~!
