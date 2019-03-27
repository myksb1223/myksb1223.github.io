---
layout: post
title:  "리액트 네이티브(React Native) 사용기 15편 - BottomTab과 Navigation 함께 이용하기"
date:   2019-03-15 05:05:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Navigation BottomTab tabBarIcon
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 [이전 글](https://myksb1223.github.io/reactnative/2019/03/14/React-Native-14.html)까지 구현한 부분에 이제 BottomTab을 넣어볼 차례이다.

일단 BottomTab에 관한 지식이 전무하기 때문에... 역시나.. [Document](https://reactnavigation.org/docs/en/tab-based-navigation.html)를 참고한다.

Document를 전부 읽지 않았다면... 방법을 전혀 몰랐을 것이지만!
맨 밑에 보면 [A stack navigator for each tab](https://reactnavigation.org/docs/en/tab-based-navigation.html#a-stack-navigator-for-each-tab)라고 설명이 있다. 꼭 Document 메뉴라도 꼭 다 읽어보도록 하자.

그러면 아래와 같이 구현할 수 있다.

```Javascript
const Navigator = createStackNavigator({
  Home: { screen: HomeScreen },
  User: { screen: CreateUserScreen },
  Create: { screen: CreateScreen },
  Camera: { screen: CameraScreen },
});

const ProfileNavigator = createStackNavigator({
  Profile: { screen: ProfileScreen },
});

const DashboardTabRoutes = createBottomTabNavigator({
   Home: Navigator,
   Profile: ProfileNavigator
});

const Container = createAppContainer(DashboardTabRoutes);

export default class App extends React.Component {
  render() {
    return (
      <Container />
    );
  }
}
```

너무 허전하니, `tabBarIcon`에 이미지를 넣어보기로 한다. 찾아보면 방법도 많고, 미리 지원해주는 [이미지](https://expo.github.io/vector-icons/)들도 많다. 또한 `Icon`에 대한 [내용](https://docs.expo.io/versions/latest/guides/icons/)도 참고하길 바란다.

하지만 나는 새로운 이미지를 넣고 싶었기에... Document의 예제를 수정하였다.

```Javascript
const DashboardTabRoutes = createBottomTabNavigator({
   Home: Navigator,
   Profile: ProfileNavigator
},
{
    defaultNavigationOptions: ({ navigation }) => ({
      tabBarIcon: ({ focused, horizontal, tintColor }) => {
        const { routeName } = navigation.state;
        let iconName = '';
        if (routeName === 'Home') {
          iconName = focused ? require('./assets/home_focused.png') : require('./assets/home.png');

          // iconName = `./assets/home${focused ? '_focused' : ''}.png`;
          // Sometimes we want to add badges to some icons.
          // You can check the implementation below.
          // IconComponent = HomeIconWithBadge;
        } else if (routeName === 'Profile') {
          iconName = focused ? require('./assets/profile_focused.png') : require('./assets/profile.png');

          // iconName = `./assets/profile${focused ? '_focused' : ''}.png`;
        }

        // You can return any component that you like here!
        return <Image source={ iconName } style={% raw %}{{width: 25, height: 25}}{% endraw %}/>;
      },
    }),
    tabBarOptions: {
      showLabel: false,
      activeTintColor: 'tomato',
      inactiveTintColor: 'gray',
    },
  });
```

참고로 `Image`에서 `require`를 사용할 때는 [여기](https://facebook.github.io/react-native/docs/images.html)에 쓰여진 것처럼 사용해야 한다.

즉, 아래와 같이 사용하지 말아야 한다.

```Javascript
let iconName = focused ? 'home_focused' : 'home';
<Image source={require('./assets/' + iconName + '.png')} />;
```

이렇게 하면 탭이 생긴 것을 알 수 있다.

다음은 리액트 네이티브(React Native), Expo의 이미지 로테이션에 관한 방법을 공부해서 글을 남겨보려고 한다.

[이미지 로테이션 하기](https://myksb1223.github.io/reactnative/2019/03/18/React-Native-16.html) 보기

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
