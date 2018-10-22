---
layout: post
title:  "NSURLSession 사용하는 방법(How to use NSURLSession)."
date:   2018-10-22 22:57:00
author: Seungbeom Kim
categories: develop_diary
tags:	아이폰 iOS NSURLSession Objective-c
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)을 iOS8이상 지원에서 iOS10 이상으로 지원을 바꾸었다. 그러자 warning이 엄청나게 뜬다... 전부 deprecated된 메소드에 대한 부분이었다.

일단 NSURLConnection을 NSURLSession으로 바꾸어야 한다.

일단 NSURLSession을 사용하게 되면 결과에 대한 처리를 main 쓰레드로 옮겨주어야 한다.

그래서 몇 가지 방법을 사용해 보았다.

```Objective-c
NSURLSession *session = [NSURLSessionConfiguration defaultSessionConfiguration] delegate:nil delegateQueue:[NSOperationQueue mainQueue]];

[[session dataTaskWithRequest:request completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
  // data 처리.
}] resume];
```

위의 코드와 같이 쓰면 쉽게 UI를 업데이트할 수 있다. 하지만 여기서 문제가 발생한다. 매 통신마다 session을 생성한다는 점이다. 이렇게 사용하니 무엇인가 속도가 통신속도도 느리고 반응도 엄청 느려진다. 그래서 고민하던 중 Singleton 패턴을 쓰기로 했다.

NSURLSession은 자체적으로 Singleton 패턴으로 디자인 된 객체를 지원한다. `[NSURLSession sharedSession]` 이다.

대신에 data처리 부분에서 UI관련 처리부분을 main 쓰레드로 옮겨주어야 한다.

```Objective-c
NSURLSession *session = [NSURLSession sharedSession];

[[session dataTaskWithRequest:request completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
    // data 처리.
    dispatch_async(dispatch_get_main_queue(), ^{
      //UI 처리.
    });
] resume];
```

이렇게 하면 NSURLSession 객체를 하나만 이용하기에 메모리에도 좋고 속도가 느려지는 모든 문제가 해결되었다.

혹시 커스텀할 필요가 있다면, 따로 Singleton 패턴을 이용하는 클래스를 생성하여 구현해주면 된다.
