---
layout: post
title:  "iOS 프로토콜(iOS protocol)"
date:   2019-03-09 23:52:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS Objective-c Protocol 프로토콜
sitemap :
  changefreq : daily
  priority : 1.0
---

[이전 글](https://myksb1223.github.io/develop_diary/2019/03/09/iOS-delegate-protocol.html)에서 `Delegate`에 대해 다뤘으니 이번에는 `Protocol`의 사용을 조금 보려고 한다.

일단, `Protocol`을 사용하면 객체 모델링 및 데이터의 흐름(?)이 깔끔해 진다.

```Objective-c

// Custom.h

#import <UIKit/UIKit.h>

@protocol Custom

@property (nonatomic, strong) NSObject *object;

- (NSObject *)doSomething;

@end
```

이렇게 Custom이라는 `Protocol`을 만든 다음 아래와 같이 사용할 수 있다.

```Objective-c
// CustomThird.h

#import <UIKit/UIKit.h>

@interface CustomThird : NSObject <Custom>

@property (nonatomic, strong) NSObject *object;
// ...

@end

// CustomThird.m
#import "CustomThird.h"

@implementation CustomThird

- (NSObject *)doSomething {

}

@end
```
```Objective-c
// CustomSecond.h

#import <UIKit/UIKit.h>

@interface CustomSecond : NSObject <Custom>

@property (nonatomic, strong) NSObject *object;
// ...

@end

// CustomSecond.m
#import "CustomSecond.h"

@implementation CustomSecond

- (NSObject *)doSomething {

}

@end
```

이렇게 되면 CustomThird와 CustomSecond는 같은 Custom이라는 `Protocol`을 사용하는 클래스가 된다. 자 그러면 다른 곳에서 Custom `Protocol`을 사용하는 인스턴스에 대한 관리가 쉬워진다.

```Objective-c
// CustomFirst.h

#import <UIKit/UIKit.h>

@interface CustomFirst : NSObject

@end

// CustomFirst.m
#import "CustomFirst.h"

@implementation CustomFirst

- (void)doCustomProtocol {
  for (id<Custom> custom in customs) {
    [custom doSomething];
  }
}

@end
```

이렇게 되면 CustomFirst의 인스턴스는 CustomSecond와 CustomThird를 구별할 필요없이 Custom이라는 같은 `Protocal`이므로 위와 같이 사용할 수 있다.

또한 아래 처럼 다른 곳에서 내가 원하는 데이터에 대한 처리를 쉽게 위임할 수 있다.

```Objective-c
// UseProtocol.h

#import <UIKit/UIKit.h>

@interface UseProtocol : NSObject

@property (nonatomic, weak) id <Custom> customSource;

@end

// UseProtocol.m
#import "UseProtocol.h"

@implementation UseProtocol

- (void)doCustomProtocol:(NSObject *)obj {
  self.customSource.object = obj;
}

@end

// CustomSecond.h

#import <UIKit/UIKit.h>

@interface CustomSecond : NSObject <Custom>

@property (nonatomic, strong) NSObject *object;
// ...

@end

// CustomSecond.m
#import "CustomSecond.h"

@implementation CustomSecond

- (id)init {
  self = [super init];

  if(self) {
    UseProtocol *use = [[UseProtocol init] alloc];
    user.parentSource = self;
  }

  return self;
}

- (NSObject *)doSomething {

}

@end
```

위 처럼 UseProtocol의 인스턴스가 doCustomProtocol 메소드를 사용하면 CustomSecond의 object는 obj로 설정되는 이점이 있다.

마찬가지로 당연히 내 인스턴스 자체를 넘겨도 되지만 나는 object만 건드리고 싶은데 내 인스턴스 전부에 대한 접근을 시키는 것은 문제가 있을 수 있기에 위와 같이 처리해주는 것이 좋다고 판단된다.
