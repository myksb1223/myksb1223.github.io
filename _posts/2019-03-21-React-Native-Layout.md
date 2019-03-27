---
layout: post
title:  "리액트 네이티브(React Native) 사용기 레이아웃 편"
date:   2019-03-21 05:06:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo 레이아웃 Layout
sitemap :
  changefreq : daily
  priority : 1.0
---

최근 업데이트 : 3, 4, 5, 6 : 2019-03-28 02:57:00.

리액트 네이티브(React Native), Expo로 구현하다가 레이아웃 관련 부분에서 잊지말아야 하는 것들(?)을 꾸준히 생길 때마다 업데이트 하면서 기록하려고 한다.

1. `flex`와 `flexDirection` : 그냥 뷰의 비율과 배치 방향이라고 생각하면 쉬운 것 같다.

    ```HTML
    {% raw %}<View style={{flex: 1, flexDirection: 'column'}}>
      <View style={{flex: 1, backgroundColor: '#0000ff'}} />
      <View style={{flex: 4, backgroundColor: 'skyblue'}} />
    </View>{% endraw %}
    ```

    세로로 1:4의 비율로 뷰가 세로(column)로 배치된다. `column`을 `row`로 변경하면 가로로 1:4의 비율로 뷰가 배치된다.

2. `justifyContent`와 `alignItems` : center만 써 보았지만 일단, flexDirection에 영향을 받는다. (추후 사용할 때 마다 기록을 남길 예정이다.)

    ```HTML
    <View style={% raw %}{{flex:1, flexDirection: 'row', backgroundColor: 'steelblue', justifyContent: 'center', alignItems: 'center'}}{% endraw %}>
      <Image source={% raw %}{{ uri: uri }} style={{width:70, height:70, borderRadius: 35}}{% endraw %}/>
    </View>
    <View style={% raw %}{{flex:3, backgroundColor: 'red'}}{% endraw %} />
    ```

    위 부분에서 중요한 부분은 아래부분이다.

    ```HTML
    // 가운데 정렬
    <View style={% raw %}{{flex:1, flexDirection: 'row', backgroundColor: 'steelblue', justifyContent: 'center', alignItems: 'center'}}{% endraw %}>

    <View style={% raw %}{{flex:1, flexDirection: 'column', backgroundColor: 'steelblue', justifyContent: 'center', alignItems: 'center'}}{% endraw %}>

    // 수평 정렬
    <View style={% raw %}{{flex:1, flexDirection: 'row', backgroundColor: 'steelblue', justifyContent: 'center'}}{% endraw %}>

    <View style={% raw %}{{flex:1, flexDirection: 'column', backgroundColor: 'steelblue', alignItems: 'center'}}{% endraw %}>

    // 수직 정렬
    <View style={% raw %}{{flex:1, flexDirection: 'column', backgroundColor: 'steelblue', justifyContent: 'center'}}{% endraw %}>

    <View style={% raw %}{{flex:1, flexDirection: 'row', backgroundColor: 'steelblue', alignItems: 'center'}}{% endraw %}>
    ```

3. `flex`와 `position: 'absolute'`이 함께 사용된 경우
    ```html
    <View style={% raw %}{{flex:1, position: 'absolute', bottom: 0, left:0, right: 0}}{% endraw %}>
      <!-- 내부 뷰 구현 -->
    </View>
    ```
    위와 같은 뷰가 있다고 한다면 `flex`가 1이기 때문에 해당 비율만큼 잡혀서 높이가 계산될 것으로 예상했으나, 내부 뷰의 크기만큼만 높이가 계산되었다.
4. `flex`와 `position: relative`이 함께 사용된 경우
    3의 경우에서 `position`만 `relative`로 변경하였다. 이 경우에는 `flex`의 값이 적용이 된다.
5. `position: absolute`이면서 `left`, `right` 옵션이 적용되지 않은 경우.
    ```html
    <View style={% raw %}{{position: 'absolute', bottom: 0,}}{% endraw %}>
      <!-- 내부 뷰 구현 -->
    </View>
    ```
    이 경우 내부 뷰가 없다면 `width`가 0이지만, 내부 뷰가 있다면 화면의 가로크기만큼 `width`가 잡힌다.
6. 상위 뷰에 `flex`가 설정된 상태에서 내부 뷰가 `ScrollView`이고 동시에 `height`를 설정할 경우 적용 안됨.
    ```HTML
    <View style={{flex: 1, flexDirection: 'column', backgroundColor: '#000000' }}>
      <ScrollView style={{height: 200}}>
        // ...
      </ScrollView>
    </View>
    ```
    위와 같은 경우에 `ScrollView`에 height가 적용되지 않는다. 그렇기 때문에 아래처럼 `ScrollView`를 감싼 뷰에 `height`속성을 주어야 한다.

    ```HTML
    <View style={{flex: 1, flexDirection: 'column', backgroundColor: '#000000' }}>
      <View style={{height: 200}}>
        <ScrollView>
          // ...
          </ScrollView>
      </View>
    </View>
    ```
7.


일단 인스타그램의 프로필 화면을 비슷하게 구현하는 중이라 레이아웃을 다 짠 후, 본격적인 구현 다시 시작해 볼 생각이다. 리액트 네이티브(React Native), Expo의 레이아웃을 구성하기에는 정말 쉽고 효율적인 것 같다.

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
