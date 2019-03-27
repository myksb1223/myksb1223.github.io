---
layout: post
title:  "리액트 네이티브(React Native) 사용기 16편 - 이미지 로테이션 하기"
date:   2019-03-18 20:58:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo 이미지 image 이미지회전 ImageRotation
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 [이전 글](https://myksb1223.github.io/reactnative/2019/03/15/React-Native-15.html)까지 구현한 부분에서 아이콘의 회전이 필요하여 이미지 회전에 관련된 부분에 대해 작성해 본다.

### 목표
[메시지 이미지](https://icons8.com/icons/set/chat)를 인스타그램과 같은 형태로 꼬리부분이 왼쪽이 아닌 오른쪽으로 돌리기.

### 시나리오
Row가 그려지기 전에 메시지 이미지를 회전시킨 후, 그 회전시킨 이미지를 뿌린다.

일단 이미지 회전에 관한 지식이 전무하기 때문에... [Document](https://docs.expo.io/versions/latest/sdk/imagemanipulator/#basic-example)를 참고한다.

일단 시키는대로 하면 당연히 원하는 결과가 나오지 않는다.

```Javascript
componentDidMount() {
  (async () => {
    const image = Asset.fromModule(require('../path/to/image.jpg'));
    await image.downloadAsync();
    this.setState({
      ready: true,
      image,
    });
  })();
}

_rotate90andFlip = async () => {
    const manipResult = await ImageManipulator.manipulateAsync(
      this.state.image.localUri || this.state.image.uri,
      [{ rotate: 90}, { flip: { vertical: true }}],
      { format: 'png' }
    );
    this.setState({ image: manipResult });
  }
```

위의 코드에서 `_rotate90andFlip`를 언제 호출해야할까가 고민이다.

하지만 간단하게 `componentDidMount`내의 `setState` 다음에 호출해주면 된다.

그러면 결과적으로 아래와 같이 구현할 수 있다.

```Javascript
componentDidMount() {
  (async () => {
    const image = Asset.fromModule(require('./assets/reply.png'));
    await image.downloadAsync();
    this.setState({
      ready: true,
      image,
    });
    this._rotate180andFlip();
  })();
}

_rotate180andFlip = async () => {
    const manipResult = await ImageManipulator.manipulateAsync(
      this.state.image.localUri || this.state.image.uri,
      [{ rotate: 180}, { flip: { vertical: true }}, {resize: {width: 50, height: 50}}],
      { format: 'png' }
    );

    this.setState({ image: manipResult });
}

render() {
  // ...
  {% raw %}return(
    <View style={{flex: 1, flexDirection: 'column', margin: 12}}>
      // ...
      <View style={{flex: 1, flexDirection: 'row', marginBottom: 12}}>
        <TouchableOpacity
          onPress={() => {

            }
          }>
          <Image style={{width: 25, height: 25}}
          resizeMode='contain'
           source={ require('./assets/heart.png')}/>
        </TouchableOpacity>
        <TouchableOpacity
          onPress={() => {
            }
          }>
          {this.state.ready && this._renderImage()}
        </TouchableOpacity>
        <TouchableOpacity
          onPress={() => {

            }
          }>
          <Image style={{width: 25, height: 25, marginLeft: 12}}
          resizeMode='contain'
           source={ require('./assets/share.png')}/>
        </TouchableOpacity>
      </View>
      // ...
    </View>
  )
}

_renderImage = () => {
    return (
      <Image
        source={{ uri: this.state.image.localUri || this.state.image.uri }}
        resizeMode= 'contain'
        style={{ width: 25, height: 25, marginLeft: 12 }}
        />
    );

};{% endraw %}
```

이렇게 하면 이미지가 내가 원하는대로 변해서 호출된다.

다음은 리액트 네이티브(React Native), Expo에서 tabBar 및 DrawerNavigator에 대해서 조금 더 알아보려고 한다.

[Navigator 조금 더 파헤치기](https://myksb1223.github.io/reactnative/2019/03/19/React-Native-17.html)

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
