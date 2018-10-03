---
layout: post
title:  "html page 모바일 화면에 자동으로 맞추기(Html page fit mobile automatically)."
date:   2018-10-04 01:00:00
author: Seungbeom Kim
categories: develop_diary
tags:	html Web
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 수업을 편하게 추가하고 공유할 수 있는 방법으로 링크 생성 및 공유를 생각했다.

즉, 링크를 생성해서 페이지로 들어간 후, JOIN 버튼을 누르면 앱이 켜지면서 자동적으로 수업이 들어가는 구조이다.

정말 오랜만에(약 7년) Web을 다루려고 하니 앞이 막막했다. 어느정도 desktop 환경에서는 잘 보이는데, 모바일에서는 아주 작은 사이즈로 나왔다.

Mac Safari

<img src="{{ site.baseurl }}/assets/develop_diary/html_page_1.png" title="html page 1" class="post-image">

iPhone Safari

<img src="{{ site.baseurl }}/assets/develop_diary/html_page_2.png" title="html page 2" class="post-image">


그래서 자동적으로 모바일 화면에 맞추는 기능이 나왔을 것 같아 찾아보았다.

```Html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

위와 같이 meta 태그의 viewport 속성을 사용하면 된다.

적용 후 iPhone Safari

<img src="{{ site.baseurl }}/assets/develop_diary/html_page_3.png" title="html page 3" class="post-image">
