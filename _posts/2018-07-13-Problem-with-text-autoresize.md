---
layout: post
title:  "클래스업(ClassUp) 글자 크기 제한 문제...(Problem with text autoresize)"
date:   2018-07-13 19:08:00
author: Seungbeom Kim
categories: develop_diary
---

클래스업(ClassUp)에 [텍스트 크기 자동 조절](https://myksb1223.github.io/develop_diary/2018/07/12/Text-auto-resizing.html)에 대한 기능을 시험삼아 넣어보았다.
여기서 생기는 문제가 한가지 있다.

만약, 수업박스의 크기가 작아지면 글씨가 거의 0이 된다. 그래서 최소 크기를 변경 가능한 최소크기인 6으로 지정하였다. (앱 내부에서 6~14 까지 조정할 수 있다.)
하지만 글자 크기가 6이라도 수업박스가 작아지면 글씨가 보이지 않는다.

이 문제 해결에 대한 이야기를 하기 전에 몇 가지 클래스업(ClassUp)의 수업 표시 방법에 대한 이해가 필요하다.

클래스업은 시간표 화면의 특징
1. 스크롤을 지원하지 않는다.
즉, 화면 크기에 따라 수업박스 크기가 변한다. 시간의 최소단위는 5분.
2. 사용자가 수업을 넣는 시간에 따라 시간표의 시간표시가 자동으로 변한다.<br>
예를 들면, 08시~10시, 16시~18시 수업을 넣으면 시간표는 08시~18시까지 시간을 표시하는 시간표가 된다. 여기서 18시~20시 수업을 넣으면 시간표는 08시~20시까지로 시간이 확장된다. 이 때, 최소 5분단위의 크기는 2시간 늘어난 만큼 줄어든다.

<figure>
<img src="{{ site.baseurl }}/assets/develop_diary/problem_autoresizing_1.png" title="AppWidget Picture 1" class="post-image"><img src="{{ site.baseurl }}/assets/develop_diary/problem_autoresizing_2.png" title="AppWidget Picture 2" class="post-image">
<figcaption>왼쪽: 08시~18시, 오른쪽 08시~20시</figcaption>
</figure>
<p style="clear: left;">
간단하게 식으로 표현하면...
screen_height : 화면 크기, delta_time : 종료시간 - 시작시간.
5분의 height = screen_height / (delta_time * 60(분)) * 5(분)
즉, delta_time이 증가하면 결국 5분의 height 값은 작아진다.
</p>

그래서 생각해 낸 방법이 delta_time이 a라는 값을 넘어가면 화면이 스크롤이 가능하도록 만들지 사용자에게 물어보는 것이다.

사용자가 만약 스크롤을 원하면, 5분의 height는 delta_time이 a라는 값일 때의 높이로 고정된다.

이런 방법으로 어느 정도 문제가 해결되기는 한다. 하지만 5분, 10분 등으로 수업을 생성할 경우 어차피 글씨는 잘리거나 보이지 않을 것이다. 하지만 설마... 5분, 10분 수업을 넣는 사람이 있을까? 라는 생각을 하면서 나름 만족하고 있다...

<img src="{{ site.baseurl }}/assets/develop_diary/problem_autoresizing_3.png" title="AppWidget Picture 1" class="post-image">

<p style="clear: left;">
혹시, 더 좋은 방법있다면 댓글이나 쪽지주세요~!
</p>
