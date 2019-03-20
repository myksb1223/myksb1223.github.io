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

리액트 네이티브(React Native), Expo로 구현하다가 레이아웃 관련 부분에서 잊지말아야 하는 것들(?)을 꾸준히 생길 때마다 업데이트 하면서 기록하려고 한다.

1. `flex`와 `flexDirection` : 그냥 뷰의 비율과 배치 방향이라고 생각하면 쉬운 것 같다.

    ```HTML
    <View style={{flex: 1, flexDirection: 'column'}}>
      <View style={{flex: 1, backgroundColor: '#0000ff'}} />
      <View style={{flex: 4, backgroundColor: 'skyblue'}} />
    </View>
    ```

    세로로 1:4의 비율로 뷰가 세로(column)로 배치된다. `column`을 `row`로 변경하면 가로로 1:4의 비율로 뷰가 배치된다.

2. `justifyContent`와 `alignItems` : center만 써 보았지만 일단, flexDirection에 영향을 받는다. (추후 사용할 때 마다 기록을 남길 예정이다.)

    ```HTML
    <View style={{flex:1, flexDirection: 'row', backgroundColor: 'steelblue', justifyContent: 'center', alignItems: 'center'}}>
      <Image source={{ uri: uri }} style={{width:70, height:70, borderRadius: 35}}/>
    </View>
    <View style={{flex:3, backgroundColor: 'red'}} />
    ```

    위 부분에서 중요한 부분은 아래부분이다.

    ```HTML
    // 가운데 정렬
    <View style=`{{flex:1, flexDirection: 'row', backgroundColor: 'steelblue', justifyContent: 'center', alignItems: 'center'}}`>

    <View style={{flex:1, flexDirection: 'column', backgroundColor: 'steelblue', justifyContent: 'center', alignItems: 'center'}}>

    // 수평 정렬
    <View style={{flex:1, flexDirection: 'row', backgroundColor: 'steelblue', justifyContent: 'center'}}>

    <View style={{flex:1, flexDirection: 'column', backgroundColor: 'steelblue', alignItems: 'center'}}>

    // 수직 정렬
    <View style={{flex:1, flexDirection: 'column', backgroundColor: 'steelblue', justifyContent: 'center'}}>

    <View style={{flex:1, flexDirection: 'row', backgroundColor: 'steelblue', alignItems: 'center'}}>
    ```

3. 다음은 무엇일까나...?


일단 인스타그램의 프로필 화면을 비슷하게 구현하는 중이라 레이아웃을 다 짠 후, 본격적인 구현 다시 시작해 볼 생각이다. 리액트 네이티브(React Native), Expo의 레이아웃을 구성하기에는 정말 쉽고 효율적인 것 같다.

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
