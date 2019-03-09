---
layout: post
title:  "iOS delegate protocol(iOS delegate protocol)"
date:   2019-03-09 03:34:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS Objective-c Delegate Protocol
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 텍스트 뷰의 커스텀한 자동완성 형식을 구현하던 중 오랜만에 `Delegate`와 `DataSource`에 대한 고찰(?)아닌 고찰을 해보았다.

여기서는 `Delegate`에 대해서만 조금 설명해 보려고 한다.

일반적으로 `Delegate`는 아래와 같이 사용했다.

```Objective-c

// Custom.h

#import <UIKit/UIKit.h>

@protocol CustomDelegate <NSObject>

@required

// method
- (void)myRequiredDelegateMethod;

@optional

// method
- (void)myOptionalDelegateMethod;

@end

@interface Custom : NSObject

@property (nonatomic, weak) id<CustomDelegate> delegate;

@end

// Custom.m
#import "Custom.h"

@implementation Custom

- (void)doRequiredSomething {
  [self.delegate myRequiredDelegateMethod];
}

- (void)doOptionalSomething {
  [self.delegate myRequiredDelegateMethod];
}

@end

// UseCustomDelegate.m

@implementation UseCustomDelegate

- (void)myRequiredDelegateMethod {
  // Do something (for Custom)
}


- (void)myOptionalDelegateMethod {
  // Do something (for Custom)
}

@end
```

위와 같은 예는 보통 1:1 관계에서의 일이다. 즉, `ViewController`에 `UITableViewDelegate`를 선언할 때와 비슷한 경우이다.

하지만, 만약 하나의 Delegate가 여러 클래스가 유기적을 연동되어야 할 경우는 어떻게 해야할까?

즉, 한 인스턴스의 수행이 그 인스턴스를 지니고 있는 인스턴스, 그리고 그 지닌 인스턴스를 지닌 인스턴스에 순서적으로 영향을 주고 싶은 경우이다. 말로 표현하니 이상해 지는데 아래와 같이...

UITextView의 변화를 UITextView를 감싸고 있는 UIView에 그리고 그 UIView는 UIViewController에 영향을 주는 식이다.

이 경우에는 아래와 같이 표현할 수 있다..

```Objective-c
// CustomDelegate.h

#import <UIKit/UIKit.h>

@protocol CustomDelegate <NSObject>

@required

// method
- (void)myRequiredDelegateMethod;

@optional

// method
- (void)myOptionalDelegateMethod;

@end
```

먼저 `Delegate`를 만들고 수행되어야 하는 메소드들을 생성한다.

```Objective-c
// CustomThird.h

#import <UIKit/UIKit.h>

@interface CustomThird : NSObject

@property (nonatomic, weak) id<CustomDelegate> parent;

@end

// CustomThird.m
#import "CustomThird.h"

@implementation CustomThird

- (void)doRequireSomethinigForParent {
  // CustomSecond의 myRequiredDelegateMethod 호출
  [self.parent myRequiredDelegateMethod];
}

- (void)doOptionalSomethingForParent {
  // CustomSecond의 myOptionalDelegateMethod 호출
  [self.parent myRequiredDelegateMethod];
}

@end
```

CustomThird에서 수행된 함수에서는 CustomThird와 연결된 인스턴스인 CustomSecond의 Delegate메소드를 호출한다.

```Objective-c
// CustomSecond.h

#import <UIKit/UIKit.h>

@interface CustomSecond : NSObject <CustomDelegate>

@property (nonatomic, weak) id<CustomDelegate> parent;

@end

// CustomSecond.m
#import "CustomSecond.h"

@implementation CustomSecond

- (id)init {
  self = [super init];

  if(self) {
    CustomThird *third = [[CustomThird init] alloc];
    third.parent = self;
  }

  return self;
}

- (void)myRequiredDelegateMethod {
  // CustomFirst의 myOptionalDelegateMethod 호출
  [self.parent myRequiredDelegateMethod];
}

- (void)myRequiredDelegateMethod {
  // CustomFirst의 myOptionalDelegateMethod 호출
  [self.parent myRequiredDelegateMethod];
}

@end
```

이렇게 되면 CustomThird에서 CustomSecond의 Delegate메소드로 오게되고, 여기서 CustomSecond는 해야할 수행을 한 후에 다시 CustomFirst의 Delegate메소드를 호출한다.

```Objective-c
// CustomFirst.h

#import <UIKit/UIKit.h>

@interface CustomFirst : NSObject <CustomDelegate>

@end

// CustomFirst.m
#import "CustomFirst.h"

@implementation CustomFirst

- (id)init {
  self = [super init];

  if(self) {
    CustomSecond *second = [CustomSecond alloc] init];
    second.parent = self;
  }

  return self;
}

- (void)myRequiredDelegateMethod {
  // Do somthing.
}

- (void)myRequiredDelegateMethod {
  // Do somthing.
}

@end
```

이렇게 되면 CustomFirst의 인스턴스는 CustomThird에서의 변화를 감지할 수 있게된다.

당연히, 인스턴스 자체를 넘겨주는 것도 가능하지만 이 경우에는 memory관련 문제가 일어날 수 있으므로 최대한 메소드만으로 처리가 가능할 경우에는 `Delegate`를 이용하는 것이 좋은 것 같다.
