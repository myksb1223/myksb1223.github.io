---
layout: post
title:  "리액트 네이티브(React Native) 사용기 6편 - 카메라, 갤러리에서 이미지 가져오기"
date:   2019-02-23 22:27:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 Expo 이미지 Image
sitemap :
  changefreq : daily
  priority : 1.0
---

[카메라, 갤러리 퍼미션](https://myksb1223.github.io/reactnative/2019/02/21/React-Native-04.html)
[카메라 사진 찍기](https://myksb1223.github.io/reactnative/2019/02/22/React-Native-05.html)

위의 두 글을 종합하면 일단 갤러리를 띄울 수 있고, 카메라 화면을 띄울 수 있다. 남은 것은 해당 두 과정을 통합하는 작업이 필요하다.

일단, 카메라에서 사진 찍은 후 경로를 받는 과정과 갤러리에서 가져온 이미지 경로를 받는 과정이 다르다. 하지만 나는 같은 `Image`뷰에 띄우는 것을 목표로 하기에 다른 곳에 넘어온 경로를 `state`에 저장해야 했다.

갤러리에서 받아온 경로를 `state`에 넣는 것은 앞에 써놓은 첫 링크를 보면 바로 나오기 때문에 카메라에 대한 경로를 저장하는 것만 구현하면 된다.

### 1. 카메라 경로 받아오기.

카메라에서 경로를 받아올 때는 `navigation`의 `param`을 이용한다.

- Camera 호출 화면 -> Camera
- Camera -> Carmera 호출 화면(이 때, `param`에 경로를 넘긴다.)

화면이 다시 호출될 때, `param`을 건드려야 하기 때문에 `Component Life Cycle`을 이용하면 된다.

```Javascript
componentWillReceiveProps(nextProps){
  if(this.props === nextProps){
    return;
  }
  // TODO perform changes on state change

  this.setState({image: nextProps.navigation.getParam('image', null)});
}
```

즉, 화면이 다시 호출되었을 때, `props`가 변했으면 업데이트 해주는 것이다.

### 2. 코드 통합.

```Javascript
import React from 'react';
import { StyleSheet, View, Button, Alert, Image } from 'react-native';
import { ImagePicker, Permissions, Camera } from 'expo';
import CameraScreen from './CameraScreen';

export default class CreateScreen extends React.Component {
  static navigationOptions = ({navigation}) => {
    return {
      title: 'Write',
      headerRight: (
        <Button
          onPress={() =>
            navigation.navigate('Home')
          }
          title="Done"
        />
      ),
    }
  };

  constructor(props) {
    super(props);
    this.state = {status: false, image: null};
  }

  componentWillReceiveProps(nextProps){
    if(this.props === nextProps){
      return;
    }
    // TODO perform changes on state change

    this.setState({image: nextProps.navigation.getParam('image', null)});
  }

  getPermissionStatus(type, p_type) {
    const { Permissions } = Expo;
    if(p_type == 0) {
      if(type == 0) {
        return Permissions.getAsync(Permissions.CAMERA);
      }
      else {
        return Permissions.getAsync(Permissions.CAMERA_ROLL);
      }
    }
    else {
      if(type == 0) {
        return Permissions.askAsync(Permissions.CAMERA);
      }
      else {
        return Permissions.askAsync(Permissions.CAMERA_ROLL);
      }
    }
  }

  async checkPermisson(type) {
    const { status } = await this.getPermissionStatus(type, 0);
    this.setState({ status: status === 'granted' });
    if (status !== 'granted') {
      const { status } = await this.getPermissionStatus(type, 1);
      this.setState({ status: status === 'granted' });
    }

    if (this.state.status) {
      if(type == 0) {
        this.props.navigation.navigate('Camera', { name: 'Jane' })
      }
      else {
        this.pickImage();
      }
    }
  }

  async pickImage() {
    let result = await ImagePicker.launchImageLibraryAsync({
      allowsEditing: true,
      aspect: [4, 3],
    });

    // alert(result.uri);

    if (!result.cancelled) {
      this.setState({ image: result.uri });
    }
  }

  showAlert() {
    Alert.alert(
      null,
      'Select menu',
      [
        {text: 'Camera', onPress: () => this.checkPermisson(0)},
        {text: 'Gallery', onPress: () => this.checkPermisson(1)},
      ],
      { cancelable: true }
    )
  }

  render() {
    // let { navigate } = this.props.navigation;
    return (
      <View style={styles.container}>
          <Button
            title= "Select image"
            onPress={() => {
                this.showAlert();
              }
            }
          />


          <Image source={{ uri: this.state.image }} style={{ width: 200, height: 200 }} />
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    // alignItems: 'center',
    // justifyContent: 'center',
  },
});

```

다음은 `Component Life Cycle`에 대해 조금 더 공부할 차례인 것 같다.
