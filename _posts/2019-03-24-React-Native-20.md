---
layout: post
title:  "리액트 네이티브(React Native) 사용기 20편 - Profile화면 구현하기 1편"
date:   2019-03-24 02:30:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Instagram 인스타그램
sitemap :
  changefreq : daily
  priority : 1.0
---

[Profile화면 구현하기 2편](https://myksb1223.github.io/reactnative/2019/03/28/React-Native-24.html) 보기.

리액트 네이티브(React Native), Expo로 [이전 글](https://myksb1223.github.io/reactnative/2019/03/22/React-Native-19.html)까지 구현한 부분에서 Profile화면에서 `ListView`에 관해서 조금 썼는데 아예 Profile화면의 구현을 순서대로 써 보려고 한다.

1. Top navigation bar의 구현.
2. Top navigation bar의 터치 구현.
3. Overlay화면 구현.

### 1. Top navigation bar 구현.

1. 일단 drop_down, drop_up 이미지를 구하도록 하자.
2. `CreateUserScreen`에서 사용자를 몇 명추가하도록 하자.
    혹시, `CreateUserScreen`이 궁금하면 [여기](https://github.com/myksb1223/ReactNative-instagram-example/blob/master/CreateUserScreen.js)에 가서 참고하면 된다.

위 두 부분이 완료되었으면 이제부터 [ProfileScreen](https://github.com/myksb1223/ReactNative-instagram-example/blob/master/ProfileScreen.js)의 top navigation bar를 구현하도록 할 것이다.

현재, navigation 구조가 궁금하다면 [여기](https://github.com/myksb1223/ReactNative-instagram-example/blob/master/App.js)를 참고하면 된다.

먼저, `App.json`에 `global.allUsers` 및 `global.currentUser`라는 변수를 생성하고 `SQLite`에서 읽어들인 값을 넣도록 한다.

**global.allUsers** : 현재 생성한 모든 사용자.
**global.currentUser** : 현재 선택된 사용자.(SQLite의 users 테이블에서 current값이 1인 사용자)

```Javascript
global.allUsers = null;
global.currentuser = null;

export default class App extends React.Component {
  constructor() {
    super();

    // ...
  }

  componentDidMount() {
      db.transaction(tx => {
        tx.executeSql(
          'CREATE TABLE IF NOT EXISTS contents (id INTEGER PRIMARY KEY AUTOINCREMENT, content TEXT, picture TEXT, user_id INTEGER, user_name TEXT, user_pic TEXT);'
        );
        tx.executeSql(
          'CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, info TEXT, picture TEXT, current INTEGER DEFAULT 0);'
        );

        tx.executeSql('SELECT * FROM users', [], (_, { rows: { _array } }) => {
            for(var i in _array) {
              if(1 === _array[i]["current"]) {
                global.currentUser = _array[i]
                break
              }
            }

            global.allUsers = _array

            // ...
        }

        );
      });
    }


  render() {
    return (
      <Container />
    );
  }
}
```

자, 여기까지 왔으면 이제 ProfileScreen으로 가보자.
navigation에서 `title` 옵션이 아닌, `headerTitle`을 사용하면 된다.

```Javascript
export class ProfileHeaderTitle extends React.Component {
  constructor() {
    super();

    this.state = { isPopUp: false, };
  }

  render() {
    return (
      {% raw %}<View
        style={{flex: 1, flexDirection: 'row', justifyContent: 'center', alignItems: 'center'}}>
          <Text style={{fontWeight: 'bold', fontSize: 16}}>{global.currentUser["name"]}</Text>
          {!this.state.isPopUp ? <Image style={{width: 15, height: 15}} source={ require('./assets/drop_down.png')}/> : <Image style={{width: 15, height: 15}} source={ require('./assets/drop_up.png')}/>}
      </View>{% endraw %}
    );
  }
}

export default class ProfileScreen extends React.Component {
  static navigationOptions = ({navigation}) => {
    return {
      headerTitle: <ProfileHeaderTitle />,
      headerTitleStyle: {textAlign:'center', alignSelf:'center',flex:1},
    }
  };

  // ...
}
```

이렇게 하면 인스타 그램의 top navigation bar에 사용자 이름과 아래쪽 화살표 방향의 형태가 구성된다.

### 2. Top navigation bar의 터치 구현.

이 부부은 `ProfileHeaderTitle` 부분에서 View를 `TouchableOpacity`로 변경하도록 하고, `onPress`를 구현해준다.

```Javascript
export class ProfileHeaderTitle extends React.Component {
  constructor() {
    super();

    this.state = { isPopUp: false, };
  }

  render() {
    return (
      {% raw %}<TouchableOpacity
        style={{flex: 1, flexDirection: 'row', justifyContent: 'center', alignItems: 'center'}}
        onPress={() => {
            // 사용자 목록 Overlay로 띄우기.
          }}>
          <Text style={{fontWeight: 'bold', fontSize: 16}}>{global.currentUser["name"]}</Text>
          {!this.state.isPopUp ? <Image style={{width: 15, height: 15}} source={ require('./assets/drop_down.png')}/> : <Image style={{width: 15, height: 15}} source={ require('./assets/drop_up.png')}/>}
      </TouchableOpacity>{% endraw %}
    );
  }
}
```

이렇게 해주면 끝이다. 이제 남은 부분은 `onPress`가 호출되었을 때, Overlay로 사용자 리스트가 뜨면 된다.

### 3. Overlay화면 구현.

자 먼저 `onPress`가 호출되었을 때, `ProfileScreen`의 `render` 부분에 영향을 주고, Overlay된 사용자 목록을 띄워야 한다.

즉, 상위 Component의 메소드를 호출해야 하기에 아래와 같이 구현한다.

```Javascript
export class ProfileHeaderTitle extends React.Component {
  constructor() {
    super();

    this.state = { isPopUp: false, };
  }

  updatePopup(isPopUp) {
    this.setState({isPopUp: isPopUp});
    this.props.onPopupView(isPopUp);
  }

  render() {
    return (
      {% raw %}<TouchableOpacity
        style={{flex: 1, flexDirection: 'row', justifyContent: 'center', alignItems: 'center'}}
        onPress={() => {
            this.updatePopup(!this.state.isPopUp)
          }}>
          <Text style={{fontWeight: 'bold', fontSize: 16}}>{global.currentUser["name"]}</Text>
          {!this.state.isPopUp ? <Image style={{width: 15, height: 15}} source={ require('./assets/drop_down.png')}/> : <Image style={{width: 15, height: 15}} source={ require('./assets/drop_up.png')}/>}
      </TouchableOpacity>{% endraw %}
    );
  }
}

let _this;

export default class ProfileScreen extends React.Component {
  static navigationOptions = ({navigation}) => {
    const {params = {}} = navigation.state;
    return {
      headerTitle: <ProfileHeaderTitle
                    ref={title => {
                      navigation.title = title;
                    }}
                    onPopupView={params.updatePopup}/>,
      headerTitleStyle: {textAlign:'center', alignSelf:'center',flex:1},
    }
  };

  constructor(props) {
    super(props);

    // ...
    this.state = {
      // ...
      dataSource: ds.cloneWithRows([""]),
      userDataSource: userDs.cloneWithRows(global.allUsers),
      popUp: false,
    };
    _this = this;
    this.props.navigation.setParams({
      updatePopup: this.updatePopup,
    });
  }

  updatePopup(popUp) {
    _this.setState({popUp: popUp});
  }

  render() {
    let {height, width} = Dimensions.get('window');
    return (
      <View>
        // ProfileScreen 전체화면 스크롤을 위한 리스트뷰
        <ListView
          dataSource={this.state.dataSource}
          renderRow={(rowData, sectionID, rowID) =>
            // rowID: 0 => 프로필 내용 화면
            // rowID: 1 ~ n => 그리드(Grid), 리스트(List) 등의 실제 컨텐츠 뷰들
            }

          enableEmptySections
        />
        // 이 부분이 Overlay 화면
        {% raw %}{this.state.popUp && <View style={{flex: 1, flexDirection: 'column', position: 'absolute', left: 0, top: 0, backgroundColor: 'rgba(1, 0, 0, 0.6)', width: width, height: height}}>
          <TouchableOpacity style={{flex: 1}}
          activeOpacity={1}
          onPress={() => {
            _this.props.navigation.title.updatePopup(false)
            }}>
            <ListView
              dataSource={this.state.userDataSource}
              renderRow={(rowData, sectionID, rowID) =>
                <View style={{flex: 1, backgroundColor: 'white', flexDirection: 'row', }}>
                  <View style={{flex: 1, backgroundColor: 'white', flexDirection: 'row', margin: 10}}>
                    <Image source={{ uri: rowData["picture"] }} style={{width: 40, height: 40, borderRadius: 20, borderWidth: 1, borderColor: 'black'}}/>
                    <View style={{flex: 1, justifyContent: 'center', marginLeft: 10}}>
                      <Text style={{fontWeight: 'bold'}}>{rowData["name"]}</Text>
                    </View>
                  </View>
                </View>
              }
              enableEmptySections
              scrollEnabled={false}
            />
          </TouchableOpacity>
        </View>}{% endraw %}
      </View>
    );
  }
}
```

코드가 길어서 복잡해 보이지만 핵심은 아래와 같다.

1. `ProfileHeaderTitle`가 클릭되면 `onPopupView`가 호출되고, 미리 설정해 놓은 `updatePopup`함수가 호출된다.
    ```Javascript
    _this = this;
    this.props.navigation.setParams({
      updatePopup: this.updatePopup,
    });
    ```
2. Overlay뷰를 터치했을 때, 해당 Overlay가 보이지 않아야 하기에 아래와 같은 `ref` 처리가 필요하다.
    ```Javascript
    headerTitle: <ProfileHeaderTitle
                  ref={title => {
                    navigation.title = title;
                  }}
                  onPopupView={params.updatePopup}/>
    그리고

    {% raw %}<TouchableOpacity style={{flex: 1}}
    activeOpacity={1}
    onPress={() => {
      _this.props.navigation.title.updatePopup(false)
      }}>{% endraw %}
    ```

이렇게 하면 충분히, Top navigation bar를 선택하면 사용자 목록이 보이고, Overlay뷰를 선택하면 사라지는 것을 구현할 수 있다. 당연히, Top navigation bar를 번갈아 선택해도 보였다 사라졌다 한다.

다음은 리액트 네이티브(React Native), Expo Instagram 프로젝트에서 ProfileScreen의 사용자 화면 레이아웃에 대해 글을 쓸 것이다.

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
