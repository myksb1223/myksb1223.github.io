---
layout: post
title:  "리액트 네이티브(React Native) 사용기 23편 - Overlay 뷰"
date:   2019-03-27 00:15:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Instagram 인스타그램 오버레이뷰 OverlayView
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 [인스타그램(Instagram)을 구현하는 부분](https://myksb1223.github.io/reactnative/2019/03/24/React-Native-20.html)에서 Overlay뷰를 구현했는데, 이 Overlay에 대해서 조금 알아보려고 한다.

인스타그램(Instagram)의 프로필 창에서 Top Navigation에 있는 사용자 이름을 클릭하면 위에서 아래로 내려온다.(iOS 기준)

보통 뷰가 새롭게 보여질 때는, 연관된 다른 뷰는 무조건 움직인다. 하지만 위처럼 뷰에 뷰가 떠있는 구조를 위해서는, 즉 다른 뷰에 영향을 주지 않기 위해서는 조금 다른 스타일(Style)을 적용해 주어야 한다.

바로 `position: absolute`라는 옵션을 사용하는 것이다. 이는 절대위치를 잡겠다는 것이다. 즉, 다른 뷰와는 상관없이 다른 좌표에서 움직이겠다는 것이다.

`absolute`라는 속성을 부여한 후, left, top의 값을 0으로 주면 해당 뷰는 가장 위에 배치된다. 이렇게 하면 이 뷰는 다른 뷰에 영향을 주지 않고 작동한다.

```Javascript
// 다른 뷰들...

{% raw %}<View style={{flex: 1, flexDirection: 'column', position: 'absolute', left: 0, top: 0, backgroundColor: 'rgba(1, 0, 0, 0.6)', width: width, height: height}}>
// Overlay 뷰 내에 표현할 뷰 작성.
</View>{% endraw %}
```

그리고 참고로 부모 뷰의 투명도를 자식 뷰에 영향을 주지 않게 하기 위해서는 `opacity`가 아닌 `backgroundColor`의 `alpha`값을 조정해주어야 한다.

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
