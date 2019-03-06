---
layout: post
title:  "리액트 네이티브(React Native) 사용기 10편 - ListView 사용하기"
date:   2019-03-07 02:10:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 Expo ListView
sitemap :
  changefreq : daily
  priority : 1.0
---

[이전 글](https://myksb1223.github.io/reactnative/2019/02/27/React-Native-09.html)까지 완료했으면 이제는 저장된 Content들을 `ListView`에 뿌릴 차례이다..

일단 `ListView` 구현에 대한 부분을 아예 모르기 때문에 [Document](https://docs.expo.io/versions/latest/react-native/listview/)를 참조했다.

```Javascript
// ...
export default class HomeScreen extends React.Component {
  // ...

  constructor() {
    super();
    const ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
    this.state = {
      dataSource: ds.cloneWithRows([]),
    };
  }

  // ...

  render() {

    return (
      <ListView
        dataSource={this.state.dataSource}
        renderRow={(rowData) =>
          <View style={{flex: 1, flexDirection: 'column', margin: 12}}>
            <View style={{flex: 1, flexDirection: 'row'}}>
              <Image source={{ uri: rowData.user_pic }} style={{ width: 36, height: 36 }} />
              <Text>{rowData.user_name}</Text>
            </View>
            <Image source={{ uri: rowData.picture, cache: 'force-cache', }} style={{ flex: 1, height: 200 }} />
            <Text>{rowData.content}</Text>
          </View>
        }
        enableEmptySections
      />
    );
  }
}
```

일단 dataSource에 넣을 데이터가 없기 때문에 빈 배열을 넣었다. 빈 배열을 넣으면 아래와 같은 에러 문구가 뜬다.

    Warning: In next release empty section headers will be rendered. In this release you can use 'enableEmptySections' flag to render empty section headers.

이 때는 `enableEmptySections`이 속성을 `ListView`에 추가해 주면 된다.

이제는 `SQLite`에서 값을 읽어오면 된다.

```Javascript
  componentWillMount() {
    this.read();
  }

  componentWillReceiveProps(nextProps){
    if(this.props === nextProps){
      return;
    }

    // TODO perform changes on state change
    this.read();
  }

  read() {
    db.transaction(
      tx => {
        tx.executeSql('SELECT * FROM contents', [], (_, { rows: { _array } }) => {
            this.setState({ dataSource: this.state.dataSource.cloneWithRows(_array)})
        });
      },
    );
  }
```

이 부분을 조금 살펴보면...

`componentWillMount`를 사용한 이유는 앱이 처음 켜졌을 때, 값을 읽어야 하기 때문이다.

`componentWillReceiveProps`를 사용한 이유는 글 작성화면에서 다시 돌아온 경우에 데이터를 다시 읽어서 뷰를 다시 랜더하기 위함이다.

이제는 각 row에 수정, 삭제 버튼을 넣어서 지우는 작업을 할 것이다.
