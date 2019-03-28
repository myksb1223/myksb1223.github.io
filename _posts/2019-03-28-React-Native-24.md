---
layout: post
title:  "리액트 네이티브(React Native) 사용기 24편 - Profile화면 구현하기 2편"
date:   2019-03-28 03:16:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Instagram 인스타그램
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 [Profile화면 구현하기 1편](https://myksb1223.github.io/reactnative/2019/03/24/React-Native-20.html)까지 구현한 부분에 조금 더 추가하여 Profile화면에서 컨텐츠가 로드되기 직전까지의 뷰를 구현하는 것을 써보려고 한다.

일단 [ListView로 인스타그램 Profile화면 구현하기](https://myksb1223.github.io/reactnative/2019/03/22/React-Native-19.html)에 설명하였듯 첫 번째 Row(index는 0번)가 바로 프로필 내용과 3개의 모드 버튼(그리드, 리스트, 내가 태그된 컨텐츠)까지 이다. 즉, ListView의 `renderRow` 쪽의 rowID가 0일 때의 구현이 바로 아래이다.

```javascript

// 게시물, 팔로워, 팔로잉 버튼 구현부분.
{% raw %}export class CountButton extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <TouchableOpacity
        style={styles.countButton}
        onPress={() => {
          this.props.moveToFirst();
          }}>
        <Text style={styles.countTextLarge}>
          {this.props.count}{'\n'}
          <Text style={styles.countTextSmall}>
            {this.props.type}
          </Text>
        </Text>
      </TouchableOpacity>
    );
  }
}

// 사용자 프로필의 정보부분 전까지 구현된 부분이다.
export class ProfieTopLayout extends React.Component {
  render() {
    return (
      <View style={{flex: 0.88, flexDirection: 'row'}}>
        <View style={{flex:1, justifyContent: 'center', alignItems: 'center'}}>
          <Image source={{ uri: global.currentUser["picture"] }} style={styles.profileImage}/>
        </View>

        <View style={{flex:3, flexDirection: 'column'}}>
          <View style={{flex: 1.5}} />
          <View style={{flex: 6.5, flexDirection: 'row'}}>
            <CountButton
              moveToFirst={() => this.props.moveToFirst()}
              count={88}
              type={"게시물"}/>
            <CountButton
              count={146}
              type={"팔로워"}/>
            <CountButton
              count={52}
              type={"팔로잉"}/>
          </View>
          <View style={{flex: 3.3, justifyContent: 'center'}}>
            <TouchableOpacity
              style={{flex: 1, justifyContent: 'center', alignItems: 'center', borderWidth: 1, borderRadius: 5, borderColor: 'lightgray'}}
              onPress={() => {
                this.props.updateProfile();
                }}>
              <Text style={{fontSize: 14, fontWeight: 'bold'}}>
                프로필 수정
              </Text>
            </TouchableOpacity>
          </View>
          <View style={{flex: 1.5}} />
        </View>
        <View style={{flex: 0.15}} />
      </View>
    );
  }
}

export class ProfileLayout extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      type: 0,
    };
  }

  renderIcon(type) {
    if(this.state.type === 0) {
      if(this.state.type === type) {
        return (
          <Image style={{width: 25, height: 25}}
           source={ require('./assets/grid_focused.png')}/>
        );
      }

      if(type === 1) {
        return (
          <Image style={{width: 25, height: 25}}
           source={ require('./assets/list.png')}/>
        );
      }
      else if(type === 2) {
        return (
          <Image style={{width: 25, height: 25}}
           source={ require('./assets/folder.png')}/>
        )
      }
    }
    else if(this.state.type === 1) {
      if(this.state.type === type) {
        return (
          <Image style={{width: 25, height: 25}}
           source={ require('./assets/list_focused.png')}/>
        );
      }

      if(type === 0) {
        return (
          <Image style={{width: 25, height: 25}}
           source={ require('./assets/grid.png')}/>
        );
      }
      else if(type === 2) {
        return (
          <Image style={{width: 25, height: 25}}
           source={ require('./assets/folder.png')}/>
        );
      }
    }
    else {
      if(this.state.type === type) {
        return (
          <Image style={{width: 25, height: 25}}
           source={ require('./assets/folder_focused.png')}/>
        );
      }

      if(type === 0) {
        return (
          <Image style={{width: 25, height: 25}}
           source={ require('./assets/grid.png')}/>
        );
      }
      else if(type === 1) {
        return (
          <Image style={{width: 25, height: 25}}
           source={ require('./assets/list.png')}/>
        );
      }
    }
  }

  render() {
    return (
      <View style={{flex: 1, flexDirection: 'column', marginTop: 14}}>
        <ProfieTopLayout
          moveToFirst={() => this.props.moveToFirst()}
          updateProfile={() => {this.props.updateProfile()}}/>
        <View style={{flex: 1, marginTop: 14, marginLeft: 14, marginRight: 14}} >
          <Text style={{ fontWeight: 'bold'}}>{global.currentUser["name"]}</Text>
          <Text>{global.currentUser["info"]}</Text>
        </View>
        <View style={{flex: 1, flexDirection: 'row', marginTop:14, paddingTop: 8, paddingBottom: 8, borderWidth: 0.5, borderColor: 'lightgray'}} >
          <TouchableOpacity
            style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
            onPress={() => {
              this.props.onSelectType(0);
              this.setState({type: 0});
              }}>
              {this.renderIcon(0)}
          </TouchableOpacity>
          <TouchableOpacity
            style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
            onPress={() => {
              this.props.onSelectType(1);
              this.setState({type: 1});
              }}>
              {this.renderIcon(1)}
          </TouchableOpacity>
          <TouchableOpacity
            style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}
            onPress={() => {
              this.props.onSelectType(2);
              this.setState({type: 2});
              }}>
              {this.renderIcon(2)}
          </TouchableOpacity>
        </View>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  countButton: {
    flex: 1,
    flexDirection: 'column',
    justifyContent: 'center',
    alignItems: 'center'
  },
  countTextLarge: {
    fontWeight: 'bold',
    textAlign: 'center',
    fontSize: 16
  },
  countTextSmall: {
    color: 'gray',
    fontSize: 10
  },
  profileImage: {
    width:70,
    height:70,
    borderRadius: 35
  }
});{% endraw %}
```

일단, 구조화를 조금 하다가... 시간도 부족하고 조금 더 생각해야할 부분이 있어서 그대로 구현한 부분이 많지만... 어쨋든 설명을 하자면...

`CountButton`에서 `moveToFirst`는 게시물 버튼을 클릭하면 Scroll이 자동적으로 컨텐츠만 보이도록 움직이는데 이를 위한 메소드이다. 해당 부분은 다음편이나 다다음편에 나올 것 같다. 어떤 구현인지 알고 싶으면 [여기](https://myksb1223.github.io/reactnative/2019/03/24/React-Native-20.html)를 참조하면 된다.

`ProfileLayout`에서 각 버튼을 누를 때, `onSelectType` 메소드가 호출되는데 이 부분은 읽은 데이터를 그리드(Grid)로 보여줄 지, 리스트(List)로 보여줄지에 대한 처리가 담겨있다. 이 부분은 다음편에 자세히 설명할 것이다.

나머지 부분은 충분히 쉽게 이해 할 수 있을 것으로 예상하기 때문에 생략한다.

다음은 리액트 네이티브(React Native), Expo Instagram 프로젝트에서 ProfileScreen에서의 컨텐츠 표현에 대해서 글을 작성할 것이다.

[Profile화면 구현하기 3편](https://myksb1223.github.io/reactnative/2019/03/29/React-Native-25.html) 보기

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
