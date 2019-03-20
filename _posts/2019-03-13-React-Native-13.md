---
layout: post
title:  "리액트 네이티브(React Native) 사용기 13편 - ScrollView paging 하기"
date:   2019-03-13 04:29:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 ScrollView Paging
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 [이전 글](https://myksb1223.github.io/reactnative/2019/03/12/React-Native-12.html)에서는 `Picker`를 보완했으니 이제는 `ListView`에 넣은 이미지가 여러장일 때 `paging`할 수 있도록 할 것이다.

일단, `ScrollView`에 관해 전혀 모르니 [Document](https://docs.expo.io/versions/latest/react-native/scrollview/)를 참조한다.

속성에 보면 `horizontal`과 `pagingEnabled`이라고 있다.
각각을 일단 `true`로 만들어야 할 것 같다.

```Javascript
<ScrollView
  horizontal={true}
  pagingEnabled={true}
  showsHorizontalScrollIndicator={true} >
  // ...
</ScrollView>
```

이 코드를 원래 코드와 조합을 하면 아래와 같이 된다.

```Javascript
// ...

render() {
  // ...
  {% raw %}return (
    <ListView
      dataSource={this.state.dataSource}
      renderRow={(rowData, sectionID, rowID) =>
        <View style={{flex: 1, flexDirection: 'column', margin: 12}}>
          // ...
          <ScrollView
            style={{ flex: 1, marginBottom: 12}}
            horizontal={true}
            pagingEnabled={true}
            showsHorizontalScrollIndicator={true}

            >
              <Image source={{ uri: rowData.picture, cache: 'force-cache', }} style={{ flex: 1, width:  width-24, height: width-24}} />
              <Image source={{ uri: "https://www.instagram.com/p/BlGcOrlDr5W/media/?size=m", cache: 'force-cache', }} style={{ flex: 1, width:  width-24, height: width-24}} />
          </ScrollView>
          // ...
        </View>
      }
      enableEmptySections
    />
  );
}
{% endraw %}
//...
```

위와 같이 코드를 작성할 수 있다.
한가지 조심해야 할 부분이 `ScrollView`에서 크기를 잡는 것이 아닌 자식 뷰에서 크기를 잡아야 한다는 점이다.

리액트네이티브(ReactNative), Expo의 장점이라면 일단 이미지 캐싱에 대한 생각을 안해도 된다는 점과 저렇게 웹에서 이미지를 손쉽게 가져올 수 있다는 점인 것 같다.

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
