---
layout: post
title:  "커스텀 link로 iOS 앱을 열기(Open iOS app from custom Link)."
date:   2018-10-16 15:06:00
author: Seungbeom Kim
categories: develop_diary
tags:	아이폰 iOS URLScheme Objective-c
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 수업을 쉽게 추가하기 위해서 웹의 URL을 이용하여 바로 수업을 추가하는 기능을 만들었다.

이 때, 유의해야 할 부분이 있다. 바로 아래의 글이다.

    AppWidget에서 앱으로 접속할 경우에도 Link를 쓰는데 혼용될 경우

이 것 때문에 하루종일 에러 때문에 엄청난 정신적인 고통을 받았다.
Crashlytics에서 빨간 경고가 뜰 정도였으니...

일단, Link를 이용할 경우 두가지 경우가 있다.
1. 앱이 백그라운드 상태인 경우.
2. 앱이 아예 종료된 경우.

1번의 경우인 경우에는 `- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation` 라는 메소드를 이용하여 url의 구조에 따라 분기하면 된다.

하지만 문제는 2번이다.

나는 2번의 경우을 잊고 있어서... sqlite3에 null 데이터가 들어가서 아예 앱이 종료되는 상황이 나왔다.

```Objective-c
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
  //...
  self.launchedURL = [launchOptions objectForKey:UIApplicationLaunchOptionsURLKey];
  //...
}

- (void)applicationDidBecomeActive:(UIApplication *)application {
  //...
  if(self.launchedURL != nil) {
    [self saveSubjectWithUrl:self.launchedURL];
    self.launchedURL = nil;
  }
  //...
}

- (void)saveSubjectWithUrl:(NSURL *)url {
  //Save subject to database.
}
```

위의 코드와 같이 Link를 따로 저장한 후, 앱이 Active 상태가 되면 저장된 Link를 사용하여 수업을 저장하는데...

이 때, 해당 `saveSubjectWithUrl:` 에서 url을 분석하여 분기시키지 않았던 것이다. 그래서 AppWidget에서 들어가는 경우 계속 아무 값도 없는 데이터가 저장되었던 것이다.

그래서 아래와 같이 수정해 주었다.

```Objective-c
- (void)saveSubjectWithUrl:(NSURL *)url {
  if(web_url) {
    // URL from Web.
  }
  else {
    // Other URL.
  }
}
```

이렇게 하고 나서 앱 켜질 때 NULL인 데이터를 모두 지워주었더니 복구되었다.

요즘은 아이폰의 앱 심사기간이 짧아서 그나마 다행이다...

이런 실수 다시는 하지 말자!
