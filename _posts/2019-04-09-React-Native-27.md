---
layout: post
title:  "리액트 네이티브(React Native) 사용기 27편 - 리스트뷰(ListView) refresh, load more"
date:   2019-04-09 04:28:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Instagram 인스타그램 리스트뷰 ListView
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 `ListView`에 `refresh`와 `load more` 기능을 구현하는 방법을 써보려고 한다.

인스타그램(Instagram) 따라 만들기에서 HomeScreen 부분에 적용시키면서 해볼 것이기에 [소스](https://github.com/myksb1223/ReactNative-instagram-example/blob/master/HomeScreen.js)를 참고하면 될 것 같다.

일단, `ListView`는 기본적으로 `ScrollView`를 기반으로 만들어 졌기때문에 `ScrollView`의 [옵션](https://docs.expo.io/versions/v32.0.0/react-native/scrollview/)도 잘 파악해야 한다. `ScrollView`의 옵션에는 `refreshControl`이라는 부분이 있다.

저 부분을 사용하면 새로고침은 바로 해결할 수 있다. [관련된 링크](https://docs.expo.io/versions/v32.0.0/react-native/refreshcontrol/)를 따라가면 예제가 나와 있기 때문에 쉽게 사용할 수 있다.

```javascript
export default class HomeScreen extends React.Component {
  // ...

  constructor() {
    super();


    this.state = {
      dataSource: ds.cloneWithRows([]),
      refreshing: false,
      datas: null,
      // ...
    };

  }

  read() {
    db.transaction(
      tx => {
        let query = 'SELECT * FROM contents ORDER BY id DESC LIMIT 18'

        tx.executeSql(query, [], (_, { rows: { _array } }) => {

          if(this.state.refreshing) {
            // alert("refreshing" + _array.length);
            this.setState({ dataSource: this.state.dataSource.cloneWithRows(_array), datas: _array, refreshing: false})
          }
          else {
            this.setState({ dataSource: this.state.dataSource.cloneWithRows(_array), datas: _array})
          }
        }

        );
      },
    );
  }

  // ...

  _onRefresh = () => {
    this.setState({refreshing: true}, () => this.read());
  }

  render() {

    // ...
    return (
      <ListView
        dataSource={this.state.dataSource}
        renderRow={(rowData, sectionID, rowID) =>
          // ...
        }
        enableEmptySections
        refreshControl={
          <RefreshControl
            refreshing={this.state.refreshing}
            onRefresh={this._onRefresh}
          />
        }
      />
    );
  }
}
```

위와 같이, `RefreshControl`에서 `onRefresh`가 호출될 때, `_onRefresh`메소드를 수행해주도록 하는 것이다.

`_onRefresh`메소드 내에서는 코드처럼 DB를 다시 읽은 후 다시 렌더해도 되고, 새롭게 하고 싶은 것을 해주면 된다.

이번에는 `load more`에 대한 구현을 해야하는데 이 부분은 `ListView`에 아주 좋은 옵션이 있다. `onEndReached`를 이용하면 된다.

일단, Native에서 개발해 본 경험이 있다면 알겠지만 `ListView`의 마지막에 `ActivityIndicator`를 넣을 때는 footer를 이용하는 것이 보통이다.

그러므로 `renderFooter` 역시 이용할 것이다.

```javascript
export default class HomeScreen extends React.Component {
  // ...

  constructor() {
    super();


    this.state = {
      dataSource: ds.cloneWithRows([]),
      refreshing: false,
      isLoadingMore: false,
      moreCount: 0,
      datas: null,
      // ...
    };

  }


  read() {
    db.transaction(
      tx => {
        let query = 'SELECT * FROM contents ORDER BY id DESC LIMIT ?'
        if(this.state.isLoadingMore) {
          query = 'SELECT * FROM contents WHERE id < ' + this.state.datas[this.state.datas.length-1]["id"] + ' ORDER BY id DESC LIMIT ?'
        }

        tx.executeSql(query, [(this.state.moreCount+1) * 18], (_, { rows: { _array } }) => {

          if(this.state.isLoadingMore) {
            let temp = this.state.datas
            _array = temp.concat(_array)
            this.setState({ dataSource: this.state.dataSource.cloneWithRows(_array), datas: _array, moreCount: this.state.moreCount+1})
          }
          else {
            if(this.state.refreshing) {
              // alert("refreshing" + _array.length);
              this.setState({ dataSource: this.state.dataSource.cloneWithRows(_array), datas: _array, refreshing: false})
            }
            else {
              this.setState({ dataSource: this.state.dataSource.cloneWithRows(_array), datas: _array})
            }
          }
        }

        );
      },
    );
  }


  // ...

  fetchMore() {
    if(this.state.datas.length >= 18*(this.state.moreCount+1)) {
      // alert("fetchMore")
      this.read();
    }
    else {
      this.setState({isLoadingMore: false});
    }
  }

  render() {

    // ...
    return (
      <ListView
        dataSource={this.state.dataSource}
        renderRow={(rowData, sectionID, rowID) =>
          // ...
        }
        enableEmptySections
        refreshControl={
          <RefreshControl
            refreshing={this.state.refreshing}
            onRefresh={this._onRefresh}
          />
        }
        onEndReachedThreshold={100}
        onEndReached={() =>
            this.setState({ isLoadingMore: true }, () => this.fetchMore())}
        renderFooter={() => {
          return (
            this.state.isLoadingMore &&
            <View style={{ flex: 1, padding: 10 }}>
              <ActivityIndicator size="small" />
            </View>
          );
        }}
      />
    );
  }
}
```

위와 같이 만들어 주면, 모든 것이 끝나는데 `onEndReachedThreshold`이 없으면 항상 `onEndReached`가 호출되는 현상을 볼 수 있다.

`onEndReachedThreshold`를 보면 알겠지만 `onEndReached`을 호출하기 위한 임계값이라고 설명되어 있다. 즉, 위의 코드에서는 100이기 때문에 bottom에서 100정도 더 스크롤 되었을 때, `onEndReached`을 호출한다는 뜻이다.

다음은 리액트 네이티브(React Native), Expo Instagram 프로젝트에서 `props`와 `state`의 차이에 대해 써보려고 한다. 드디어 둘의 차이를 체감한 것 같다.

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
