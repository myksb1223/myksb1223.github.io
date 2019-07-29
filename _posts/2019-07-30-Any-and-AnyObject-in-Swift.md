---
layout: post
title:  "Swift에서 Any와 AnyObject(Any and AnyObject in Swift)"
date:   2019-07-30 00:50:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS Swift Any AnyObject
sitemap :
  changefreq : daily
  priority : 1.0
---

`Any` : 말 그대로 어떤 타입이든 다 포함한다.
`AnyObject` : class 타입만 포함한다.

```Swift
var anyArr: [Any] = [0, "1"] // (o)
var anyObjArr: [AnyObject] = [0, "1"] // (x)
```

즉, **Value type**은 `AnyObject`로 캐스팅될 수 없다.

```Swift
class A {}
class B {}

var anyObjArr: [AnyObject] = [A(), B()] // (o)
```

특수한 상황이 아니고서는 대부분 `Any`를 사용하면 될 것 같다.
