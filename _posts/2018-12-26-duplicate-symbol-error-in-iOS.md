---
layout: post
title:  "Xcode duplicate symbol 에러.(duplicate symbol error in iOS)."
date:   2018-12-26 17:53:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS Xcode error
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)을 개발하다가 아래와 같은 에러를 마주했다.

    duplicate symbol _direction in:
    .../Objects-normal/arm64/AViewController.o
    .../Objects-normal/arm64/BViewController.o
    ld: 1 duplicate symbol for architecture arm64

즉, 1개의 중복된 symbol이 있다는 것이다.

그래서 각각의 A, B ViewController를 조사해 보았지만 그 어디에도 잘못된 곳을 찾을 수 없었다.

.h, .m 파일 모두 제대로 작성되어 있었다. StackOverflow에도 검색해 보았지만 나와 해당부분은 없었다.

위와 같은 에러는 보통 Library들 사이에서 충돌이 생겨 나타나지만 클래스업(ClassUp)에서는 해당 충돌에 관한 제어는 모두 했기에 다른 문제로 판단하였다.

그렇게 코드를 하나씩 테스트해 본 결과 아래와 같은 경우 에러가 발생한다.

```Objective-C

// AViewController.m

@interface AViewController ()

@end

@implementation AViewController

// ...

int a = -1;

// ...

@end

// BViewController.m

@interface AViewController ()

@end

@implementation AViewController

// ...

int a = -1;

// ...

@end

```

즉, 위와 같이 저렇게 코드와 코드 사이에 변수를 같은 이름으로 선언해 준 경우에 발생하는 것이었다.
