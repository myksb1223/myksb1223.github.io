---
layout: post
title:  "리액트 네이티브(React Native) 사용기 9편 - Picker의 사용 및 setState"
date:   2019-02-27 00:36:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 Expo Picker state
sitemap :
  changefreq : daily
  priority : 1.0
---

[이전 글](https://myksb1223.github.io/reactnative/2019/02/26/React-Native-08.html)에서 사용자관련 테이블을 만들었다. 사용자 테이블을 만들었으니 사용자 생성화면을 만들면 되는데, 이는 컨텐트 작성화면과 크게 다른 부분이 없기 때문에 생략한다.

컨텐트 화면의 구성을 간단히 이야기하면...

1. 사용자 선택(`Picker` 사용)
2. 글 작성
3. 이미지 추가
4. 등록

위의 4과정이 필요하다.

여기서 `Picker`컴포넌트는 사용자 선택에서 사용될 것인데, 이 때, 주의할 점은 `SQLite`에 저장된 사용자를 읽고 난 후에 `Picker`에 표현해야 한다는 점이다.

무작정 `Picker`를 사용하면 아마 에러가 뜰 것이다. 왜냐하면 `Picker`에 표현할 데이터가 없기 때문이다. 즉, `SQLite`에서 데이터를 읽기 전에 이미 렌더가 되었기 때문이라고 생각하면 쉽다.

여기서의 핵심은 바로 데이터를 읽은 후 렌더를 해야하는 부분이다.
그러므로 아래와 같이 구현할 수 있다.

```Javascript
export default class CreateScreen extends React.Component {
  // ...
  constructor(props) {
    super(props);
    this.state = {text: '', height: 0, users: null, user: null, loading: true};
    _this = this;
    this.props.navigation.setParams({
      done: this.add,
    });
  }

  componentWillMount() {
    db.transaction(
      tx => {
        tx.executeSql('SELECT * FROM users', [], (_, { rows: { _array } }) => {
            this.setState({ users: _array, loading: false, user: _array[0] })
          }
        );
      },
    );
  }
  // ...

  render() {
    let { navigate } = this.props.navigation;

    if(this.state.loading) {
      return (
       <View style={{flex: 1, paddingTop: 20}}>
         <ActivityIndicator />
       </View>
     );
    }

    return (
      <View style={styles.container}>
        <Picker
           selectedValue={this.state.user}
           style={{ height: 50, width: 100 }}
           onValueChange={(itemValue, itemIndex) => this.setState({user: itemValue})}>
           { this.state.users.map((item, key)=>(
              <Picker.Item label={item.name} value={item} key={key} />)
              )}

         </Picker>

         // ...
      </View>
    );
  }
}
```

`state`에 정의된 속성을 살펴보면...

    text : 내가 작성한 글.
    users : Picker에 뿌릴 사용자 데이터.
    user : 선택된 사용자 데이터
    loading : db에서 사용자를 다 읽었는지 아닌지 판단.

`loading`을 넣은 이유는 db가 완전히 읽혀야지만 `Picker`가 렌더되도록 해야하기 때문이다.

그러면 먼저 `componentWillMount`을 이용하여 일단 db에서 사용자 데이터를 읽어들인다. 그리고 난 후 `state`에 등록한다.

이 떄, user라는 곳에 `_array[0]`을 넣은 이유는 초기값은 맨 처음 사용자로 미리 등록시켜놓기 위해서이다.

    **setState**를 사용하면 기본적으로 항상 랜더를 다시하기 때문이 이 특징을 이용한 것이다.

그래서 `render()`를 보면 처음에 `ActivityIndicator`만을 호출하고, `setState`에 users와 user가 등록되고 loading이 true값을 가지면 `Picker`가 렌더되는 구조이다.

이렇게 하면 `Picker`가 에러없이 잘 나타난다. 여기서의 핵심은 `setState`함수를 사용했을 때, 렌더가 다시 된다는 사실이 핵심인 것 같다.
