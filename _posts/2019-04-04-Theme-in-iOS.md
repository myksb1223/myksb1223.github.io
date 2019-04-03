---
layout: post
title:  "Objective-c로 iOS 테마 건드리기(Theme in iOS with Objective-c)"
date:   2019-04-04 01:29:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS Objective-c 테마 Theme
sitemap :
  changefreq : daily
  priority : 1.0
---

[안드로이드 테마 건드리기 1편](https://myksb1223.github.io/develop_diary/2019/03/28/Theme-in-Android.html) 보기.
[안드로이드 테마 건드리기 2편](https://myksb1223.github.io/develop_diary/2019/03/28/Theme-in-Android-2.html) 보기.

이전에 안드로이드로 어두운 테마를 적용해 본 글을 썼는데 이번에는 iOS에 테마 적용하는 방법을 한번 써볼까 한다. 딱히, 쓸 것은 없고 [라이브러리](https://github.com/bradgmueller/CFATheme)를 찾았는데 해당 소스를 간략히 리뷰해보려고 한다.

일단, iOS는 안드로이드처럼 style과 같이 한꺼번에 적용할 수 있는 부분이 없기때문에 각 `ViewController`마다 적용시켜야 하는 단점이 있는 것 같다. 더 좋은 방법이 있을 수 있겠지만...

### 구조

- UIResponder+Theme
- CFAThemeManger

이렇게 크게 두 부분으로 나뉜다.

일단, `UIResponder+Theme`가 정말 획기적인 생각이라고 생각한다.

나는 사실 처음에 `UIViewController`를 상속받은 `BaseViewController`를 만든 후, 나의 모든 ViewController의 부모로 만들려고 했다. 하지만 정말 멍청한 짓이라는 것이 여기서 증명된 것 같다.

`UIViewController`는 `UIResponder`를 상속받기 때문에 UIResponder에 대한 `카테고리`만 만들면 되는 것이다. 이렇게 되면 어느 ViewController에서든 다 내가 카테고리에 선언해둔 메소드를 사용할 수 있다.

`CFAThemeManger` 부분은 테마가 변경되거나, 현재 테마의 상태를 체크하고 변화가 생길 시에, `ViewController`에 `Notification`을 보내주는 역할을 한다. 어느 화면이든 테마에 대한 상태는 하나면 되기때문에 `Singleton패턴`을 적용한 것을 알 수 있다.

이 `Notification`을 받은 `ViewController`는 `themeDidChange` 델리게이트 메소드를 이용하여 내가 원하는 배경색, 글자색 등을 적용시키면 된다.

솔직히 리뷰라고 했는데... 누구나 라이브러리 코드만 봐도 알 수 있는 내용이라 리뷰할 것이 별로 없었다. 가장 중요한 것은 `UIResponder`의 카테고리를 만드는 것인 것 같다.

### 요약

1. `CFAThemeManger`에서 테마 변화를 감지하고 관련 `Notification` 보냄.
2. 각 `ViewController`마다 테마 변화에 대한 `Notification`을 받을 수 있도록 등록.
3. `Notification`을 받은 각 `ViewController`는 델리게이트 메소드인 `themeDidChange`를 통해서 무엇을 하도록 할지 정하도록 함.

아예, 전체에 한번에 적용할 수 있는 방법이 있었으면 좋을 것 같다. 혹시 아시는 분 있으면 댓글이나 연락 부탁드립니다.
