---
layout: post
title:  "리액트 네이티브(React Native) 사용기 31편 - 간단히 바인드(bind) 알아보기"
date:   2019-04-24 02:57:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Instagram 인스타그램 바인드 bind
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 인스타그램(Instagram) 따라 만들면서 `바인드(bind)`라는 개념이 나와서 공부하면서 글을 남겨보려고 한다.

`바인드(bind)`는 말 그대로 엮는다는 뜻인데 즉, 내가 사용해야할 메소드의 주인이 누구인지를 정해주는 것이라고 생각하면 될 것 같다.

```javascript
export default class TestScreen extends React.Component {

  doSomthing() {
    // Do something.
  }

  render() {
    return (
      <TouchableOpacity
        onPress={this.doSomething}>
      </TouchableOpacity>
    );
  }
}
```

위와 같이 코딩하면 `doSomething` 메소드가 호출되지 않고 에러가 난다. 즉, 현재 컴포넌트에 doSomething이라는 것을 찾지 못했다는 뜻이다.

그렇기 때문에 `바인드(bind)`라는 개념을 이용하여 코딩해야 한다. 당연히 컴포넌트 내에 있는데 this를 왜 따로 정의해줘야 하는가에 큰 의문이 들었지만, 자바스크립트의 특성상 `this`라는 개념이 상황에 따라 바뀔 수 있기 때문에 그런 것 같다.

그래서 아래와 같은 방법들로 코딩할 수 있다.

```javascript
export default class TestScreen extends React.Component {

  doSomthing() {
    // Do something.
  }

  render() {
    return (
      <TouchableOpacity
        onPress={() => {this.doSomething}}>
      </TouchableOpacity>
    );
  }
}
```

```javascript
export default class TestScreen extends React.Component {

  doSomthing = () =>  {
    // Do something.
  }

  render() {
    return (
      <TouchableOpacity
        onPress={this.doSomething}>
      </TouchableOpacity>
    );
  }
}
```

```javascript
export default class TestScreen extends React.Component {

  doSomthing {
    // Do something.
  }

  render() {
    return (
      <TouchableOpacity
        onPress={this.doSomething.bind(this)}>
      </TouchableOpacity>
    );
  }
}
```

```javascript
export default class TestScreen extends React.Component {
  constructor(props) {
    super(props);

    this.doSomething = this.doSomething.bind(this);
  }

  doSomthing {
    // Do something.
  }

  render() {
    return (
      <TouchableOpacity
        onPress={this.doSomething}>
      </TouchableOpacity>
    );
  }
}
```

위와 같이 다양한 방법이 있다. 이 때, 당연하지만 신경써야할 부분은 내가 어떤 함수 내에서 `this`를 사용할 때만 위와 같은 바인딩이 필요하다는 것이다.

```javascript
export default class TestScreen extends React.Component {
  DatabaseUtil.firstRead().then(function(data){
    alert("here");
  });
}
```

위와 같이 `Promise`를 이용하고 그 결과를 받는 함수에서 `this`와 관련된 처리가 없다면 `바인드(bind)`를 할 필요가 없다. 하지만 아래와 같은 경우는 이야기가 다르다.

```javascript
export default class TestScreen extends React.Component {
  DatabaseUtil.firstRead().then(function(data){
    this.read();
  }.bind(this));

  read() {
    // Read something.
  }
}
```

`바인드(bind)`를 공부하면서 자연스럽게 javascript에 대한 내용을 공부하게 되어서 좋은 계기가 된 것 같다.

인스타그램(Instagram) 따라 만들기 전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
