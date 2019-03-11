---
layout: post
title:  "리액트 네이티브(React Native) 사용기 12편 - Picker 사용시 주의점"
date:   2019-03-08 04:35:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 Expo Picker
sitemap :
  changefreq : daily
  priority : 1.0
---

[이전 글](https://myksb1223.github.io/reactnative/2019/03/08/React-Native-10.html)까지 완료했던 작업을 조금씩 보완하려고 한다.

일단, Content 수정 시, `Picker`에 사용자 목록이 나타나는데, 이 때, 사용자를 움직여도... 계속 처음 위치로 되돌아 간다.

그래서 찾아본 결과 아래와 같다. 링크는 [여기](https://stackoverflow.com/a/45092288).

1. Picker의 selectedValue는 무조건 숫자아니면 문자형태이다.
2. `selectedValue`와 내가 넣으려는 값이 서로 다를 경우이다.

그냥 보자마자... 첫 번째 이유였다. 나는 이 작업에서 JSON데이터 자체를 selectedValue에 넣었기 때문이다.

그래서 코드를 아래처럼 고쳤다.

```Javascript
export default class CreateScreen extends React.Component {
  // ...

  componentWillMount() {
    db.transaction(
      tx => {
        tx.executeSql('SELECT * FROM users', [], (_, { rows: { _array } }) => {
            let user = _array[0]["id"]
            if(this.state.updated !== null) {
              for(var i in _array) {
                if(this.state.updated["user_id"] === _array[i]["id"]) {
                  user = _array[i]["id"]
                  break
                }
              }
            }
            this.setState({ users: _array, loading: false, user: user })
          }
        );
      },
    );
  }

  // ...

  add() {
    let picture = null;
    let name = null;
    for(var i in _this.state.users) {
      if(_this.state.user === _this.state.users[i]["id"]) {
        picture = _this.state.users[i]["picture"];
        name = _this.state.users[i]["name"];
      }
    }

    // ...
  }

  render() {
    // ...

    return (
      <View style={styles.container}>
        <Picker
           selectedValue={this.state.user}
           onValueChange={(itemValue, itemIndex) => {
             this.setState({user: itemValue})
           }}>
           { this.state.users.map((item, key)=>(
              <Picker.Item label={item.name} value={item.id} key={key} />)
              )}

         </Picker>
         // ...
      </View>
    );
  }
}
```

위와 같이 같이 변경하니 `Picker`가 잘 변경되고 수정도 잘 되었다.
다음엔 `ListView`의 Row에 `ImageView` **pagination**을 걸어볼 생각이다.

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
