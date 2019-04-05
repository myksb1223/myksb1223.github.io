---
layout: post
title:  "리액트 네이티브(React Native) 사용기 26편 - Profile화면 구현하기 4편(global 조금 더 활용하기)"
date:   2019-04-06 03:56:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Instagram 인스타그램
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 [Profile화면 구현하기 3편](https://myksb1223.github.io/reactnative/2019/03/29/React-Native-25.html)까지 구현한 부분에서 다른 Screen에서 데이터가 수정되었을 때 바로 ProfileScreen에 반영하는 방법에 대해 글을 남기려고 한다.

일단, `BottomTabNavigator`에 각각 `StackNavigator`가 존재하는 형태이다.

일단, `StackNavigator`에서의 Screen간의 데이터에 대한 흐름은 자연스럽게 할 수 있다.

`setParams`과 `getParam`등을 이용하면 된다. 그리고 이러한 데이터 흐름은 `componentWillReceiveProps`와 같은 메소드를 부르기 때문에 새로운 데이터에 대한 반영이 매우 쉽다.

지금 구현하는 인스타그램에서는 `HomeScreen`으로 가는 탭과 `ProfileScreen`으로 가는 탭이 있는데 일단 글의 작성은 `HomeScreen`쪽에서 일어난다. 그러면 어떻게 다른 탭의 Screen에 바로 반영을 시킬 수 있을까?

나는 `setState`를 어떻게든 호출하고 싶었다. 그래서 `shouldComponentUpdate(nextProps, nextState)`메소드를 활용하여 데이터를 새롭게 읽고 싶었다. 당연히 `setState`의 특성상 렌더도 다시 하기때문에 가장 최적의 방법이라고 생각했다.

그래서 생각해 본 것 중에 바로 `global` 변수를 활용하는 것이었다.

내가 `ProfileScreen`을 들어간 시점에 컴포넌트 자체를 `global`에 저장하는 것이다.

```javascript
export default class ProfileScreen extends React.Component {
  // ...

  constructor(props) {
    super(props);

    //...

    global.profileScreen = this;
  }

  shouldComponentUpdate(nextProps, nextState) {
    if(nextState.needReload) {
        // 데이터 다시 읽기
    }
    return this.state !== nextState;
  }
  // ...
}
```

그러면 `HomeScreen`에서 데이터 변경이 있을 시에는 `componentWillReceiveProps`메소드를 호출할 것이므로...(`HomeScreen` => `CreateScreen` => `HomeScreen`)

```javascript
export default class HomeScreen extends React.Component {

  //...

  componentWillReceiveProps(nextProps){

    if(this.props === nextProps){
      return;
    }

    if(global.profileScreen !== null) {
      global.profileScreen.setState({needReload: true});
    }
  }

  // ...
}
```

위와 같이 만들어 주면, `ProfileScreen`에 돌아갔을 때도 데이터가 변경되어 있다.

아직 초심자라서 `global`을 남용하면 안될 것 같지만 잘만 활용한다면 정말 편하게 내가 원하는 부분에 활용할 수 있는 것 같다.

나중에 글을 작성하겠지만 이렇게 `global`을 활용하면 하트표시 변경 등의 다양한 값들을 자유롭게 변경할 수 있다.

이제 웬만큼 형태는 다 잡혔으니 다음은 리액트 네이티브(React Native), Expo Instagram 프로젝트에서 ListView의 refresh, loadmore에 관한 글을 작성할 것이다.

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
