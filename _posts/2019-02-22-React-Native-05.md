---
layout: post
title:  "리액트 네이티브(React Native) 사용기 5편 - Camera 컴포넌트를 이용하기"
date:   2019-02-22 02:35:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 Expo Camera 카메라
sitemap :
  changefreq : daily
  priority : 1.0
---

[이전 글](https://myksb1223.github.io/reactnative/2019/02/21/React-Native-04.html)에서 리액트 네이티브(React Native), Expo에서의 퍼미션에 대해서 공부했으니 이제는 직접 사진을 찍어볼 차례이다.

일단, 생각해보면 기본적으로 어디든 마찬가지겠지만 경로만 알면된다. 즉, 사진을 찍고 해당 사진의 경로를 반환해주면 된다.

일단 Expo에서의 카메라에 대한 지식이 전무하니 [샘플 코드](https://docs.expo.io/versions/latest/sdk/camera/)를 보았다.

가져온 샘플에서 추가할 것은 일단 Flip밖에 없으니 실제로 찍은 후, 내 화면으로 돌아오는 버튼을 만들어야 한다. 예제의 코드에서 버튼 두개만 아래처럼 추가한다.

```Javascript
<TouchableOpacity
  style={{
    flex: 0.33,
    alignSelf: 'flex-end',
    alignItems: 'center',
  }}
  onPress={
    // To do snap.
  }>
  <Text
    style={{ fontSize: 18, marginBottom: 10, color: 'white' }}>
    {' '}Snap{' '}
  </Text>
</TouchableOpacity>
<TouchableOpacity
  style={{
    flex: 0.33,
    alignSelf: 'flex-end',
    alignItems: 'center',
  }}
  onPress={
    // To do done.
    }>
  <Text
    style={{ fontSize: 18, marginBottom: 10, color: 'white' }}>
    {' '}Done{' '}
  </Text>
</TouchableOpacity>
```

이제 `onPress`쪽만 구현해주면 된다.

일단 Done을 눌렀을 때는 그냥 이전 Screen으로 돌아오는데, 경로를 함께 넘겨주면 된다. 즉 아래처럼 코딩한다.

```Javascript
onPress={() => {
  // alert(this.state.path);
  this.props.navigation.navigate('Create', { image: this.state.path })
}}
```

이제 가장 중요한 부분인 사진 찍는 부분인데... 이 부분은 [여기](https://github.com/expo/camerja)를 참고했다.

```Javascript
snap = () => {
  if (this.camera) {
    this.camera.takePictureAsync({onPictureSaved: this.onPictureSaved});
  }
};

onPictureSaved = async photo => {
  let path = `${FileSystem.documentDirectory}photos/${Date.now()}.jpg`;
  await FileSystem.moveAsync({
    from: photo.uri,
    to: path,
  });
  this.setState({ path: path });
}
```

위와 같이 코딩하면 분명히 `moveAsync`에서 해당 path로 이동시키지 못했다는 에러가 난다. 그러므로 아래처럼 반드시 해당 디렉토리를 만들어 주도록 한다.

```Javascript
async componentDidMount() {
  try {
    await FileSystem.makeDirectoryAsync(
      `${FileSystem.documentDirectory}photos`,
      {
        intermediates: true,
      }
    )
  } catch (e) {
    console.log(e)
  }
}
```

그리고 가장 생소했던 코드가 아래의 코드였다.

```Javascript
<Camera
  ref={ref => {
    this.camera = ref;
    }}
    // ...
</Camera>
```

`ref`의 사용을 전혀 몰라서 찾아보았다. [여기](https://velopert.com/1148)로 가면 자세한 설명이 있다.

다음은 리액트 네이티브(React Native), Expo에서 카메라, 갤러리에서 넘어오는 경로를 같은 `state`에 저장하는 것과 `Component Life Cycle`에 대해서 공부를 할 차례이다.
