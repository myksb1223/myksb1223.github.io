---
layout: post
title:  "NSURLSession 조금 더 고찰(NSURLSession consideration)."
date:   2018-10-22 22:57:00
author: Seungbeom Kim
categories: develop_diary
tags:	아이폰 iOS NSURLSession Objective-c
sitemap :
  changefreq : daily
  priority : 1.0
---

이전에 `NSURLSession`의 [사용방법](https://myksb1223.github.io/develop_diary/2018/10/22/How-to-use-NSURLSession.html)에 대해서 올렸었는데 오늘은 조금 더 알아보았다.

클래스업(ClassUp)에서 `NSURLSession`을 사용중에 새로운 화면에 들어가면 아래 로그를 띄우며 계속 `NSURLSession`이 취소되었다.

    Error Domain=NSURLErrorDomain Code=-999 "cancelled"

즉, 위 로그처럼 -999는 `NSURLSession`에서 Task가 cancel되엇을 때 뜨는 코드이다.

일단 맨 처음에 생각한 것은 한번에 너무 많은 Connection을 처리하는 것이 아닌가에 대한 생각이었다.

그래서 찾아본 결과 `NSURLSessionConfiguration`의 `HTTPMaximumConnectionsPerHost`의 값을 늘려주라고 되어 있었다.

이를 위해서는 현재 사용하는 방식을 조금 바꿔야 했다.

현재는 `[NSURLSession sharedSession]`인 기본 singleton 객체를 사용하고 있는데 이는 Configuration을 변경할 수 없다.

그래서 아래와 같이 바꿔보았다.

1. NetworkManager 라고하는 클래스를 생성한다.
2. Singleton 객체화 한 후, NSURLSession 객체를 하나만 생성하도록 한다.
3. `NSURLSessionConfiguration`은 기본값을 사용하고 `HTTPMaximumConnectionsPerHost`의 값만 변경 시킨다.

```Objective-c
  NSOperationQueue *queue = [[[NSOperationQueue alloc] init] autorelease];
  queue.name = @"Network Queue";
  queue.maxConcurrentOperationCount = 5;

  NSURLSessionConfiguration *config = [NSURLSessionConfiguration defaultSessionConfiguration];
  config.HTTPMaximumConnectionsPerHost = 10;
  self.session = [NSURLSession sessionWithConfiguration:config delegate:nil delegateQueue:queue];
```

간략히 구현하면 위와 같다.

그러면 `[NetworkManger sharedManager].session`과 같은 형식으로 사용하면 된다.

`finishTasksAndInvalidate`나 `invalidateAndCancel`은 세션값을 아예 초기화시키기 때문에 singleton 객체로 유지할 경우에 사용하면 앱이 꺼진다.

즉, 위의 메소드를 사용하려면 `NSURLSession`을 다시 구성해 주어야 한다.

`[NSURLSession sharedSession]`의 기본 `HTTPMaximumConnectionsPerHost`값은 4이다.

하지만, 해결되지 않았다. 아무리 생각해도 클래스업의 동시에 연결이 아무리 많아도 3개라고 생각되었기에...

조금 더 찾아본 결과 `NSURLSession`은 같은 URL을 가진 녀석이 중복해서 들어올 경우에는 가장 나중에 들어온 녀석만 수행하고 자체적으로 앞의 처리를 취소한다고 한다.

즉, 자체 큐(Queue)를 가지고 있어 알아서 처리한다는 것이다. 즉, 새롭게 `NSURLSession`객체를 만들 필요가 없는 것이다.

결국 원인은 예전에 `NSURLConnection`을 사용할 때, 직접 큐를 만들어 관리했었는데, 해당 부분의 Cancel 처리부분이 `NSURLSession`의 모든 Task를 취소하고 있었던 것이었다...

즉, `[NSURLSession sharedSession]`객체는

1. 전체 앱의 Session을 관리한다.
2. 자체적으로 큐를 가지고 있고, 해당 큐에 들어온 Task를 처리하는데, 중복된 URL을 가진 Task가 들어오면 먼저 들어온 앞서 들어온 Task를 취소한다.
3. `HTTPMaximumConnectionsPerHost`는 한 호스트에 동시에 연결할 수 있는 숫자를 나타낸다. 즉, 같은 Host에 10개의 Task를 요청하면 4개까지 동시에 연결되고 나머지 6개는 큐에서 기다린다.
