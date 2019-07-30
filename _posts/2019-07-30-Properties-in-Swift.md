---
layout: post
title:  "Property 알아보기(Properties in Swift)"
date:   2019-07-30 22:30:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS Swift Property Properties
sitemap :
  changefreq : daily
  priority : 1.0
---

`Property`는 **class**, **struct**, **enmeration**에서 사용할 수 있다. 크게 `Stored property`와 `Computed property`로 구분할 수 있는데 Stored property는 class, struct에서만 사용할 수 있다.

추가적으로 `Type property`가 있다.

`Property observer`라는 것도 있는데 말 그대로 Property의 변화를 체크하는 기능이다.

### Stored properties

```Swift
struct FixedLengthRange {
  var firstValue: Int // = 0 등으로 바로 초기화도 가능하다.
  let length: Int // = 3 등으로 바로 초기화도 가능하다.
}

var rangeOfThreeItems = FixedLengthRange(firstValue: 0, Length: 3)
rangeOfThreeItems.firstValue = 6
rangeOfThreeItems.length = 5 // (x) let으로 선언되었음
```

#### Stored Properties of Constant Structure Instances

```Swift
let rangeOfThreeItems = FixedLengthRange(firstValue: 0, Length: 3)
rangeOfThreeItems.firstValue = 6 // (x) rangeOfThreeItems이 let으로 선언됨
```

만약 FixedLengthRange가 **class**였다면 `let`으로 선언하더라도 내부 변수는 변경할 수 있다. class는 `Reference type`이기 때문이다.

#### Lazy Stored Properties

말 그대로 초기화를 늦게 하겠다는 것이다. 즉, 값이 접근되는 순간에 초기화한다는 뜻이다.

자세한 자료는 [여기](https://alisoftware.github.io/swift/2016/02/28/being-lazy/?utm_campaign=This%2BWeek%2Bin%2BSwift&utm_medium=email&utm_source=This_Week_in_Swift_76)를 참고하면 된다.

애플 문서에서는 `var`은 항상 `lazy`로 선언하라고 하고 있다.

lazy를 사용하는 이유는 변수에 들어가는 값이 복잡하거나 계산의 비용이 많이 들어가는 경우에 사용하라고 한다. 당연히 초기화과정에서 시간이 오래 소모되면 프로그램 성능에 큰 영향을 미칠 것이다.

### Computed properties

```Swift
struct Point {
    var x = 0.0, y = 0.0
}
struct Size {
    var width = 0.0, height = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set(newCenter) {
            origin.x = newCenter.x - (size.width / 2)
            origin.y = newCenter.y - (size.height / 2)
        }
    }
}
var square = Rect(origin: Point(x: 0.0, y: 0.0), size: Size(width: 10.0, height: 10. 0))
let initialSquareCenter = square.center
print("square.origin is now at (\(square.origin.x), \(square.origin.y))")
// Prints "square.origin is now at (10.0, 10.0)"
```

#### Shorthand Setter Declaration

위의 것을 아래처럼 짧게 사용할 수 있다.

```Swift
struct AlternativeRect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set {
            origin.x = newValue.x - (size.width / 2)
            origin.y = newValue.y - (size.height / 2)
        }
    }
}
```

기본적으로 `set`에 아무 매개변수도 적지 않으면  **newValue**라는 값으로 사용할 수 있다.

#### Shorthand Getter Declaration

`get`도 아래처럼 줄일 수 있다. expression이 한줄이면 return값이 필요없다. ([여기 참고](https://docs.swift.org/swift-book/LanguageGuide/Functions.html#ID607))

```Swift
struct CompactRect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            Point(x: origin.x + (size.width / 2),
                  y: origin.y + (size.height / 2))
        }
        set {
            origin.x = newValue.x - (size.width / 2)
            origin.y = newValue.y - (size.height / 2)
        }
    }
}
```

#### Read-Only Computed Properties

아래 처럼 `get` 키워드와 `{}`를 없애면 된다. 주의할 점은 항상 `var`로 변수를 선언해야 한다는 것이다.

```Swift
struct Cuboid {
    var width = 0.0, height = 0.0, depth = 0.0
    var volume: Double {
        return width * height * depth
    }
}
let fourByFiveByTwo = Cuboid(width: 4.0, height: 5.0, depth: 2.0)
print("the volume of fourByFiveByTwo is \(fourByFiveByTwo.volume)")
// Prints "the volume of fourByFiveByTwo is 40.0"
```

### Property Observers

값의 입력을 모니터링 하여 따로 수행할 수 있는 작업을 할 수 있다.

`lazy`로 선언된 property이외에 모든 stored property는 `property observer`를 추가할 수 있다. property observer는 상속할 수 있다.

`computed property`는 `get`, `set`을 이용하여 값의 변화를 체크할 수 있기 때문에 property observer를 정의할 필요가 없다.

`willSet` : 값이 저장되기 전에 호출된다. 기본적으로 `newValue`라는 이름으로 사용할 수 있다.
`didSet` : 새로운 값이 저장된 후 즉시 호출된다. 변경 전의 값은 `oldValue`라는 이름으로 사용할 수 있다.

`in-out parameter`에서 역시 didset, willset은 항상 호출된다. `in-out parameter`를 호출한 함수가 반환될 때 항상 해당 값을 갱신하기 때문이다.

실제 사용해본 코드를 살펴보면...
클래스업(ClassUp)에는 시간표 배경화면의 이미지를 설정하는 ViewController가 2개 존재한다. 두 Controller는 조금 역할이 다르지만 공통적으로 이미지를 사용한다. 이 이미지의 값이 변경될 때 마다 실제 배경에 적용하고 `JPEG`, `GIF`에 따라 type이 변해야 한다.

```Swift
class TimetableBackgroundViewController: UIViewController {
    @IBOutlet weak var backView: FLAnimatedImageView! // 배경

    var backgroundExtensionType = 0
    var image: UIImage? {
        willSet {
            // To do something
        }
        didSet {
            self.backgroundExtensionType = 0
            self.backView.image = self.image
        }
    }

    var flaImage: FLAnimatedImage? {
        willSet {
            // To do something
        }
        didSet {
            self.backgroundExtensionType = 1
            self.backView.animatedImage = self.flaImage
        }
    }
}

// 존재하는 배경 수정할 경우
class PreviewSettingViewController: TimetableBackgroundViewController {
    override var image: UIImage? {
        willSet {
            // To do something
        }
        didSet {
            // 기존에 있던 배경 이미지를 처리하기
        }
    }
}

// 최초 배경 만들 경우
class SelectBackgroundViewController: TimetableBackgroundViewController {
    override var image: UIImage? {
        willSet {
            // To do something
        }
        didSet {
            // 최초 배경 만들 때 필요한 작업하기
        }
    }
}
```

클래스업(ClassUp)에 대해 자세히 설명하면 너무 길어지기에... 간략히 말하면 위와 같이 활용할 수 있다.

    didSet 내부에서의 해당 값에 대한 변경은 다시 didSet을 호출하지 않는다.

### Global and Local Variables

전역 변수, 지역 변수 모두 observer와 computed variable을 정의할 수 있다.

전역 변수에는 자체적으로 호출되어야지만 값을 할당하기 때문에 lazy를 사용할 필요가 없다.
지역 변수는 `lazy`를 사용할 수 없다.

### Type Properties

간단하게 `static` 변수이다.

`initializer`을 가지지 않기 때문에 항상 기본 값을 할당해 주어야 한다.

자체적으로 `lazy` 속성을 가지기 때문에 처음 호출되는 경우에 할당된다.

```Swift
struct SomeStructure {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 1
    }
    static var computedTypeReadWriteProperty: Int {
        get {
            return 1
        }
        set {
            newValue + 1
        }
    }
}
enum SomeEnumeration {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 6
    }
}
class SomeClass {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 27
    }
    class var overrideableComputedTypeProperty: Int {
        return 107
    }
}
```

위에서 보는 것과 같이 `class` 키워든는 자식 클래스에서 override 할 수 있다.
