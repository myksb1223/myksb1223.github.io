---
layout: post
title:  "리액트 네이티브(React Native) 사용기 30편 - Prevent React setState on unmounted Component"
date:   2019-04-17 02:57:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Instagram 인스타그램
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 인스타그램(Instagram) 따라 만들면서 `ProfileScreen`부분을 `BottomTabNavigator`, `StackNavigator` 이렇게 두군데에서 사용하였다.

`StackNavigator`내에 존재하는 화면에서 사용자 이름을 클릭하거나 이미지를 클릭하면 `ProfileScreen`과 같은 형태에 데이터만 바뀐 화면이 보여야 한다. 그렇기 때문에 `ProfileScreen`을 함께 사용하기로 하였다.


```javascript
const ProfileNavigator = createStackNavigator({
  Profile: { screen: ProfileScreen },
  // ...
});

const HomeStack = createStackNavigator({
  // ...
  UserProfile: { screen: ProfileScreen},
});
```

위 처럼 같은 화면을 사용하더라도 `routeName`은 다르게 가져가 주어야 한다.

`ProfileScreen`에서 TopNavigationBar를 클릭하면 사용자 목록이 뜨는데, 이를 위해서 아래와 같이 구현하였다.

```javascript
let _this;

export default class ProfileScreen extends React.Component {
  static navigationOptions = ({navigation}) => {
    const {params = {}} = navigation.state;
    return {
      headerTitle: <ProfileHeaderTitle
                    {...navigation}
                    ref={title => {
                      navigation.title = title;
                    }}
                    routeName={navigation.state.routeName}
                    onPopupView={params.updatePopup}/>,
      headerTitleStyle: {textAlign:'center', alignSelf:'center',flex:1},
    }
  };

  constructor(props) {
    super(props);

    // ...
    this.props.navigation.setParams({
      updatePopup: this.updatePopup,
    });

    _this = this;
  }

  updatePopup(popUp) {
    _this.setState({popUp: popUp});
  }

  // ...

  render() {
    return (
      // ...
    );
  }
}
```

위와 같이 구성하였는데, `StackNavigator`에서 본 `ProfileScreen`이 사라진 후, `BottomTabNavigator`의 `ProfileScreen`으로 간 후,  TopNavigationBar를 클릭하면 아래와 같은 에러가 생긴다.

    Warning: Can’t call setState (or forceUpdate) on an unmounted component.
    This is a no-op, but it indicates a memory leak in your application.
    To fix, cancel all subscriptions and asynchronous tasks in the componentWillUnmount method.

위와 같은 이유는 이미 해제된 컴포넌트에서 `setState`를 불렀다는 것이다.

이해가 안되었다. `StackNavigator`에서 생성한 `ProfileScreen`과 `BottomTabNavigator`에서 생성한 화면은 완벽히 다른 화면이기 때문이었다.

하지만 원인은 `_this`에 있었다. 이 `_this`의 값은 `ProfileScreen`과 공유가 되는 변수이었던 것이다. 그래서 이 부분을 아래와 같이 고쳐주었다.

```javascript
componentWillUnmount() {
  _this = global.profileScreen;
}
```

위와 같이 해제될 때, 기존 `BottomTabNavigator`의 `ProfileScreen`으로 변경시켜 주었다.

다음은 `bind`에 대해서 글을 써보려고 한다.

인스타그램(Instagram) 따라 만들기 전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
