---
layout: post
title:  "iOS sqlite3 락 문제(Sqlite3 is locked in iOS)."
date:   2018-10-22 23:16:00
author: Seungbeom Kim
categories: develop_diary
tags:	아이폰 iOS Database Sqlite3 Objective-c Lock
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)을 배포하고 받는 에러중에 `sqlite3LockAndPrepare` 같은 에러를 발견했다.

즉, 락이 걸린 것이다.

여기서 또 하나 배웠다. 나는 한번도 걸리지 않는 락이 다른 사용자에게서는 걸릴 수 있다는 것을...

Singleton 패턴을 이용해서 Database 작업을 하고 있기 때문에 아래와 같이 락을 걸기로 했다.

```Objective-c
.h 파일
//...

@interface ClassUpDbUtil : NSObject

@property (nonatomic, string) NSObject *lock;

@end

.m 파일

//...

@implementation ClassUpDbUtil

//...

- (id)init {
    if(self = [super init]) {
        self.lock = [[NSObject alloc] init];
    }
}

//...

- (void)readSomething {
  if(self.lock == nil) {
      self.lock = [[NSObject alloc] init];
  }

  @synchronized (self.lock) {
      // database 처리
  }
}

```

iOS 락에 관하여 조금 더 자세히 알고 싶으면 [링크](http://rykap.com/objective-c/2015/05/09/synchronized/)를 참조하면 된다.
