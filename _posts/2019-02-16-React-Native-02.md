---
layout: post
title:  "리액트 네이티브(React Native) 사용기 2편 - 구조와 문법 파악 및 Navigation 적용기..."
date:   2019-02-16 03:53:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 Expo Navigation
sitemap :
  changefreq : daily
  priority : 1.0
---

제대로 된 체험에 앞서서 구조와 문법 파악이 중요하기에 부딪혀 보았다.

일단, React 문법 자체를 모르기 때문에 공부를 하기위해서 이것 저것 찾아보았지만, 역시나 그냥 남의 코드를 보며 익히고, 그냥 직접 구현해 보는 것이 가장 내 스타일이라고 판단하여 바로 Instagram을 따라 만들어 보기로 했다.

과외로 Instagram 따라 만들기라는 주제로 하기에... Expo를 이용하여 만들어 보기로 결정했다.

일단 기본적으로 아래와 같은 구조를 가진다.

```Javascript
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';

export default class App extends React.Component {
  render() {
    return (
      //...
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```

일단 import하는 부분은 워낙 자주 보던 표현이라 바로 알 수 있었고, 이제 class 선언하는 부분의 `export default`라는 부분이 눈에 띄었다.

일단, 저 App이라는 class는 가장 처음 시작되는 부분의 class로 생각하면 될 것 같다. 그래서 `default`라는 문구가 붙었다고 파악하고 넘어간다.

얘네는 상속을 매번 `Component`를 하는 것을 봐서는 항상 기본 틀인 것 같다. 약간 뷰를 쪼개서 관리하는 느낌이다.

그리고 아래보면 Style에 관한 코드가 있는데, Javascript문법을 잘 모르지만 대충 `StyleSheet`라는 것을 하나의 객체 개념으로 가져가는 것 같다.

### 1. Navigation 구현
일단 가장 먼저, [Document 문서](https://docs.expo.io/versions/latest/react-native/navigation/)를 보면 설명이 나와 있다.

여기서 부터... 막혔는데, 해당 문서에 아래와 같은 코드가 있다.

```Javascript
import {
  createStackNavigator,
} from 'react-navigation';

const App = createStackNavigator({
  Home: { screen: HomeScreen },
  Profile: { screen: ProfileScreen },
});

export default App;
```

여기서 일단 class 선언이 없어서 뭐지 했다. 알고보니 `Component`개념을 저렇게 함수형태로도 만들 수 있는 것 같다.

그대로 따라하면 아래와 같은 에러가 뜬다...

    Invariant Violation: The navigation prop is missing for this navigator.
    In react-navigation 3 you must set up your app container directly.
    More info: https://reactnavigation.org/docs/en/app-containers.html

찾아본 결과 이제는 무조건 AppContainer라는 것을 사용한 후에 Navigation을 만들어야 한다.

그래서 아래와 같이 고치면...

```Javascript
import React from 'react';
import { Button } from 'react-native';
import {
  createStackNavigator,
  createAppContainer
} from 'react-navigation';

class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Welcome',
  };
  render() {
    const { navigate } = this.props.navigation;
    return (
      <Button
        title="Go to Jane's profile"
        onPress={() =>
          navigate('Profile', { name: 'Jane' })
        }
      />
    );
  }
}

const App = createStackNavigator({
  Home: { screen: HomeScreen },
  // Profile: { screen: ProfileScreen },
});

const Container = createAppContainer(App);

export default Container;
```

참고로, Document 문서에서는 HomeScreen관련 코드밖에 없지만 우리는 반드시 ProfileScreen까지 만들어 주어야 한다. 아니면 `Profile: { screen: ProfileScreen },`을 삭제하도록 하자!

그러면 이제 Document 문서에서 하는 대로 될 것이다.
항상 참고는 Document 문서를 기준으로 하는 것이 가장 좋은 것 같다.

이 구조를 이해하는데 2시간 걸렸다... 그래도 다행히 Navigation을 넣었다는 것에 만족한다.
