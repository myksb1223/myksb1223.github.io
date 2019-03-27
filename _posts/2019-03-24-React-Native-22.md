---
layout: post
title:  "리액트 네이티브(React Native) 사용기 22편 - TouchableOpacity, TouchableWithoutFeedback, TouchableNativeFeedback style"
date:   2019-03-24 04:38:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Instagram 인스타그램 TouchableOpacity TouchableWithoutFeedback TouchableNativeFeedback Style
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 [인스타그램(Instagram)을 구현하는 부분](https://myksb1223.github.io/reactnative/2019/03/24/React-Native-20.html)에서 Overlay뷰의 빈 부분을 클릭했을 때의 동작을 위해서 `TouchableWithoutFeedback`을 사용하려고 했다.

하지만... style이 아무것도 먹지 않는 것이었다... 엄청나게 찾아보았으나... style에 대한 Props가 없다는 것을 발견 하였다.

여기는 [TouchableOpacity](https://facebook.github.io/react-native/docs/touchableopacity)의 도큐먼트이고 여기는 [TouchableWithoutFeedback](
https://facebook.github.io/react-native/docs/touchablewithoutfeedback#props)의 도큐먼트이다.

자세히 보면... props부분에 style부분이 빠져있는 것을 알 수 있다.

항상 도큐먼트를 잘 읽으면서 사용하도록 해야겠다... 이것 때문에 시간 엄청 빼앗긴 것 같다.

마찬가지로, [TouchableNativeFeedback](https://facebook.github.io/react-native/docs/touchablenativefeedback#props) 역시 style에 대한 props가 없으니 주의하길 바란다!

다음에도 리액트 네이티브(React Native), Expo Instagram 프로젝트에서 많이 고민했던 부분을 쓸 것이다. 주제는 아마 `Overlay 뷰`에 관한 것일 것 같다.

[Overlay 뷰](https://myksb1223.github.io/reactnative/2019/03/27/React-Native-23.html) 보기.

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
