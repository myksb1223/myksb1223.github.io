---
layout: post
title:  "iOS에서 ScrollView와 Gesture 사용(ScrollView with Gesture in iOS)."
date:   2018-11-29 02:18:00
author: Seungbeom Kim
categories: develop_diary
tags:	아이폰 iOS ScrollView Gesture
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)을 최근에 달력처럼 무한 스크롤 방식을 도입하면서 슬라이드 메뉴와 스크롤 액션이 겹치는 상황이 발생했다. 그래서 스크롤만 되고, 아예 슬라이드 메뉴가 나오지 않았다. 라이브러리를 사용하다 보니 Gesture가 중복된 것이다.

그래서 찾아본 결과 UIGestureRecognizerDelegate를 사용하여 이를 쉽게 해결할 수 있었다.

CocoaPods를 이용했기에 슬라이드 메뉴 라이브러리의 파일을 Unlock을 시킨 후, 아래와 같이 코드를 추가했다.

```Objective-c
.m 파일
//...

- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer
{
//    NSLog(@"gestrue : %@", gestureRecognizer);
//    NSLog(@"other : %@", otherGestureRecognizer);

    if(![otherGestureRecognizer isEqual:self.sideMenuController.panGesture]) {
        [otherGestureRecognizer setState:UIGestureRecognizerStateCancelled];
    }
    return YES;
}

//...

```

즉, 현재 슬라이드 제스쳐가 수행될 경우 중복으로 수행되는 다른 제스쳐를 Cancel시켜 주는 것이다.
