---
layout: post
title:  "Swift Optional"
date:   2019-07-29 18:36:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS Swift Optional
sitemap :
  changefreq : daily
  priority : 1.0
---

`Optional`은 해당 변수가 값이 있을 수도 있고 없을 수도 있다는 것을 나타내는 것을 의미한다.

그래서 몇 가지 규칙이 필요한데...

```Swift
var a = nil // (x)
// 일단 스위프트는 값이 들어가면 데이터 타입을 예측할 수 있는데
// nil은 데이터 타입을 알 수 없기 때문에 허용되지 않는다.

var a: Int = nil // (x)
// 무조건 변수에는 nil이 아닌 다른 초기값이 들어가야 한다.

var a: Int = 0 // (o)

var a: Int? // 이 경우 기본적으로 nil로 초기화 된다.
```

즉, 데이터를 다룰 때 nil이라는 값이 들어갈 것 같으면, 값이 중간에 없어지는 경우가 생길 것 같으면(?) `Optional`을 사용하면 된다.

그러면 이런 Optional 변수를 실제로 사용할 때는 어떻게 할까?

일단 기본적으로 **if**, **guard**, **!**, **??** 를 사용하는 방법이 있는 것 같다.

```Swift
var a: Int?

// Case 1
if let a = a {
  print(a)
}
else {
  print("a is nil")
}

// Case 2
guard let a = a else {
  print("a is nil")
  return
}

print(a)

// Case 3
a = 3
print(a!)

// Case 4

print(a ?? 3)
```

일반적으로 그냥 숫자나 문자열은 nil이 될 확률이 거....의 없다고 생각한다. 하지만 예를 들어 서버에서 데이터가 넘어온다고 가정해 보면... 데이터가 제대로 넘어올 수도 있고 안 넘어 올 수도 있다. 즉, 이런 경우는 `Optional`을 이용하면 될 것 같다. 그리고 웬만하면 초기 값을 설정할 수 있으면 최대한 하도록 노력하면 좋을 것 같다.
