---
layout: post
title:  "리액트 네이티브(React Native) 사용기 17편 - Navigator 조금 더 파헤치기"
date:   2019-03-19 02:56:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Navigator tabBar Drawer
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 [이전 글](https://myksb1223.github.io/reactnative/2019/03/18/React-Native-16.html)까지 구현한 부분에서 이제 BottomTab을 숨겨볼 차례이다.

일단 최초 화면에서는 BottomTab이 있어도 괜찮지만 Navigator에서의 두 번째 스택 이후에는 tabBar가 사라지는 것을 구현하고 싶었다.

일단 [Document](https://reactnavigation.org/docs/en/navigation-options-resolution.html#a-tab-navigator-contains-a-stack-and-you-want-to-hide-the-tab-bar-on-specific-screens)를 참고한다.

아주 간단하다.

```Javascript
const Navigator = createStackNavigator({
  Home: { screen: HomeScreen },
  User: { screen: CreateUserScreen },
  Create: { screen: CreateScreen },
  Camera: { screen: CameraScreen },
});

Navigator.navigationOptions = ({ navigation }) => {
  let tabBarVisible = true;
  if (navigation.state.index > 0) {
    tabBarVisible = false;
  }

  return {
    tabBarVisible: tabBarVisible,
  };
};

const ProfileNavigator = createStackNavigator({
  Profile: { screen: ProfileScreen },
});

const DashboardTabRoutes = createBottomTabNavigator({
   Root: { screen: Navigator },
   Profile: { screen: ProfileNavigator },
})

const Container = createAppContainer(DashboardTabRoutes);
```

이번에는 조금 심화해서, `Drawer`와 `Tabbar`와 `Stack` 모두 사용해 보도록 하려고 한다.

### 시나리오
1. 맨 처음화면에 TabNavigator가 보인다. 이 화면에서는 DrawerNavigator도 구동된다.
2. 첫 화면에서 두 번째 스택이 쌓이는 화면으로 들어갔을 경우, DrawerNavigator는 구동이 정지되고, TabNavigator의 Tabbar는 보이지 않는다.

이 경우에는 아까 본 Document의 마지막 문단 쪽 코드를 잘 이용하면 된다.

```Javascript
const Navigator = createStackNavigator({
  Home: { screen: HomeScreen },
});

Navigator.navigationOptions = ({ navigation }) => {
  let drawerLockMode = 'unlocked';
  if (navigation.state.index > 0) {
    drawerLockMode = 'locked-closed';
  }

  return {
    drawerLockMode: drawerLockMode,
  };
};

const ProfileNavigator = createStackNavigator({
  Profile: { screen: ProfileScreen },
});

const RootDrawer = createDrawerNavigator({
    NavigatorHome : {
      screen: Navigator,
    }
  });

const HomeStack = createStackNavigator({
    Drawer: { screen: RootDrawer},
    User: { screen: CreateUserScreen },
    Create: { screen: CreateScreen },
    Camera: { screen: CameraScreen },
  });

HomeStack.navigationOptions = ({ navigation }) => {
    let tabBarVisible = true;
    if (navigation.state.index > 0) {
      tabBarVisible = false;
    }

    return {
      tabBarVisible: tabBarVisible,
    };
  };


const DashboardTabRoutes = createBottomTabNavigator({
   Home: { screen: HomeStack },
   Profile: { screen: ProfileNavigator,
            },
});

const Container = createAppContainer(DashboardTabRoutes);
```

위와 같이 구현하면 잘 작동은 하는데 첫 화면에 `Navigator`의 `Header`가 겹치게 된다. 이유는 `createStackNavigator`를 두번 적용되기 때문이다. 그러므로 아래처럼 고쳐준다.

```Javascript
const HomeStack = createStackNavigator({
    Drawer: { screen: RootDrawer,
      navigationOptions: {
          header:null
      }, },
    User: { screen: CreateUserScreen },
    Create: { screen: CreateScreen },
    Camera: { screen: CameraScreen },
    /* add routes here where you want the drawer to be locked */
  });
```

그러면 겹친 `HomeScreen`의 화면이 제대로 보이게 된다.

다음은 리액트 네이티브(React Native), Expo의 tabBar에 선택된 사용자의 아이콘을 표시하는 방법을 공부해서 글을 남겨보려고 한다.

[Tabbar 이미지 Dynamic하게 변경하기](https://myksb1223.github.io/reactnative/2019/03/20/React-Native-18.html) 보기

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
