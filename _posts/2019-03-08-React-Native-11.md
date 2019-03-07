---
layout: post
title:  "리액트 네이티브(React Native) 사용기 11편 - ListView에 수정, 삭제 반영"
date:   2019-03-08 03:34:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 Expo ListView SQLite
sitemap :
  changefreq : daily
  priority : 1.0
---

[이전 글](https://myksb1223.github.io/reactnative/2019/02/27/React-Native-10.html)까지 완료했으면 이제는 저장된 Content들을 수정, 삭제할 차례이다.

이 때, 고민해야할 것들이 몇가지 있다.
- 수정버튼을 누르면 컨텐츠 생성화면으로 가고, 내가 선택한 데이터들이 표현되어 있어야 한다.
- 수정 후, `ListView`에 반영되어야 한다.
- 삭제 시, `ListView`에 반영되어야 한다.

`View`에 버튼을 추가하는 것은 쉽기 때문에 생략하기로 하고 바로 수정하는 부분으로 넘어가도록 한다.

    참고로 **Button**은 글씨 크기조절이 안되니, **TouchableOpacity**을 사용하기를 추천한다.

### 수정 부분

일단, 수정을 눌렀을 때 가장 처음으로 필요한 값은 내가 누른 Row의 **Index**값이다.

그리고 난 후에는 내가 표현한 데이터들 중에서 해당 **Index**에 해당하는 녀석의 `SQLite`의 **id**값이다.

그러면 아래와 같은 순서를 생각해 볼 수 있다.

1. `SQLite`에서 읽은 배열 값들을 state에 저장해 둔다.
2. Row가 선택되면 state에 저장해둔 배열에서 선택된 **Index**에 해당하는 값을 뽑아낸다.
3. 뽑아낸 값을 `Navigation`을 통해 넘긴다.

그럼 이전 글에서 `read`함수와 `ListView`부분을 아래와 같이 고친다.

```Javascript
read() {
  db.transaction(
    tx => {
      tx.executeSql('SELECT * FROM contents', [], (_, { rows: { _array } }) => {
          this.setState({ dataSource: this.state.dataSource.cloneWithRows(_array), datas: _array})
        }
      );
    },
  );
}

render() {
  return (
    <ListView
      dataSource={this.state.dataSource}
      renderRow={(rowData, sectionID, rowID) =>
        // ...
        <TouchableOpacity
          style={{ marginLeft: 12, marginRight: 12 }}
          onPress={() => {
              let data = this.state.datas[rowID]
              this.props.navigation.navigate('Create', { data: data, image: data["picture"] })
            }
          }>
          <Text style={{fontSize: 14, textAlign: 'center', textAlignVertical: "center"}}>수정</Text>
        </TouchableOpacity>
        // ...
    />
  );
}

// ...
```

위 코드에서 `data`의 값이 JSON이기 때문에 `data["picture"]`로 접근할 수 있다.

이제 수정 화면의 부분을 생각해보면..

1. `Text`에 글이 채워져 있어야 한다.
2. `Image`에 이미지가 보여야 한다. [이전 글](https://myksb1223.github.io/reactnative/2019/02/23/React-Native-06.html)에서 `componentWillReceiveProps`을 구현해 두어서 그대로 놔두어도 된다.
3. 사용자는 내가 선택한 `User`로 선택되어 있어야 한다.

그대로 코드로 구현하면 아래와 같다.

```Javascript
//...

constructor(props) {
  super(props);

  let updated = this.props.navigation.getParam("data", null);
  let text = '';
  if(updated !== null) {
    text = updated["content"]
  }
  this.state = {text: text, height: 0, users: null, user: null, loading: true, updated: updated};
  _this = this;
  // ...
}

componentWillMount() {
  db.transaction(
    tx => {
      tx.executeSql('SELECT * FROM users', [], (_, { rows: { _array } }) => {
          let user = _array[0]
          if(this.state.updated !== null) {
            for(var i in _array) {
              if(this.state.updated["user_id"] === _array[i]["id"]) {
                user = _array[i]
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

add() {
  if(_this.state.updated !== null) {
    db.transaction(
      tx => {
        tx.executeSql('UPDATE contents SET content = ?, picture = ?, user_id = ?, user_name = ?, user_pic = ? WHERE id = ?', [_this.state.text, _this.image.state.image, _this.state.user.id, _this.state.user.name, _this.state.user.picture, _this.state.updated["id"]]);
      },
    );
  }
  else {
    // INSERT Code...
  }
}
// ...
```

### 삭제 부분

삭제 부분에서 가장 고민해야 할 부분은 즉시, `ListView`에 반영하는 것이다.

`this.state.dataSource`를 그대로 사용하면 이상하게 `rowHasChanged`가 호출되지 않아 찾아본 결과 아래와 같이 해야했다. (원인을 알면 댓글이나 연락 부탁드립니다.)

```Javascript
let ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});

export default class HomeScreen extends React.Component {
  // ...

  constructor() {
    super();

    this.state = {
      dataSource: ds.cloneWithRows([]),
      datas: null
    };
  }

  delete(contentId) {
    db.transaction(
      tx => {
        tx.executeSql(`DELETE FROM contents WHERE id = ?;`, [contentId]);
      },
    )
  }

  deleteRow(rowId) {
    this.state.datas.splice(rowId, 1)
    this.setState({
      dataSource: ds.cloneWithRows( this.state.datas ),
    })
  }

  render() {
    return (
      <ListView
        dataSource={this.state.dataSource}
        renderRow={(rowData, sectionID, rowID) =>
          // ...
          <TouchableOpacity
            onPress={() => {
                let data = this.state.datas[rowID]
                this.delete(data["id"])
                this.deleteRow(rowID)
              }
            }>
            <Text style={{fontSize: 14}}>삭제</Text>
          </TouchableOpacity>
          // ...
      />
    );
  }
}
```

위와 같이 구현하면 수정, 삭제 후의 모든 값이 반영된다.

이렇게 인스타그램의 메인 뷰 부분을 그대로 따라 만드는 나만의 구성대로 만들어 보았다.

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
