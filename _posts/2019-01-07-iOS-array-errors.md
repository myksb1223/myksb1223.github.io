---
layout: post
title:  "iOS에서 배열의 범위를 넘어갔을 경우에 생길 수 있는 에러들.(iOS errors when array indexes out of bounds)."
date:   2019-01-07 22:24:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS error 배열 Array
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)을 개발하다가 정말 오래되었지만 고치지 못한 고질적 에러가 한개 있었다.

바로...

    EXC_BAD_ACCESS KERN_INVALID_ADDRESS 0x11bc61a36dc800d3

그냥 메모리가 해제된 부분에 접근해서 생기는 부분인데... 도대체 이해가 되지 않았다. 왜냐하면 Crashlytics에서 알려주는 부분은 전혀 다른 부분이었기 때문이었다. 예를 들면 아예 함수정의 부분이라던가...

그래서 이번에 해결하게 되었는데, 위와 같은 에러는 배열의 범위를 벗어났을 때도 생길 수 있다.

```Objective-C
NSString *arr[3] = {"a", "b", "c"};
[self function:arr[3]];

- (void)function:(NSString *)str {
  // ...
}
```

위와 같은 경우에 바로 에러를 떨어뜨릴 수 있지만 아닐 수도 있다. 즉, 무슨 말이냐 하면 주소이기 때문에 arr[3]의 주소에 정말로 값이 없었다면 위와 같은 에러를 떨어뜨릴 수 있지만, 아닌 경우에는 그렇지 않다는 것이다.

```Objective-C
NSString *arr[3] = {"a.jpg", "b.jpg", "c.jpg"};
UIImage image = [UIImage imageNamed:arr[3]];
```

위와 같은 코드가 있다고 할 때, 혹시 arr[3]에 어떤 값이 있었다면 아래와 같이 다른 에러를 떨어 뜨릴 수 있다.

    -[__NSCFNumber stringByDeletingPathExtension]: unrecognized selector sent to instance 0xb861f569b89300d1

즉, imageNamed를 수행할 때, 넘어간 arr[3]의 어떤 값이 `stringByDeletingPathExtension`를 수행할 수 없다는 것이다.

또 다른 예제를 보자.

<figure>
<img src="{{ site.baseurl }}/assets/develop_diary/array_out_of_index_1.PNG" title="Out of index 1" class="post-image-double"><img src="{{ site.baseurl }}/assets/develop_diary/array_out_of_index_2.PNG" title="Out of index 2" class="post-image-double">
<figcaption style="text-align: center;">왼쪽이 정상이고 오른쪽이 범위가 넘어간 경우</figcaption>
</figure>

위와 같이 실제로 꺼지지 않는 경우도 있다. 즉, 타입이 잘 맞을 경우. 위 그림에서 배열에서 가져와서 처리할 데이터타입이 `NSString`인데 넘어간 값 역시 `NSString`일 경우이다.

혹시 배열을 사용하면서 무슨 에러인지 모를 경우에는 배열 범위를 넘어가는 경우가 있는지 반드시 체크해주는 것이 좋다.
