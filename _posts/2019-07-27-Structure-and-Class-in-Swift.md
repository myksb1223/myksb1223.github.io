---
layout: post
title:  "스위프트의 Structure와 Class(Structure and Class in Swift)"
date:   2019-07-23 18:36:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS Objective-c Swift Weak
sitemap :
  changefreq : daily
  priority : 1.0
---

Objective-c에서 Swift로 넘어오려다 보니 `Structure`라는 것이 보인다. 내가 일반적으로 생각하는 구조체와는 다른다.

### Structure와 Class 비교

##### 공통점

- 값을 저장하기 위한 속성 정의
- 기능 제공을 위한 메소드 정의
- subscript 구문을 이용한 접근을 위한 subscript 정의
- 기본 구현을 넘어 기능 확장을 위한 확장
- 특정 형태의 기본 기능 제공을 위한 protocol을 따름

#### Class만 가진 특징

- 상속
- Type Casting을 이용한 런타임시 instance의 타입을 해석하고 확인할 수 있게 함 (Cast 가능하다라는 뜻인 것 같다.)
- Deinitializer는 instance이 할당된 리소스든 해제할 수 있게 함(deinit()을 이용하여 해제할 수 있다.)
- Reference counting은 instance의 하나 이상의 참조를 가능하게 함(값의 복사가 아닌 주소값을 공유한다는 뜻이다.)

#### 언제 Structure를, 언제 Class를 사용해야 할까?

[Apple 공식 문서](https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes)에서는 아래와 같이 나타내고 있다.

- 기본적으로 Structure를 사용
- Objective-c와 상호처리를 해야하면 Class를 사용
- 모델링한 데이터의 identity를 제어해야 하는 경우 Class 사용
- Protocol과 함께 사용하여 일단 Stucuture를 사용해서 구현

#### Choose Structures by Default
Structure는 Class처럼 변수 정의, 값의 계산, 메소드 정의도 할 수 있고, protocol을 채택할 수 있다. 또한 Value type 속성을 가지기 때문에 상태나 흐름 체크가 쉽다. 값이 무조건 복사되기 때문에 원본 데이터의 변화에 신경쓸 필요가 없다는 뜻으로 생각하면 될 것 같다. 왜냐하면 항상 우리가 메소드의 매개변수로 instance를 넘기면 원본 데이터의 변화가 어떻게 이루어지는지 생각하면서 코딩해야하기 때문이다.

#### Use Classes When You Need Objective-C Interoperability
Objective-C에서도 사용되어야 한다면 그냥 class를 사용하라는 말 같다. 이 부분은 조금 더 공부가 필요할 것 같다.

#### Use Classes When You Need to Control identity
데이터의 변경이나 상태의 변경이 전체 흐름에 영향을 주어야 하는 경우네는 class를 사용하라는 것 같다.

#### Use Structures When You Don't Control identity
즉, 현재 instance가 유효한지 검증할 필요가 없을 경우 사용하라는 뜻이다. 예를 들어 앱 화면에 ScrollView의 형태가 두개 이상 있다고 가정하면, 현재 내가 스크롤한 뷰가 무엇인지 정확히 체크를 해야한다. 이 경우에는 Class를 사용해야하지만 그런 경우가 아니라면 Structure를 사용해도 된다는 뜻이다. 예는 무척 많지만 막상 생각이 안난다.

#### Use Structures and Protocols to Model Inheritance and Share Behavior
Class의 상속으로 구현할 수 있는 모델링은 Structure, Protocol로 충분히 할 수 있다. 그러니까 일단 Structure와 Protocol을 사용해서 모델링을 시도하자.
