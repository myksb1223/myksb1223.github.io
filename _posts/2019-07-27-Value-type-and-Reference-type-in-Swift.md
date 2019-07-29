---
layout: post
title:  "스위프트 Value type과 Reference type(Value type and Reference type in Swift)"
date:   2019-07-27 19:36:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS Swift ValueType ReferenceType
sitemap :
  changefreq : daily
  priority : 1.0
---

어느 언어든지 값의 복사, 값의 참조 이 두 경우가 항상 중요하다. Swift 공부를 시작하면서 해당 언어에서 사용하는 방식을 조금 정리해 보려고 한다.

`Structure`와 `Emumeration`은 **Value type**이다. Value type이라는 것은 함수의 매개변수로 값을 보낼 때나 값을 할당할 때,  **copy** 속성으로 값을 처리한다는 것이다.

```Swift
  Struct Resolution {
    var width = 0
    var height = 0
  }

  let hd = Resolution(width: 1920, height: 1080)
  var cinema = hd
  cinema.width = 2048
```

위와 같은 경우 cinema와 hd는 완전히 다른 instance라는 뜻이다. 즉, cinema의 width만 2048로 변경된다.

```Swift
enum CompassPoint {
  case north, south, east, west
  mutating func turnNorth() {
    self = .north
  }
}

var currentDirection = CompassPoint.west
let rememberedDirection = currentDirection
currentDirection.turnNorth()
```

위와 같은 경우에는 초기에는 currentDirection과 rememberedDirection 모두가 west지만 turnNorth()의 호출 후에는 currentDirection만 north로 바뀐다.

`Int`, `Float`, `Bool`, `String`, `Array`, `Dictionary`는 Value type이다.

`Class`로 생성된 instance는 **Reference type**이다.

```Swift
class VideoMode {
  var Resolution = Resolution()
  var interlaced = false
  var frameRate = 0.0
  var name: String?
}

let tenEighty = ViewMode()
tenEighty.resolution = hd
tenEighty.interlaced = Structure
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0

let alsoTenEighty = tenEighty
alsoTenEighty.frameRate = 30.0
```

위 경우 tenEighty와 alsoTenEighty의 frameRate 모두가 30으로 변경된다. 왜냐하면 같은 주소값을 참조하고 있기 때문이다.

tenEighty, alsoTenEighty 모두 contant로 선언되었지만 내부 변수는 변경할 수 있다. 당연하겠지만 tenEighty, alsoTenEighty가 가리키는 주소를 변경할 수 없는 것이지 내부 변수들은 자유롭게 변경할 수 있다.
