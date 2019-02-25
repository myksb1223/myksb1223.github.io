---
layout: post
title:  "리액트 네이티브(React Native) 사용기 8편 - SQLite를 이용한 데이터 저장"
date:   2019-02-26 02:36:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 Expo SQLite
sitemap :
  changefreq : daily
  priority : 1.0
---

카메라, 갤러리에서 이미지를 가져오는 방법을 알았으니 이제는 해당 해당 데이터를 저장할 차례이다.

일단, 인스타그램의 비슷한 구조로 만들 것이기에 몇가지 필드가 필요하다.

### User 테이블
    id : INTEGER // AUTOINCREMENT
    name : TEXT
    picture : TEXT // image path

### Content 테이블
    id : INTEGER // AUTOINCREMENT
    content : TEXT
    picture : TEXT // image path
    user_id : INTEGER // User 테이블의 id
    user_name : TEXT // User 테이블의 name
    user_pic : TEXT // User 테이블의 picture


일단, Expo SQLite 데이터베이스 사용을 전혀 모르기 때문에 Document의 [샘플 코드](https://github.com/expo/sqlite-example/blob/master/App.js)를 참고했다.

```Javascript
import { SQLite } from 'expo';

const db = SQLite.openDatabase('db.db');

export default class App extends React.Component {
  // ...

  componentDidMount() {
      db.transaction(tx => {
        tx.executeSql(
          'CREATE TABLE IF NOT EXISTS contents (id INTEGER PRIMARY KEY AUTOINCREMENT, content TEXT, picture TEXT, user_id INTEGER, user_name TEXT, user_pic TEXT);'
        );
        tx.executeSql(
          'CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, picture TEXT);'
        );
      });
  }

  // ...
}
```

`componentDidMount`에서 먼저 table을 생성한다. 그리고 난 후,  이제 실제 데이터를 입력하는 화면에서는 아래와 같이 구현하면 된다.

Navigation을 사용했기 때문에 아래와 같이 표현할 수 있다.

```Javascript
import { SQLite } from 'expo';

const db = SQLite.openDatabase('db.db');
let _this;

export default class CreateScreen extends React.Component {
  // ...
  static navigationOptions = ({navigation}) => {
    const {params = {}} = navigation.state;
    return {
      title: 'Write',
      headerRight: (
        <Button
          onPress={() => {
            params.done();
            navigation.navigate('Home', {added: 1})
          }}
          title="Done"
        />
      ),
    }
  };

  constructor(props) {
    super(props);
    this.state = {text: '', height: 0, users: null, user: null, loading: true};
    _this = this;
    this.props.navigation.setParams({
      done: this.add,
    });
  }

  add() {
    db.transaction(
      tx => {
        tx.executeSql('INSERT INTO contents (content, picture, user_id, user_name, user_pic) values (?, ?, ?, ?, ?)', [_this.state.text, _this.image.state.image, _this.state.user.id, _this.state.user.name, _this.state.user.picture]);
      },
    );
  }

  // ...
}
```

Image관련 처리를 ImageProcess라는 Component로 분리했기 때문에 해당 컴포넌트를 ref로 직접 접근하는 방법을 사용했다.
```Javascript
<ImageProcess
  ref={image => {
    this.image = image;
  }}
  // ...
/>
```

navigationOptions에서 `this.add`처럼 사용할 수 없다. 그렇기에 아래와 같이 해당 함수를 `param`으로 등록하는 편법을 이용하였다.
```Javascript
this.props.navigation.setParams({
  done: this.add,
});
```

이렇게 하면, `add`함수에서 역시 `this`를 사용할 수 없다. 그렇기에 `_this`라는 변수를 전역에 선언한 후에 `constructor`에서 `_this = this`와 같이 사용하였다. 이 부부은 [여기](https://github.com/react-navigation/react-navigation/issues/1468#issuecomment-301282017)를 참고하면 알 수 있다.

navigationOptions에서 버튼이 눌렸을 때, `navigation.navigate('Home', {added: 1})` 부분에서 `param`을 등록해준 이유는 Home이라는 화면으로 돌아갔을 때, `componentWillReceiveProps`를 호출하기 위해서다.

`componentWillReceiveProps`이 호출되어야 `ListView`에 표현할 데이터를 다시 부를 수 있다.

`Picker`, `ListView` 그리고 `SQLite`에서의 삭제 및 수정에 관한 글을 순서대로 올릴 것이다.
