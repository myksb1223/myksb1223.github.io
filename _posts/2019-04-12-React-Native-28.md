---
layout: post
title:  "리액트 네이티브(React Native) 사용기 28편 - Props와 State의 차이"
date:   2019-04-12 01:51:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Instagram 인스타그램 Props State
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 인스타그램(Instagram) 따라 만들면서 여러 부분에서 찾아보면서 막무가내로 사용한 `Props`와 `State`의 차이를 조금 써볼까 한다. (완전 틀릴 수도 있다. 그냥 사용해본 결과의 대한 느낌이기에 틀린 부분이 있으면 알려주면 감사하겠습니다.)

일단, `Props`와 `State`의 가장 큰 차이는 다른 컴포넌트로의 전달여부이다.

당연히 `State`의 값도 다른 컴포넌트로 전달할 수 있지만, 이 때, 바로 전달할 수 있는 것이 아닌 `Props`를 통해서 전달해야한다는 것이다.

```javascript
export default class Parent extends React.Component {
  constructor(props) {
      let data = ...;
      this.state = {
        data: data;
      }
  }

  render() {
      return (
        <Child
          parentData={this.state.data}
           />
      );
  }
}

export default class Child extends React.Component {
  constructor(props) {
      super(props)
      alert(this.props.parentData);
  }
}
```

위의 코드 처럼 Parent의 `State`값을 Props를 통해 전달할 수 있다. 즉, Parent는 **parentData**라고 하는 `Props`를 생성하여 Child에게 전달한 것이다.

또한 아래처럼 부모의 Props를 자식에게 그대로 전달할 수도 있다.

```javascript
export default class Parent extends React.Component {
  constructor(props) {
      let data = ...;
      this.state = {
        data: data;
      }
  }

  render() {
      return (
        <Child
          {...this.props}
           />
      );
  }
}

export default class Child extends React.Component {
  constructor(props) {
      super(props)
  }
}
```
위의 경우는 `{...this.props}`를 통해서 Parent가 가지고 있는 모든 `Props`를 전달한 것이다.

`Props`는 **defaultProps**를 사용하여 미리 정의할 수 있다.

```javascript
export default class Parent extends React.Component {
  //...
}

Parent.defaultProps {
  //...
}
```

또한 `State`는 변경하면 렌더를 다시할 수 있기 때문에 새롭게 렌더가 필요한 경우(예를 들어 ListView의 data값의 변경과 같은 부분) 에 주로 사용하는 것 같다.

다음은 `Promise`에 대해 이해한 부분을 짧게 써보려고 한다.

[Promise(프로미스)란?](https://myksb1223.github.io/reactnative/2019/04/13/React-Native-29.html)

인스타그램(Instagram) 따라 만들기 전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
