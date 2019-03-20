---
layout: post
title:  "리액트 네이티브(React Native) 사용기 18편 - Tabbar 이미지 Dynamic하게 변경하기"
date:   2019-03-20 05:22:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Navigator tabBar
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 [이전 글](https://myksb1223.github.io/reactnative/2019/03/19/React-Native-17.html)까지 구현한 부분에서 Tabbar에 내가 원하는 이미지를 넣어 볼 차례이다.

일단 최초 화면에서는 Tabbar의 사용자 쪽 아이콘은 기본아이콘이다. 이 부분을 사용자를 생성하면 사용자를 생성할 때 사용한 이미지로 바꾸는 작업을 할 것이다. 즉, 다이나믹(Dynamic)한 Tabbar 이미지의 변경이다.

### 시나리오
1. SQLite의 users 테이블을 지웠다가 current 필드를 추가한다. (current가 1인 사용자의 이미지를 보여줄 것이다.)
2. 사용자를 생성하면 Tabbar에 사용자 이미지가 보여진다.

일단 기존에 있던 테이블을 모두 지우고 다시 만들면 된다.
```Javascript
// App.js

componentDidMount() {
    db.transaction(tx => {
      tx.executeSql('DROP TABLE users;');
      tx.executeSql('DROP TABLE contents;');
      tx.executeSql(
        'CREATE TABLE IF NOT EXISTS contents (id INTEGER PRIMARY KEY AUTOINCREMENT, content TEXT, picture TEXT, user_id INTEGER, user_name TEXT, user_pic TEXT);'
      );
      tx.executeSql(
        'CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, picture TEXT, current INTEGER DEFAULT 0);'
      );
  });
}
```

이렇게 한 후에, 이제 사용자 생성쪽을 가서 수정을 해주어야 하는데 다음과 같은 조건이 생긴다.

1. current가 1인 사용자가 없는 경우(아예 사용자가 한 명도 없는 경우)
2. current가 1인 사용자가 있는 경우

일단 사용자가 있는지 없는지를 판단하기 위해서, 앱을 최초에 실행시킬 때, 사용자를 읽고 사용자가 있으면 해당 이미지 경로(picture 컬럼)를 저장한다.

```Javascript
// App.js
global.selectedPath = null;

componentDidMount() {
      // ...
      tx.executeSql('SELECT * FROM users', [], (_, { rows: { _array } }) => {
        // setTimeout(() =>  {
          for(var i in _array) {
            if(1 === _array[i]["current"]) {
              global.selectedPath = _array[i]["picture"]
              break
            }
          }
      });
    });
}

// CreateUserScreen.js

add() {
  let current = 0;
  if(global.selectedPath === null) {
    current = 1;
    global.selectedPath = _this.image.state.image
  }

  db.transaction(
        tx => {
          tx.executeSql('INSERT INTO users (name, picture, current) values (?, ?, ?)', [_this.state.text, _this.image.state.image, current]);
        },
      );
}
```

위와 같이 조건을 주면서 사용자를 추가할 수 있다.

이제는 사용자가 생성되었을 때, 바로 Tabbar에 적용해야 한다.
어떻게 해야지만... Tabbar를 갱신할까 정말 많이 고민했다.

이 결과 알아낸 중요한 몇 가지가 있다.

1. StackNavigator에 둘러쌓인 Screen이 TabNavigator에 들어갔을 때, Tabbar에 관한 속성은 먹지 않는다.
2. global 값에 Navigator자체는 들어가지 않는다. (global값에 Navigator를 직접 넣어서 제어하려고 했다.)
3. Redux라고 하는 개념이 있다.(Dynamic한 변화에 대한 검색 중 알아낸 정보, 꼭 공부해야할 것 같은 개념이다)
4. Tabbar를 갱신하기 위해서는 AppContainer의 갱신이 필요하다.

결국 몇 시간의 노가다 후 `Redux`는 간단히 익혀질 개념이 아니라 판단하고 빠르게 포기했고, 어떻게든 갱신할 방법을 찾았다.

```Javascript
// App.js

global.container = null;

// ...

const DashboardTabRoutes = createBottomTabNavigator({
   Home: { screen: HomeStack },
   Profile: { screen: ProfileNavigator,},
},
{
    defaultNavigationOptions: ({ navigation }) => ({
      tabBarIcon: ({ focused, horizontal, tintColor }) => {
        const { routeName } = navigation.state;
        let iconName = '';
        if (routeName === 'Home') {
          iconName = focused ? require('./assets/home_focused.png') : require('./assets/home.png');
          return <Image source={ iconName } style={% raw %}{{width: 25, height: 25}}{% endraw %}/>;

        } else if (routeName === 'Profile') {
          if(global.selectedPath === null) {
            iconName = focused ? require('./assets/profile_focused.png') : require('./assets/profile.png');

            return <Image source={ iconName } style={% raw %}{{width: 25, height: 25}}{% endraw %}/>;
          }
          else {
            return <Image source={% raw %}{{ uri: global.selectedPath }} style={{width: 25, height: 25}}{% endraw %}/>;
          }
        }
      },
    }),
    tabBarOptions: {
      showLabel: false,
      activeTintColor: 'tomato',
      inactiveTintColor: 'gray',
    },
});

// ...

constructor() {
  super();

  this.state = {
    selectedPath: null,
  };

  global.container = this;
}

// HomeScreen.js

componentWillReceiveProps(nextProps){

  if(this.props === nextProps){
    return;
  }

  this.read();

  global.container.setState({selectedPath: global.selectedPath})
}
```

위와 같이 하니 바로... 변경되었다.

그러면 이제 앱을 처음 켰을 때, current가 1인 사용자가 있을 경우에는 Tabbar 이미지를 변경시켜주어야 한다.

```Javascript
// App.js

componentDidMount() {
      // ...
      tx.executeSql('SELECT * FROM users', [], (_, { rows: { _array } }) => {
        // setTimeout(() =>  {
          for(var i in _array) {
            if(1 === _array[i]["current"]) {
              global.selectedPath = _array[i]["picture"]
              break
            }
          }
      });

      this.setState({selectedPath: global.selectedPath})
    });
}
```

위와 같이 `setState`를 다시 호출해서 AppContainer를 갱신하는 것이다.

솔직히, 이렇게 구현하는 것이 좋은 방법이 아니라는 생각이 든다. 일단 먼저 `setState`로 인해서 쓸데없는 갱신이 많다는 점이다.

하지만 아직 잘 모르는 상태이기 때문에 일단 넘어가기로 하고 `Redux`나 추후에 배우는 개념에서 다른 적용이 가능하면 바꿔보도록 해야겠다.

다음은 리액트 네이티브(React Native), Expo의 ProfileScreen을 구현할 것이다. 실제 인스타 그램의 내 프로필 창과 비슷하게 만들 예정이다.

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
