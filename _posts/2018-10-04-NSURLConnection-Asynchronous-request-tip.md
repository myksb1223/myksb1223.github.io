---
layout: post
title:  "NSURLConnection Asynchronous request 팁(NSURLConnection Asynchronous request tip)."
date:   2018-10-04 01:06:00
author: Seungbeom Kim
categories: develop_diary
tags:	아이폰 iOS NSURLConnection sendAsynchronousRequest Objective-c
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 간단한 통신을 위해 `NSURLConnection`의 `sendAsynchronousRequest` 메소드를 사용했다.

이때, 아래처럼 Queue를 따로 만들어 주었는데... 생각보다 엄청 느렸다.

```Objective-c
NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url];
NSOperationQueue *queue = [[NSOperationQueue alloc] init];
[NSURLConnection sendAsynchronousRequest:urlRequest queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {
  if (error) {
  }
  else {
  }
}];
```

그래서 혹시나 해서 main queue를 사용해 보았더니 처리가 훨씬 빨랐다.

```Objective-c
NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url];
[NSURLConnection sendAsynchronousRequest:urlRequest queue:[NSOperationQueue mainQueue] completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {
  if (error) {
  }
  else {
  }
}];
```

혹시라도 빠른 처리를 원하면 Queue를 `[NSOperationQueue mainQueue]`와 같이 main queue를 사용하도록 하자!
