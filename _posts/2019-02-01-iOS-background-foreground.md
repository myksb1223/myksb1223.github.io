---
layout: post
title:  "iOS 백그라운드 포그라운드와 Notificaton observer.(iOS backgorund, foreground notification observer)."
date:   2019-02-01 16:21:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS error background foreground notification
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)을 개발하면서 Home으로 돌아갔다가 다시 돌아왔을 경우 현재 보여지는 ViewController를 다시 호출하고 싶은 경우가 있다.

예전에는 `viewWillAppear`라는 메소드가 바로바로 호출되었는데 어느 순간부터 `viewWillAppear`이나 `viewWillDisappear`과 같은 메소드가 호출되지 않았다.

이럴 경우에는 NSNotification을 이용하여 Notification을 등록하면 된다.

예를 들면, 다시 foreground 상태로 돌아올 때 `applicationDidBecomeActive` 라는 메소드가 `AppDelegate`에서 무조건 호출되기 때문에 아래와 같이 원하는 ViewController에서 등록 및 해제코드를 작성해 주면된다.

```Objective-c
- (void)registerNotification {
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(applicationDidBecomeActive:) name:UIApplicationDidBecomeActiveNotification object:nil];
}

- (void)unregisterNotification {
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(applicationDidBecomeActive:) name:UIApplicationDidBecomeActiveNotification object:nil];
}

- (void)applicationDidBecomeActive:(NSNotificationCenter *)notif {
    [self unregisterNotification];
    [self performSelector:@selector(viewWillAppear:) withObject:nil afterDelay:1];
}

- (void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];

    [self registerNotification];

    // ...
}

- (void)viewWillDisappear:(BOOL)animated {
    [super viewWillDisappear:animated];
    [self unregisterNotification];
}
```

이 때, 주의할 점이 있는데, 이 Notification의 등록들은 모두 중첩된다는 것이다. 즉, 다른 ViewController에서 동일한 이름의 Notification을 작성하면 작성한 모든 ViewController에 해당 액션이 호출된다.

즉, HomeButton을 눌렀다가 들어올 경우에는 앞에 말한 메소드들이 작용하지 않기 때문에, `applicationDidBecomeActive`의 호출이 들어오면 무조건 Notification을 먼저 해제해주는 코드를 사용하거나 `applicationWillResignActive` 관련 호출을 받는 코드를 작성한 후, 해제해주는 코드를 먼저 작성해 주면 된다.

`viewWillDisappear`은 다른 ViewController로 진입했을 경우 호출된다. 그러므로, `viewWillDisappear`에도 반드시 Notification을 해제해주는 코드를 써주어야 한다.
