---
layout: post
title:  "리액트 네이티브(React Native) 사용기 21편 - ListView 원하는 Row로 Scroll하기"
date:   2019-03-24 04:24:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Instagram 인스타그램 ListView 리스트뷰 scrollTo scrollToRow
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 인스타그램(Instagram)을 구현하는 부분에서 `ListView`에서 원하는 Row로 스크롤해야할 필요가 있었고, 해당 부분에 대한 글을 써본다.

일단, `ListView`에는 `scrollToRow`와 같은 기능이 없고 `scrollTo`를 사용해야 한다. 즉, x, y의 위치를 알아야지만 가능하다.

즉, Row의 x, y좌표를 알아내야한다는 것이다. 다행히, View에는 `onLayout`이라는 옵션이 있는데 이를 이용하면 해당 View의 위치와 크기를 알아낼 수 있다.

이를 이용하면 쉽게 scroll시킬 수 있다.

```Javascript
constructor(props) {
  super(props);

  this.rowOffsets = {}
}

onLayout(event, rowID) {
  // event.nativeEvent.layout.y가 y의 좌표값이다.
  this.rowOffsets[rowID] = y;
}

render() {
  // ...
  return (
    <View>
      <ListView
        dataSource={this.state.dataSource}
        renderRow={(rowData, sectionID, rowID) =>
          <View
            onLayout={(event) => this.onLayout(event, rowID)}>
          </View
          }
        enableEmptySections
      />
      // ...
    </View>
  );
}
```

위와 같이하면, 각 Row별로 y값을 저장할 수 있다. 이제는 `scrollToRow`라는 함수를 만들고, `ListView`를 스크롤 시키면 된다.

```Javascript
// ...

scrollToRow = (rowID) => {
  if (this.listView && this.rowOffsets[rowID]) {
    this.listView.getScrollResponder().scrollTo(scrollOffset({x: 0, y: this.rowOffsets[rowID], animated: true})
  }
}

render() {
  return (
    // ...
    <View>
      <ListView
        ref={component => this.listView = component}
        // ...
      />
      // ...
    </View>
  );
}
```

위와 같이 `ListView`에 ref를 설정하고 `scrollToRow` 메소드를 만들면 된다. 이제 `Button`이나 `TouchableOpacity`등을 이용하여 `onPress`시 `scrollToRow`를 호출하면 잘 움직이는 것을 볼 수 있다.

다음에도 리액트 네이티브(React Native), Expo Instagram 프로젝트에서 많이 고민했던 부분을 쓸 것이다.

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
