---
layout: post
title:  "리액트 네이티브(React Native) 사용기 25편 - Profile화면 구현하기 3편"
date:   2019-03-29 03:16:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Instagram 인스타그램
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 [Profile화면 구현하기 2편](https://myksb1223.github.io/reactnative/2019/03/28/React-Native-24.html)까지 구현한 부분에서 드디어 실제로 컨텐츠를 표현해 보는 부분에 대해서 작성해 보려고 한다.

일단 [ListView로 인스타그램 Profile화면 구현하기](https://myksb1223.github.io/reactnative/2019/03/22/React-Native-19.html)를 읽지 않았다면 꼭 읽고 나서 이 글을 보면 이해가 쉽다.

### 형태
1. Row 0번은 프로필 상세 내용 및 버튼들을 위한 뷰.
2. Row 1~n까지가 실제로 내가 올린 글이 나오는 부분이다.

### 그리드(Grid) 형태의 표현

컨텐츠 3개가 하나의 Row가 되는 형태이다. 그러면 총 4가지 경우가 나온다.

- 0개인 경우 => 아예 Row가 없는 것이므로 생각할 필요 없다.
- 1개인 경우 => Image 하나와 View 2개.
- 2개인 경우 => Image 두개와 View 1개.
- 3개인 경우 => Image 세개.

```javascript
{% raw %}renderGridContent(rowData, rowID) {
  let {height, width} = Dimensions.get('window');
  let cellWidth = width / 3;

  if(rowData.length === 1) {
    return (
      <View style={{flex: 1, flexDirection: 'row'}}>
        <TouchableOpacity
          style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
          onPress={() => {
            }}>
            <Image style={{width: cellWidth, height: cellWidth}}
             source={{ uri: rowData[0].picture }}/>
        </TouchableOpacity>
        <View style={{flex: 1}} />
        <View style={{flex: 1}} />
      </View>
    );
  }
  else if(rowData.length === 2) {
    return (
      <View style={{flex: 1, flexDirection: 'row'}}>
        <TouchableOpacity
          style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
          onPress={() => {
            }}>
            <Image style={{width: cellWidth, height: cellWidth}}
             source={{ uri: rowData[0].picture }}/>
        </TouchableOpacity>
        <TouchableOpacity
          style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
          onPress={() => {
            }}>
            <Image style={{width: cellWidth, height: cellWidth}}
             source={{ uri: rowData[1].picture }}/>
        </TouchableOpacity>
        <View style={{flex: 1}} />
      </View>
    );
  }
  else {
    return (
      <View style={{flex: 1, flexDirection: 'row'}}>
        <TouchableOpacity
          style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
          onPress={() => {
            }}>
            <Image style={{width: cellWidth, height: cellWidth}}
             source={{ uri: rowData[0].picture }}/>
        </TouchableOpacity>
        <TouchableOpacity
          style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
          onPress={() => {
            }}>
            <Image style={{width: cellWidth, height: cellWidth}}
             source={{ uri: rowData[1].picture }}/>
        </TouchableOpacity>
        <TouchableOpacity
          style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
          onPress={() => {
            }}>
            <Image style={{width: cellWidth, height: cellWidth}}
             source={{ uri: rowData[2].picture }}/>
        </TouchableOpacity>
      </View>
    );
  }
}{% endraw %}
```

### 리스트(List) 형태의 표현

리스트 형태의 표현은 HomeScreen에서 컨텐츠를 표현했던 방법과 똑같이 그대로 표현하면 된다.

```javascript
{% raw %}render() {
  let {height, width} = Dimensions.get('window');
  let cellWidth = width / 3;
  // ...
  return(
    <ContentRow
      // ...
      caller= {this}
      rowData= {this.props.rowData}
      sectionID= {this.props.sectionID}
      rowID= {this.props.rowID} />
  );

  // ...
}{% endraw %}
```

이전에 구현했던 프로필 상세페이지의 코드와 종합해서 보면 아래와 같다.

```javascript
export class ProfileRow extends React.Component {
  constructor(props) {
    super(props);
  }

  renderGridContent(rowData, rowID) {
    let {height, width} = Dimensions.get('window');
    let cellWidth = width / 3;

    if(rowData.length === 1) {
      return (
        <View style={{flex: 1, flexDirection: 'row'}} onLayout={(event) => this.onLayout(event, rowID)}>
          <TouchableOpacity
            style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
            onPress={() => {
              this.props.goToContentScreen(rowData[0])
              }}>
              <Image style={{width: cellWidth, height: cellWidth}}
               source={{ uri: rowData[0].picture }}/>
          </TouchableOpacity>
          <View style={{flex: 1}} />
          <View style={{flex: 1}} />
        </View>
      );
    }
    else if(rowData.length === 2) {
      return (
        <View style={{flex: 1, flexDirection: 'row'}} onLayout={(event) => this.onLayout(event, rowID)}>
          <TouchableOpacity
            style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
            onPress={() => {
              this.props.goToContentScreen(rowData[0])
              }}>
              <Image style={{width: cellWidth, height: cellWidth}}
               source={{ uri: rowData[0].picture }}/>
          </TouchableOpacity>
          <TouchableOpacity
            style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
            onPress={() => {
              this.props.goToContentScreen(rowData[1])
              }}>
              <Image style={{width: cellWidth, height: cellWidth}}
               source={{ uri: rowData[1].picture }}/>
          </TouchableOpacity>
          <View style={{flex: 1}} />
        </View>
      );
    }
    else {
      return (
        <View style={{flex: 1, flexDirection: 'row'}} onLayout={(event) => this.onLayout(event, rowID)}>
          <TouchableOpacity
            style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
            onPress={() => {
              this.props.goToContentScreen(rowData[0])
              }}>
              <Image style={{width: cellWidth, height: cellWidth}}
               source={{ uri: rowData[0].picture }}/>
          </TouchableOpacity>
          <TouchableOpacity
            style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
            onPress={() => {
              this.props.goToContentScreen(rowData[1])
              }}>
              <Image style={{width: cellWidth, height: cellWidth}}
               source={{ uri: rowData[1].picture }}/>
          </TouchableOpacity>
          <TouchableOpacity
            style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
            onPress={() => {
              this.props.goToContentScreen(rowData[2])
              }}>
              <Image style={{width: cellWidth, height: cellWidth}}
               source={{ uri: rowData[2].picture }}/>
          </TouchableOpacity>
        </View>
      );
    }
  }

  render() {
    let {height, width} = Dimensions.get('window');
    let cellWidth = width / 3;
    if(this.props.rowID === "0") {
      return(
        // 이전 편 글에 구현된 내용...
        <ProfileLayout
          this.props.onSelectType(type)}
          />
      );
    }
    else {
      if(this.props.type === 0) {
        return(
          this.renderGridContent(this.props.rowData, this.props.rowID)
        );
      }
      else if(this.props.type === 1) {
        return(
          <ContentRow
            caller= {this}
            rowData= {this.props.rowData}
            sectionID= {this.props.sectionID}
            rowID= {this.props.rowID} />
        );
      }
      else {
        return(
          <View />
        );
      }
    }
  }
}

const db = SQLite.openDatabase('db.db');

let ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});

export default class ProfileScreen extends React.Component {
  // ... 네비게이션 옵션 설정.

  componentWillMount() {
    this.read();
  }

  constructor(props) {
    super(props);
    this.state = {
      dataSource: ds.cloneWithRows([""]),
      type: 0,
    };
    global.profileScreen = this;
  }

  read() {
    let map = new Array();
    db.transaction(
      tx => {
        tx.executeSql('SELECT * FROM contents WHERE user_id = ? ORDER BY id DESC', [global.currentUser["id"]], (_, { rows: { _array } }) => {
          let j = -1;
          for(var i in _array) {
            if(i % 3 === 0) {
              let subMap = new Array();
              j = j + 1;
              map[j] = subMap;
            }
            if(_array[i]["picture"] === null) {
              _array[i]["picture"] = "https://www.instagram.com/p/BkxfIN4jpeO/media/?size=m";
              break
            }
            map[j][i%3] = _array[i];
          }

          map.unshift(global.currentUser)
          _array.unshift(global.currentUser)
          global.contents["map"] = map;
          global.contents["list"] = _array;
            this.setState({ dataSource: this.state.dataSource.cloneWithRows(map),})
        }

        );
      },
    );
  }

  updateType(type) {
    if(type === 0) {
      this.setState({type: type, dataSource: ds.cloneWithRows(global.contents["map"])})
    }
    else if(type === 1) {
      this.setState({type: type, dataSource: ds.cloneWithRows(global.contents["list"])})
    }
    else {
      this.setState({type: type, dataSource: ds.cloneWithRows([""])})
    }
  }

  //리스트 뷰의 renderRow 내에서는 {}를 사용하지 않는다.
  render() {
    let {height, width} = Dimensions.get('window');
    return (
      <View>
        <ListView
          dataSource={this.state.dataSource}
          renderRow={(rowData, sectionID, rowID) =>
            <ProfileRow
              rowData={rowData}
              sectionID={sectionID}
              rowID={rowID}
              type={this.state.type}
              onSelectType={(type) => this.updateType(type)}/>
            }
          enableEmptySections
        />
        // ... overlay 뷰.
      </View>
    );
  }
}
```

#### 설명 :
- 그리드(Grid) 모드 버튼과 리스트(List) 모드 버튼을 클릭했을 때 해당 값을 저장하고 있기 위해서 `state`에 type이 필요.
- `type={this.state.type}`는 `renderRow`쪽에 따로 구현한 `ProfileRow`컴포넌트에 해당 값을 보내주어야 현재 어떤 모드로 부를지 판단하기 위한 부분.
- `onSelectType={(type) => this.updateType(type)}`는 버튼을 클릭할 때마다 type을 바꿔주어야 하기 때문에 `onSelectType`라는 부분 따로 만들어 `ProfileRow`가 클릭될 때 `updateType`가 실행되어 `this.state.type`을 업데이트하고 렌더를 다시 하게끔 한다.

아직 미숙한 부분도 있기에 조금 정리되지 않은 느낌이 들지만 흐름도는 이해하고 있는 것으로 만족한다.

다음은 리액트 네이티브(React Native), Expo Instagram 프로젝트에서 HomeScreen에서 글을 작성했을 때, 해당 부분이 ProfileScreen에 바로 적용되어 컨텐츠가 증가되서 보이는 부분에 대해서 글을 작성할 것이다.

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
