---
layout: post
title:  "UIScrollView와 AutoLayout(UIScrollView with AutoLayout)"
date:   2018-07-17 02:014:00
author: Seungbeom Kim
categories: develop_diary
---

바로 이전에 올린 클래스업(ClassUp) 글자 크기 제한 문제에서 UIScrollView를 사용해서 해결했다.
UIScrollView와 AutoLayout의 관계(?), 사용법을 조금 생각해 보려고 한다.

AutoLayout이 나온 지는 꽤 되었다. 하지만 나는 2011년 부터 아이폰 개발을 건드렸기 때문에... AutoLayout보다 사실 그냥 코드로 Layout잡는 것이 훨씬 익숙했다. 하지만 역시... 대세는 AutoLayout이었고 여기에 맞춰갈 수 밖에 없었다. 그래서 클래스업(ClassUp)을 전부 AutoLayout을 사용하도록 변경하였는데...

UIScrollView에서 헤맸던 기억이 있고, 오랜만에 다시 건드리려고 하니 기억이 잘 나지 않아서 여기에 정리해 보려고 한다.

일단, UIScrollView에 대해서 잠깐 생각해 보도록 하면...
ContentSize라는 개념이 존재한다. 즉, 화면사이즈는 제한적인데 이 화면사이즈보다 Contents의 내용의 크기가 클 경우 ContentSize라는 값을 조정함으로써 Scroll 범위를 정할 수 있다.

예전의 코드 방식에서는...

```objective-c
CGSize contentSize = ...;
self.scrollView.contentSize = contentSize;
```

위와 같이 적용을 하면 ScrollView는 적용된 사이즈에서 ScrollView의 frame 사이즈를 뺀 만큼 scroll을 할 수 있다.

즉, 스크롤 가능한 범위는
int scrollable_size = self.scrollView.contentSize.height - self.scrollView.frame.size.height; 라고 정의할 수 있겠다.

그러면 이제 AutoLayout에서는 어떻게 이 contentSize를 자동적으로 변형시킬 수 있을까? 라는 부분이 핵심이라고 할 수 있다.

AutoLayut은 기본적으로 interface bulilder를 이용하여 작업하기 때문에 앱이 실행 중에 값이 변경되지 않는 이상 따로 코드를 사용할 필요가 없다.

여기서 생기는 문제는 바로... ScrollView내에 Contents(UIView, UILabel 등...)를 넣게 되면 Need constraints for: X position or width, Need constraints for: Y position or height 라는 에러를 만나게 된다. 즉, ScrollView의 ContentSize를 자동적으로 캐치하기 위해 가로, 세로 / 너비, 높이에 대한 제약 조건이 필요하다는 것이다.

여기서 ScrollView내에 들어가는 Contents의 종류가 많다면 하나하나 x, y좌표나 width, height를 지정해주어야 한다. 난 AutoLayout을 사용하는데 왜... width와 height를 지정해 줘야하는가? 라는 의문이 생긴다. 그리고 맨 위쪽, 맨 아래 쪽에 위치하는 뷰는 ScrollView와의 Top space, Bottom space를 맞춰주어야 하는 번거로움이 생긴다.

<img src="{{ site.baseurl }}/assets/develop_diary/uiscrollview_autolayout_1.png" title="UIScrollView AutoLayout Picture 1" class="post-image" width="100%">

위 방법의 가장 큰 문제점은 width와 height를 지정하면 기기의 화면에 때라 Layout이 다르게 표시가 된다... 즉, 여백이 일정하지 않다.

<figure>
<img src="{{ site.baseurl }}/assets/develop_diary/uiscrollview_autolayout_2.png" title="UIScrollView AutoLayout Picture 2" class="post-image-double"><img src="{{ site.baseurl }}/assets/develop_diary/uiscrollview_autolayout_3.jpeg" title="UIScrollView AutoLayout Picture 3" class="post-image-double">
<figcaption style="text-align: center;">왼쪽 : iPhone5, 오른쪽 : iPhone7Plus</figcaption>
</figure>

그래서 보통 ScrollView 내에 Container와 같은 역할을 하는 UIView를 넣어준다. Container의 Top, Bottom, Leading, Trailing Space를 ScrollView에 맞춰주고 Width와 Height만 지정해주면 들어가야하는 Contents들은 모두 Container에 넣어주고 Layout을 맞춰주면 된다. 그리고 마지막으로 Contents들의 가장 긴(?) width, height를 Container에 적용시켜주면 끝난다.

<img src="{{ site.baseurl }}/assets/develop_diary/uiscrollview_autolayout_4.png" title="UIScrollView AutoLayout Picture 4" class="post-image" width="100%">

아래 처럼 여백이 항상 같은 것을 알 수 있다.

<figure>
<img src="{{ site.baseurl }}/assets/develop_diary/uiscrollview_autolayout_5.png" title="UIScrollView AutoLayout Picture 5" class="post-image-double"><img src="{{ site.baseurl }}/assets/develop_diary/uiscrollview_autolayout_6.jpeg" title="UIScrollView AutoLayout Picture 6" class="post-image-double">
<figcaption style="text-align: center;">왼쪽 : iPhone5, 오른쪽 : iPhone7Plus</figcaption>
</figure>

이렇게 UIScrollView의 AutoLayout은 생각할 부분이 조금 더 많다!

질문이나 잘못된 점, 수정부분 있으면 댓글 부탁드립니다.!!
